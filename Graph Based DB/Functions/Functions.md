# [[초기화 관련 함수]]

# [[Channel 관련 함수]]

# [[Axis 관련 함수]]

# [[Command 관련 함수]]
# [[Vertex 관련 함수]]
# [[Cycle 관련 함수]]

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

# `save_DB`
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
- 참조 함수: [[Functions#]]
```c
void load_DB() {
    Core = (uchar**)malloc(MaxCoreSize * sizeof(uchar*));
    init_core_mapping();
    // Initially load first MaxCoreSize nodes
    for (int i = 0; i < MaxCoreSize && i < 256; i++) {
        load_node_to_core(i);
    }
    printf("Database loaded successfully\n");
    }
```

