# Overview
- 각각의 node의 channel은 임의의 다른 node의 channel과 연결될 수 있다. 이를 link라는 개념으로 설명한다. 
- node의 channel offset을 이용해서 해당 channel의 데이터 시작점으로 이동하면, axis의 개수에 대한 정보가 저장되어 있다. 그리고 axis의 offset을 이용해서 axis의 정보가 시작하는 위치로 이동하면 link 정보가 저장되어 있다. link는 해당 채널이 가리키는 다른 node, channel 쌍에 대한 정보를 가지고 있다. node를 나타내기 위해 4바이트를, channel을 나타내기 위해 2바이트를 사용한다. 따라서 하나의 link를 나타내기 위해서 총 6바이트가 필요하다.
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. 
# Create Link
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 생성하려는 axis가 존재하지 않으면 먼저 axis를 생성하고 나서 link를 생성한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는지 확인하고, 없다면 새로 axis를 만들어야 한다. 함수의 복잡도를 줄이기 위해 create_link를 호출하기 전에 axis를 만들어 놓고 create_link를 호출하기 때문에, create_axis는 이 부분에 대해서는 신경쓰지 않아도 된다.  [[Graph Based Database#Axis 생성|Axis 생성]], [[Axis 관련 함수#has_axis|has_axis]]
## check and resize node space
- required size를 계산하고, node size를 재할당 해야 한다면 재할당한다. 
```c
    uint current_actual_size = *(uint*)(node + 2);
    uint required_size = current_actual_size + 6;  // Add 6 bytes for new link
    // Check and resize if needed using the new function
    int resize_result = check_and_resize_node(&node, required_size, source_node);
    if (resize_result == FREE_SPACE_ERROR) {
        printf("Error: Failed to resize node\n");
        return LINK_ERROR;
    }
```
- link data를 삽입할 위치를 계산한다. 
```c
    ushort channel_count = get_channel_count(node);  // Get channel count
    ushort* current_link_count = (ushort*)(node + channel_offset + axis_offset);
    uint link_insert_offset = channel_offset + axis_offset + 2 + (*current_link_count * 6);
```
- 현재 axis가 마지막 채널의 마지막 axis가 아닌 경우에는 나머지 데이터들을 모두 6 bytes 앞으로 이동시켜야 한다. link_insert_offset이 actual data size보다 작다면 link_insert_offset부터 actual data size까지의 data를 6바이트 뒤로 이동시켜야 한다.  
- 경우를 두 가지로 굳이 나누어서 코드를 늘리지 말고 insert_link 함수를 써서 나머지 처리는 insert_link 함수에게 맡긴다. move_size가 0인 경우 insert_link가 알아서 마지막 위치에 link data를 기록한다. 
```c
    ushort channel_count = get_channel_count(node);  // Get channel count
    ushort* current_link_count = (ushort*)(node + channel_offset + axis_offset);
    uint link_insert_offset = channel_offset + axis_offset + 2 + (*current_link_count * 6);
    uint move_size = current_actual_size - link_insert_offset;  // Move all remaining data
    insert_link(node, link_insert_offset, dest_node, dest_ch, move_size);
```
- 그 다음 현재 채널의 현재 axis보다 뒤에 저장된 axis들의 offset을 모두 6바이트 증가시켜야 하는데, axis는 정렬되지 않은 상태로 저장될 수 있기 때문에 axis_count만큼 loop를 돌 수밖에 없다. axis를 순서대로 정렬시키지 않는 이유는 axis 번호는 channel과 달리 axis마다 고유한 성질을 부여할 수 있기 때문(모든 axis가 순서대로 생성되는 게 아님)(axis를 번호순으로 정렬하는 것이 더 효율적인지는 고민해 보아야 할 문제!!)
```c
    uint axis_entry_offset = channel_offset + 2;
    ushort axis_count = get_axis_count(node, source_ch);
    for (ushort i = 0; i < axis_count; i++)
    {
        uint current_axis_offset = *(uint *)(node + axis_entry_offset + (i * 6) + 2);
        if (current_axis_offset > axis_offset)
        {
            *(uint *)(node + axis_entry_offset + (i * 6) + 2) += 6;
        }
    }
```
- 현재 channel 이후의 모든 채널의 offset도 6 증가시켜야 한다(channel은 반드시 0부터 증가하면서 저장되기 때문에, 현재 채널보다 큰 채널만 다루면 됨)
```c
    // Update only channel offsets for subsequent channels
    if (source_ch < channel_count - 1)
    {
        for (ushort ch = source_ch + 1; ch < channel_count; ch++)
        {
            *(uint *)(node + 8 + (ch * 4)) += 6;
        }
    }
```
- 이후에는 link data를 삽입하고 link count를 1 증가시킨 다음, actual size를 required_size로 update하고, data.bin을 저장하면 된다. 
```c
    (*current_link_count)++;
    *(uint*)(node + 2) = required_size;
    if (!save_node_to_file(source_node)) {
        printf("Error: Failed to update data.bin\n");
        return LINK_ERROR;
    }
```
- 

# Delete Link
## calculate current link count
- 현재 link count를 계산한다. 
```c
    uchar* node = Core[source_node];
    uint channel_offset = get_channel_offset(node, source_ch);
    // Get axis offset and link count
    uint axis_offset = get_axis_offset(node, source_ch, axis_number);
    ushort* current_link_count = (ushort*)(node + channel_offset + axis_offset);
    uint link_data_offset = channel_offset + axis_offset + 2;  // Skip link count
```
- link_count만큼 loop를 돌면서 삭제하려는 link가 위치하는 index를 찾는다. link를 찾으면 found = true가 된다. 
```c
    bool found = false;
    int link_position = -1;
    for (int i = 0; i < *current_link_count; i++) {
        Link* current_link = (Link*)(node + link_data_offset + (i * 6));
        if (current_link->node == dest_node && current_link->channel == dest_ch) {
            found = true;
            link_position = i;
            break;
        }
    }

```
- target_link 보다 뒤에 있는 data를 6바이트만큼 앞으로 이동시킨다. 
```c
    uint target_link_offset = link_data_offset + (link_position * 6);
    uint actual_size = *(uint*)(node + 2);
    // Shift remaining links to fill the gap
    if (target_link_offset < actual_size - 6) {
        memmove(node + target_link_offset,
                node + target_link_offset + 6,
                actual_size - target_link_offset - 6);
    }
```
- target_link가 포함된 axis보다 index가 큰 axis들은 모두 offset을 6 감소시켜야 한다. 
```c
    uint current_axis_count = get_axis_count(node, source_ch);
    uint axis_entry_offset = channel_offset + 2;
    for (ushort i = axis_index; i < current_axis_count; i++) {
        uint current_axis_offset = *(uint *)(node + axis_entry_offset + (i * 6) + 2);
        if (current_axis_offset > axis_offset) {
            *(uint *)(node + axis_entry_offset + (i * 6) + 2) -= 6;
        }
    }
```
- 현재 channel보다 큰 channel들의 offset도 모두 6 감소시켜야 한다. 
```c
    ushort channel_count = get_channel_count(node);
    for (ushort ch = source_ch + 1; ch < channel_count; ch++) {
        *(uint *)(node + 8 + (ch * 4)) -= 6;
    }
```

# find Link index
- input으로 source vertex index, source channel index, source axis number, dest node, dest channel이 주어지면, source vertex, ch, axis에 존재하는 link들 중에, dest node, channel이 존재하는지 찾아서, 존재한다면 몇 번째 link에 있는지 반환하는 함수이다. 존재하지 않거나 link count가 0이면 -1을 반환한다.
# get Link
- current_vertex_index, current ch index, axis number, link index를 input으로 받아서 해당 link index에 해당하는 link data(vertex, ch)가 존재하는지 확인하여 존재한다면 vertex, ch 값을 반환한다. 