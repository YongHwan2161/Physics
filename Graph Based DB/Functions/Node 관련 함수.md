# Create_New_Node
- 새로운 token을 생성할 필요가 있을 때, node_index를 1 증가시키면서 새로운 node를 생성하는 함수이다. 
- 새로운 node data가 저장될 file offset을 결정하는 방법이 중요하다. 기존에는 currentnodeCount - 2의 index를 갖는 node가 binary file의 제일 마지막에 기록되어 있을 것이라고 가정하고, 해당 index의 node의 file_offset 값을 기준으로 node data size만큼 더한 값을 new_node의 file_offset으로 설정했으나, 이 방식은 문제가 있다. 
- 반드시 node_index 값이 가장 큰 node data가 binary file의 마지막에 저장되리라는 보장이 없다. 중간에 저장공간이 부족한 node가 생기면 재할당을 하면서 새로운 offset을 할당하기 때문이다. 
- 따라서 binary file의 마지막 offset을 찾는 다른 방법을 강구해야 한다. 
- 
```c
int create_new_node() {
    uchar* newnode = (uchar*)malloc(16 * sizeof(uchar));  // Always allocate 16 bytes initially
    // printf("Creating new node at index %d\n", CurrentnodeCount);
    initialize_node(&newnode);
    CurrentnodeCount++;
    save_current_node_count();
    Core[CurrentnodeCount - 1] = newnode;
    CoreSize++;
    CoreMap[CurrentnodeCount - 1].core_position = CurrentnodeCount - 1;
    CoreMap[CurrentnodeCount - 1].is_loaded = 1;
    if (CurrentnodeCount == 1) {
        CoreMap[CurrentnodeCount - 1].file_offset = 0;
    } else {
        uint last_node_size = 1 << (*(ushort*)Core[CurrentnodeCount - 2]);
        // printf("Last node size: %d\n", last_node_size);
        uint file_offset = CoreMap[CurrentnodeCount - 2].file_offset + last_node_size;
        CoreMap[CurrentnodeCount - 1].file_offset = file_offset;
    }
    if (!save_node_to_file(CurrentnodeCount - 1)) {
        printf("Error: Failed to save node\n");
        return ERROR;
    }
    printf("node created at index %d\n", CurrentnodeCount - 1);
    return SUCCESS;
}
```