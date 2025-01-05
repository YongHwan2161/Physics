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