# Overview
- 각각의 node의 channel은 임의의 다른 node의 channel과 연결될 수 있다. 이를 link라는 개념으로 설명한다. 
- node의 channel offset을 이용해서 해당 channel의 데이터 시작점으로 이동하면, axis의 개수에 대한 정보가 저장되어 있다. 그리고 axis의 offset을 이용해서 axis의 정보가 시작하는 위치로 이동하면 link 정보가 저장되어 있다. link는 해당 채널이 가리키는 다른 node, channel 쌍에 대한 정보를 가지고 있다. node를 나타내기 위해 4바이트를, channel을 나타내기 위해 2바이트를 사용한다. 따라서 하나의 link를 나타내기 위해서 총 6바이트가 필요하다.
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. 
# Create Link
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 생성하려는 axis가 존재하지 않으면 먼저 axis를 생성하고 나서 link를 생성한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. [[Graph Based Database#Axis 생성|Axis 생성]], [[Axis 관련 함수#has_axis|has_axis]]
```c
    // Check if axis exists, create if it doesn't
    if (!has_axis(node, channel_offset, axis_number)) {
        int result = create_axis(source_node, source_ch, axis_number);
        if (result != AXIS_SUCCESS) {
            printf("Error: Failed to create required axis\n");
            return LINK_ERROR;
        }
        // Reload node pointer as it might have changed after axis creation
        node = Core[source_node];
    }
```
- 현재 aixs의 current link count를 계산한다. channel_offset + axis_offset에서 ushort를 읽으면 current_link_count가 된다. 
```c
    // Get axis offset - this points to the link count
    uint axis_offset = get_axis_offset(node, source_ch, axis_number);
    // Get current link count and calculate new size
    ushort* current_link_count = (ushort*)(node + channel_offset + axis_offset);```
- link를 추가하는데 필요한 node size를 계산한다. 부족하면 공간을 재할당 받아야 한다. 
- link 하나를 추가하는 데에는 6바이트가 더 필요하므로, 현재 사용중인 공간에 6바이트의 여유공간이 있는지만 계산하면 된다. 이를 위해서는 node data의 actual size를 읽어서 6바이트를 더하여 required_size를 구하면 된다. 
```c
    uint current_actual_size = *(uint*)(node + 2);
    uint required_size = current_actual_size + 6;  // Add 6 bytes for new link
```
- required_size가 node_size보다 크면 공간 재할당
- 공간을 재할당한 이후에는 **반드시** link_count pointer를 업데이트 해주어야 한다. link_count는 pointer이기 때문에, 공간을 재할당 했는데도, 다시 업데이트하지 않으면, 여전히 이전 node를 가리키므로 이후 코드에서 에러가 발생한다. 
```c
    // Check if we need more space
    if (required_size > current_node_size) {
        uint new_size;
        node = resize_node_space(node, required_size, source_node, &new_size);
        if (!node) {
            printf("Error: Failed to resize node\n");
            return LINK_ERROR;
        }
        Core[source_node] = node;
        current_link_count = (ushort*)(node + channel_offset + axis_offset);
    }
```
- link data를 삽입할 위치를 계산한다. 
```c
    // Create link data
    Link link = {
        .node = dest_node,
        .channel = dest_ch
    };
    // Calculate insert position for new link
    uint link_insert_offset = channel_offset + axis_offset + 2 + (current_link_count * 6);
```
- 현재 axis가 마지막 채널의 마지막 axis가 아닌 경우에는 나머지 데이터들을 모두 6 bytes 앞으로 이동시켜야 한다. link_insert_offset이 actual data size보다 작다면 link_insert_offset부터 actual data size까지의 data를 6바이트 뒤로 이동시켜야 한다.  
```c
    // Check if this is not the last channel and not the last axis
    ushort channel_count = *(ushort*)(node + 2);  // Get channel count
    bool is_last_channel = (source_ch == channel_count - 1);
    bool is_last_axis = (axis_offset == last_axis_offset);
```
- link_insert_offset이 current_actual_size보다 작으면 이동시켜야 할 데이터가 반드시 있다. 
- 먼저 link data를 삽입할 위치를 찾고, 앞에서 계산한 required_size - move_start 만큼의 데이터를 6바이트 앞으로 이동시켜서 link data 6bytes가 들어가 공간을 만든다. 
- 그 다음 현재 채널의 현재 axis보다 뒤에 저장된 axis들의 offset을 모두 6바이트 증가시켜야 하는데, axis는 정렬되지 않은 상태로 저장될 수 있기 때문에 axis_count만큼 loop를 돌 수밖에 없다. axis를 순서대로 정렬시키지 않는 이유는 axis 번호는 channel과 달리 axis마다 고유한 성질을 부여할 수 있기 때문(모든 axis가 순서대로 생성되는 게 아님)
- 현재 channel 이후의 모든 채널의 offset도 6 증가시켜야 한다(channel은 반드시 0부터 증가하면서 저장되기 때문에, 현재 채널보다 큰 채널만 다루면 됨)
```c
    // Move data forward if this is not the last position
    if (link_insert_offset < current_actual_size) {
        // Calculate amount of data to move
        uint move_start = link_insert_offset;
        uint move_size = required_size - move_start;  // Move all remaining data
        // Move existing data forward by 6 bytes
        memmove(node + move_start + 6,
                node + move_start,
                move_size);
        // Update offsets in current channel
        uint axis_data_offset = channel_offset + 2;
        ushort axis_count = *(ushort*)(node + channel_offset);
        for (int i = 0; i < axis_count; i++) {
            uint current_axis_offset = *(uint*)(node + axis_data_offset + (i * 6) + 2);
            if (current_axis_offset > axis_offset) {
                *(uint*)(node + axis_data_offset + (i * 6) + 2) += 6;
            }
        }
        // Update only channel offsets for subsequent channels
        if (!is_last_channel) {
            for (int ch = source_ch + 1; ch < channel_count; ch++) {
                uint* channel_offset_ptr = (uint*)(node + 4 + (ch * 4));
                *channel_offset_ptr += 6;
            }
        }
    }
```
- 이후에는 link data를 삽입하고 link count를 1 증가시킨 다음, actual size를 required_size로 update하고, data.bin을 저장하면 된다. 
```c
    // Write link data at insert position
    memcpy(node + link_insert_offset, &link, sizeof(Link));
    // Update link count
    (*(ushort*)(node + channel_offset + axis_offset))++;
    *(uint*)(node + 2) = required_size;
    // Save changes to data.bin
    FILE* data_file = fopen(DATA_FILE, "r+b");
    if (data_file) {
        fseek(data_file, CoreMap[source_node].file_offset, SEEK_SET);
        fwrite(node, 1, 1 << (*(ushort*)node), data_file);
        fclose(data_file);
    }
    // Save updated free space information
    save_free_space();
    printf("Created link from node %d channel %d to node %d channel %d using axis %d\n",
           source_node, source_ch, dest_node, dest_ch, axis_number);
    return LINK_SUCCESS;
```
- 

# Delete Link
- link가 존재하는지 찾고 없으면 return error
```c
    // Search for the link
    bool found = false;
    int link_position = -1;
    for (int i = 0; i < *current_link_count; i++) {
        Link* current_link = (Link*)(node + link_data_offset + (i * sizeof(Link)));
        if (current_link->node == dest_node && current_link->channel == dest_ch) {
            found = true;
            link_position = i;
            break;
        }
    }
    if (!found) {
        printf("Error: Link not found\n");
        return LINK_ERROR;
    }
```
- target_link_offset이 actual_size - 6보다 작으면 메모리 이동이 필요하므로 이후의 메모리들을 6바이트만큼 앞으로 이동시킨다. 
```c
    uint target_link_offset = link_data_offset + (link_position * sizeof(Link));
    uint actual_size = *(uint*)(node + 2);
    // Shift remaining links to fill the gap
    if (target_link_offset < actual_size - sizeof(Link)) {
        memmove(node + target_link_offset,
                node + target_link_offset + sizeof(Link),
                actual_size - target_link_offset - sizeof(Link));
    }
```
- link_count와 actual_size를 update
```c
    // Update link count and actual size
    (*current_link_count)--;
    *(uint*)(node + 2) = actual_size - sizeof(Link);
```
