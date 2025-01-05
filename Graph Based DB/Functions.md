# `create_new_node`
-  새로운 node를 생성하는 함수이다. 새로운 노드에 기록되는 값은 `initValues` 변수를 참조한다. [[Variables#`initValues`|initValues]]
```c
void create_new_node(int index) {
    uchar* newNode = (uchar*)malloc(12 * sizeof(uchar));
    for (int i = 0; i < 12; ++i) {
        newNode[i] = initValues[i];
    }
    Core[index] = newNode;
}
```

# `create_DB`
- 새로운 database를 생성하는 함수이다. [[Functions#`create_new_node`|create_new_node]] 함수를 참조한다. 
```c
void create_DB() {
    printf("call create_DB()\n");
    Core = (uchar**)malloc(256 * sizeof(uchar*));
    for (int i = 0; i < 256; ++i) {
        create_new_node(i);
    }
}
```

# `save_node_to_file`
- save_node_to_file 함수는 개별 node를 binary file에 저장하는 함수이다:
  1. 현재 data file의 위치(offset)를 구한다
  2. 이 offset을 map file에 저장한다
  3. node의 실제 데이터를 data file에 저장한다
```c
void save_node_to_file(FILE* data_file, FILE* map_file, int index) {
    uchar* node = Core[index];
    long offset = ftell(data_file);  // Get current position in data file
    // Write offset to map file
    fwrite(&offset, sizeof(long), 1, map_file);
    // Write node data to data file
    uint node_size = *(uint*)node;  // First 4 bytes contain size
    fwrite(node, sizeof(uchar), node_size + 4, data_file);  // +4 to include size field
}
```

# save_DB
- save_DB 함수는 전체 데이터베이스를 저장하는 함수이다:
  1. data.bin과 map.bin 파일을 생성한다
  2. map.bin 파일의 시작에 전체 node 개수를 저장한다
  3. 각 node를 순차적으로 저장한다
  4. 저장이 완료되면 파일들을 닫는다
```c
void save_DB() {
    FILE* data_file = fopen("binary-data/data.bin", "wb");
    FILE* map_file = fopen("binary-data/data.bin", "wb");
    if (!data_file || !map_file) {
        printf("Error opening files for writing\n");
        return;
    }
    // Write number of nodes at the start of map file
    uint num_nodes = 256;
    fwrite(&num_nodes, sizeof(uint), 1, map_file);
    // Save each node
    for (int i = 0; i < 256; i++) {
        save_node_to_file(data_file, map_file, i);
    }
    fclose(data_file);
    fclose(map_file);
}
```

# `check_and_init_DB`
- 프로그램이 시작될 때 다음과 같은 순서로 데이터베이스를 초기화한다:
  1. binary-data 디렉토리에 data.bin과 map.bin 파일이 존재하는지 확인한다.
  2. 파일이 존재하지 않으면 새로운 데이터베이스를 생성하고 저장한다.
  3. 파일이 존재하면 기존 데이터베이스를 사용한다.
 - 참조 함수: [[Functions#`create_DB`|create_DB]], [[Functions#save_DB|save_DB]], [[Functions#`load_DB`|load_DB]]
```c
int check_and_init_DB() {
    FILE* data_file = fopen(DATA_FILE, "rb");
    FILE* map_file = fopen(MAP_FILE, "rb");
    if (!data_file || !map_file) {
        if (data_file) fclose(data_file);
        if (map_file) fclose(map_file);
        // Files don't exist, create new database
        create_DB();
        save_DB();
        return 1;  // New database created
    }
    fclose(data_file);
    fclose(map_file);
    // Load existing database
    load_DB();
    return 0;  // Existing database loaded
}
```

# `load_DB`
- load_DB 함수는 전체 데이터베이스를 로딩하는 함수이다:
  1. binary file들을 연다
  2. 전체 node 개수를 읽는다
  3. Core 배열을 할당한다
  4. 각 node의 offset을 읽고 해당 node를 로딩한다
  5. 파일들을 닫는다
- 참조 함수: [[Functions#`load_node_from_file`|load_node_from_file]]
```c
void load_DB() {
    FILE* data_file = fopen(DATA_FILE, "rb");
    FILE* map_file = fopen(MAP_FILE, "rb");
    if (!data_file || !map_file) {
        printf("Error opening files for reading\n");
        return;
    }
    // Read number of nodes
    uint num_nodes;
    fread(&num_nodes, sizeof(uint), 1, map_file);
    // Allocate Core array
    Core = (uchar**)malloc(num_nodes * sizeof(uchar*));
    // Read each node's offset and load the node
    for (int i = 0; i < num_nodes; i++) {
        long offset;
        fread(&offset, sizeof(long), 1, map_file);
        load_node_from_file(data_file, offset, i);
    }
    fclose(data_file);
    fclose(map_file);
}
```

# `load_node_from_file`
- load_node_from_file 함수는 개별 node를 binary file에서 읽어오는 함수이다:
  1. 주어진 offset 위치로 이동한다
  2. node size를 읽어온다
  3. 필요한 메모리를 할당한다
  4. 전체 node 데이터를 읽어온다
  5. Core 배열의 해당 index 위치에 저장한다
```c
void load_node_from_file(FILE* data_file, long offset, int index) {
    // Move to the correct position in data file
    fseek(data_file, offset, SEEK_SET);
    // Read size first
    uint node_size;
    fread(&node_size, sizeof(uint), 1, data_file);
    // Allocate memory for the node
    uchar* newNode = (uchar*)malloc((node_size + 4) * sizeof(uchar));
    // Move back to read the whole node including size
    fseek(data_file, offset, SEEK_SET);
    fread(newNode, sizeof(uchar), node_size + 4, data_file);
    // Store in Core
    Core[index] = newNode;
}
```