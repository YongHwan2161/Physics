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



# initialize_node
```c
void initialize_node(uchar** node) {
    for (int i = 0; i < 16; ++i) {
        (*node)[i] = initValues[i];
    }
}
```