# has_axis
```c
bool has_axis(uchar* node, uint channel_offset, int axis_number) {
    ushort axis_count = *(ushort*)(node + channel_offset);
    int axis_data_offset = channel_offset + 2;  // Skip axis count
    // Search for the axis
    for (int i = 0; i < axis_count; i++) {
        ushort current_axis = *(ushort*)(node + axis_data_offset + (i * 6));
        if (current_axis == axis_number) {
            return true;
        }
    }
    return false;
}
```

# create_axis
- 인자로 `node_index`, `channel_index`, `axis_number`를 받는다. 
- Core에 해당 `node_index`가 존재하지 않으면 `AXIS_ERROR`를 반환한다. 
- create_axis 함수에서는 인자로 받은 axis_number가 현재 node,channel data에 존재하는지 확인한 후 존재하면 새로 axis를 생성할 필요가 없고, 존재하지 않는 경우에만 새로 axis를 생성해야 한다. 
- `channel_index`의 offset을 구하고 offset이 0보다 작으면 역시 error를 반환한다. 채널의 offset을 구하는 함수는 [[Channel 관련 함수#get_channel_offset|get_channel_offset]] 참조.
- 주어진 node, ch에 대해 해당 axis가 이미 존재하는지 확인해 주는 함수는 [[Axis 관련 함수#has_axis|has_axis]] 참조.
- required_size를 구할 때, node data의 offset 2에 저장된 actual size를 직접 이용하여 빠르게 계산한다. 
```c
    // Get current actual size and calculate new required size
    uint current_actual_size = *(uint*)(node + 2);
    uint required_size = current_actual_size + 6;  // Add 6 bytes for new axis entry
```
- required_size와 current_node_size를 비교하여 부족하면  새로 공간을 할당받는다. 
```c
    // Check if we need to resize the node
    ushort node_size_power = *(ushort*)node;
    uint current_node_size = 1 << node_size_power;
    if (required_size > current_node_size) {
        uint new_size;
        node = resize_node_space(node, required_size, node_index, &new_size);
        if (!node) {
            printf("Error: Failed to resize node\n");
            return AXIS_ERROR;
        }
        Core[node_index] = node;
        // channel_offset remains the same, no need to recalculate
    }
```

```c
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

