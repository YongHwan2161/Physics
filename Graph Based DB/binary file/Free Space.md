# Oveview
- data.bin에 저장되는 데이터는 반드시 index 순서로 저장되는 것이 아니다. 또한 모든 저장공간이 차곡차곡 쌓여 있어서 빈 공간이 없는 구조도 아니다. 그 이유는 이미 특정 index의 node data가 할당되어 있는 상태에서 node data의 크기가 커지는 경우에는 새로 저장공간을 할당해 줘야 하는데, 이 때 이 하나의 node data  때문에, 나머지 index의 node data가 저장된 binary file을 수정하는 것은 매우 비효율적이기 때문이다. 이 때는 기존에 node data가 저장된 공간은 free space로 회수하고, binary file의 끝 위치 혹은 free space에서 관리하는 해당하는 크기의 공간을 새로 할당하는 방식을 사용해야 한다. 그래야 변경하려는 data의 node 정보만 수정하고 나머지 data들은 건들지 않을 수 있다. 
- free space는 binary file로 관리되어야 하며(그래야 프로그램 종료시에도 정보를 유지할 수 있으므로), 프로그램 실행시에는 모두 RAM에 올려놓고 사용한다(용량이 크지 않으므로 다 올려도 됨).
- free space binary file과 map.bin, 그리고 data.bin 파일은 수정사항이 발생할 때 모두 함께 업데이트 되어야 한다. 어느 하나만 업데이트 되고 나머지는 업데이트되지 않는다면 오류가 발생할 수 있으므로 반드시 세 binary file은 동기화 되어 있어야 한다. 
- free space는 삭제된 node index의 목록도 관리해야 한다. 삭제된 index는 나중에 재활용해야 하기 때문이다. 
- free space는 관리의 편리함을 위해서 2의 제곱 단위로 공간을 관리하며 최소 크기는 16 bytes가 될 것이다. 즉, 16, 32, 64, ... bytes 단위로 저장공간이 관리된다.  이를 위해서는 노드를 생성하거나 수정할 때에도 반드시 2의 제곱 단위로 저장공간을 관리해야 한다. 

# resize node space
- node space를 resize하는 로직이다. 
- node data에 새로운 공간을 할당하는 경우에는 데이터가 차지하는 공간을 제외한 나머지 공간은 모두 0으로 초기화한다(안 해도 상관은 없지만 나중에 이상한 오류의 원인이 될 수도 있음).
-  node 크기를 resize한 경우에는 반드시 save_free_space 함수를 호출해서 binary file과 동기화 해야 한다. 
-  resize한 경우 무조건 `CoreMap[node_index]`가 변경되기 때문에 save_map(node_index)를 호출하여 변경된 사항을 binary file과 동기화 해야 한다. 
## node space resizing 과정
- 전달받은 required_size보다 큰 new_size를 구한다(power of 2).
```c
    // Calculate new size (next power of 2)
    ushort node_size_power = *(ushort*)node;
    uint current_size = 1 << node_size_power;
    while ((1 << node_size_power) < required_size) {
        node_size_power++;
    }
    // Set new size for caller
    *new_size = 1 << node_size_power;
```
- new_size에 해당하는 Free Block이 있는지 찾는다. [[Free Space#find free block|find free block]]
```c
    // Allocate new space
    FreeBlock* free_block = find_free_block(*new_size);
```
- 반환된 FreeBlock이 있으면 해당 FreeBlock을 새로운 node pointer에 할당하고, 기존에 사용하던 저장공간은 free space에 반납한다. 
- free space에서는 새로 재할당한 new_size free block은 free space에서 제거해야 한다. 
```c
    if (free_block) {
        // Use found free block
        new_node = (uchar*)malloc(*new_size);
        memcpy(new_node, node, current_size);
        // Update CoreMap with new location
        CoreMap[node_index].file_offset = free_block->offset;
        // Add old space to free space
        add_free_block(current_size, CoreMap[node_index].file_offset);
    }
```
# free space가 생기는 경우
## node data 삭제:
- node data를 삭제하면 삭제한 node data가 free space로 이동한다. 해당하는 node data의 size와 offset이 free space에 저장되는 것이다. 그리고 CoreMap에서는 node data를 unload하고, data.bin에서 해당 node data를 모두 0으로 초기화한다. 
## node data 수정
  - node data를 수정하는 과정에서 ch을 추가하거나, link를 추가하는 등의 작업으로 인해서 node data size가 증가하다가 기존에 할당된 크기를 넘어서는 경우에는 새로 공간을 할당해 줘야 한다. 이 때, 기존에 할당된 공간은 free space로 반납하고, 새로운 공간을 재할당해 줘야 한다. 
# free space가 줄어드는 경우
## node data 생성
- node를 새로 만드는 경우에는 먼저 16바이트 공간이 free space에 존재하는지 확인한 후, 있으면 그 공간을 그대로 활용해서 node를 생성한다. 이 때 node index는 삭제된 node index가 있는 경우에는 index를 재활용하고, 그렇지 않은 경우에는 새로운 index를 부여한다. 


# Free space 출력
- free space에 저장된 내용을 화면에 출력해 줄 수 있어야 한다. 

# find free block
- free_space에서 원하는 size의 free block이 있는지 찾아서 있으면 반환한다. FreeSpace 구조체에 대해서는 [[Variables#`FreeSpace`|FreeSpace]] 참조
```c
FreeBlock* find_free_block(uint size) {
    for (uint i = 0; i < free_space->count; i++) {
        if (free_space->blocks[i].size == size) {
            return &free_space->blocks[i];
        }
    }
    return NULL;
}
```
# add free block
- free space에 free block을 반납하는 작업을 처리하는 함수이다. 
```c
void add_free_block(uint size, long offset) {
    free_space->count++;
    free_space->blocks = (FreeBlock*)realloc(free_space->blocks,
                                            free_space->count * sizeof(FreeBlock));
    free_space->blocks[free_space->count - 1].size = size;
    free_space->blocks[free_space->count - 1].offset = offset;
}
```

# get free block
- free space에서 free block을 하나 빼내어 new node에게 공간을 할당해 주는 작업을 처리하는 함수이다. 
```c

```