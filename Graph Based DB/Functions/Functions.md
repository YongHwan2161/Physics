# [[초기화 관련 함수]]

# [[Channel 관련 함수]]

# [[Axis 관련 함수]]

# [[Command 관련 함수]]

# `create_new_node`
-  새로운 node를 생성하는 함수이다. 새로운 노드에 기록되는 값은 `initValues` 변수를 참조한다. [[Variables#`initValues`|initValues]]
- 16 바이트의 memory 공간을 할당하고 이 공간을 가리키는 포인터인 `newNode`를 `Core[index]`에 저장한다. 
```c
void create_new_node(int index) {
    uchar* newNode = (uchar*)malloc(16 * sizeof(uchar));
    for (int i = 0; i < 16; ++i) {
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

# `load_node_to_core`
- binary file에 저장되어 있는 node data를 RAM에 올리는 함수이다. 
- offset data는 [[Variables#`CoreMap`|CoreMap]]을 참조해서 얻는다. 
- 참조: [[Functions#`load_node_from_file`|load_node_from_file]]
```c
int load_node_to_core(int node_index) {
    if (CoreSize >= MaxCoreSize) {
        // Need to unload something first
        for (int i = 0; i < 256; i++) {
            if (CoreMap[i].is_loaded) {
                unload_node_from_core(i);
                break;
            }
        }
    }
  
    FILE* data_file = fopen(DATA_FILE, "rb");
    if (!data_file) return -1;
    // Use stored offset directly
    long offset = CoreMap[node_index].file_offset;
    // Load the node
    CoreMap[node_index].core_position = CoreSize;
    CoreMap[node_index].is_loaded = 1;
    load_node_from_file(data_file, offset, CoreSize);
    CoreSize++;
    fclose(data_file);
    return CoreMap[node_index].core_position;
}
```
# `load_node_from_file`
- load_node_from_file 함수는 개별 node를 binary file에서 읽어오는 함수이다:
  1. 주어진 offset 위치로 이동한다
  2. node size를 읽어온다(2 바이트)
  3. 필요한 메모리를 할당한다
  4. 다시 원리 offset 위치로 이동한다(2 바이트 앞으로)
  5. 전체 node 데이터를 읽어온다
  6. Core 배열의 해당 index 위치에 저장한다
```c
void load_node_from_file(FILE* data_file, long offset, int index) {
    fseek(data_file, offset, SEEK_SET);
    // Read size power first (2 bytes)
    ushort size_power;
    fread(&size_power, sizeof(ushort), 1, data_file);
    // Calculate actual size
    uint actual_size = 1 << size_power;
    // Allocate memory for the node
    uchar* newNode = (uchar*)malloc(actual_size * sizeof(uchar));
    // Move back 2 bytes instead of seeking from start
    fseek(data_file, -2, SEEK_CUR);
    fread(newNode, sizeof(uchar), actual_size, data_file);
    Core[index] = newNode;
}
```


# create_axis
- 인자로 `node_index`, `channel_index`, `axis_number`를 받는다. 
- Core에 해당 `node_index`가 존재하지 않으면 `AXIS_ERROR`를 반환한다. 
- create_axis 함수에서는 인자로 받은 axis_number가 현재 node,channel data에 존재하는지 확인한 후 존재하면 새로 axis를 생성할 필요가 없고, 존재하지 않는 경우에만 새로 axis를 생성해야 한다. 
- `channel_index`의 offset을 구하고 offset이 0보다 작으면 역시 error를 반환한다. 채널의 offset을 구하는 함수는 [[Channel 관련 함수#get_channel_offset|get_channel_offset]] 참조.
- 주어진 node, ch에 대해 해당 axis가 이미 존재하는지 확인해 주는 함수는 [[Axis 관련 함수#has_axis|has_axis]] 참조.
- 
```c
int create_axis(int node_index, int channel_index, int axis_number) {
    if (!Core[node_index]) {
        printf("Error: Invalid node index\n");
        return AXIS_ERROR;
    }
    uchar* node = Core[node_index];
    uint channel_offset = get_channel_offset(node, channel_index);
    // Check if axis already exists
    if (has_axis(node, channel_offset, axis_number)) {
        printf("Warning: Axis %d already exists in node %d, channel %d\n",
               axis_number, node_index, channel_index);
        return AXIS_SUCCESS;  // Not an error, but no new axis created
    }
    // Get current axis count
    ushort* axis_count = (ushort*)(node + channel_offset);
    ushort current_axis_count = *axis_count;
    // Calculate new sizes
    ushort old_channel_size = get_channel_size(node, channel_index);
    ushort new_channel_size = old_channel_size + 6;  // Add 6 bytes for new axis
    // Check if we need to resize the node
    ushort node_size_power = *(ushort*)node;
    uint current_node_size = 1 << node_size_power;
    uint required_size = channel_offset + new_channel_size;
    // If required size is larger than current size, resize node
    if (required_size > current_node_size) {
        // Find next power of 2
        uint new_size = current_node_size;
        while (new_size < required_size) {
            new_size *= 2;
            node_size_power++;
        }
        // Allocate new space
        uchar* new_node = (uchar*)malloc(new_size);
        memcpy(new_node, node, current_node_size);
        free(node);
        node = new_node;
        Core[node_index] = new_node;
        // Update node size
        *(ushort*)node = node_size_power;
    }
    // Update axis count
    (*axis_count)++;
    // Calculate offset for new axis data
    uint axis_data_offset = channel_offset + 2 + (current_axis_count * 6);
    // Write axis number and its data offset
    *(ushort*)(node + axis_data_offset) = (ushort)axis_number;
    *(uint*)(node + axis_data_offset + 2) = axis_data_offset + 6;  // Points to after itself
    printf("Created axis %d in node %d, channel %d\n",
           axis_number, node_index, channel_index);
    return AXIS_SUCCESS;
}
```