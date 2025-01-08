# Overview
- 각 채널은 다른 노드의 채널과 link로 연결되는데, 이 때 axis라는 개념이 이용된다. axis는 link의 속성을 구분해 주는 개념이다. 예를 들면 forward link와 backward link는 서로 다른 axis(axis 0와 axis 1)으로 구분된다. forward 및 backward 뿐만 아니라 더 많은 axis를 정의하여 사용할 수 있다. 
- axis 3를 time axis로 정의하면 각 채널마다 axis 3로 채널이 생성된 시각에 대한 정보(8 bytes)를 저장하는 데이터와 연결시킬 수 있고, 그럼, 채널의 생성 시각, 수정시각 등 시간에 대한 정보를 forward, backward link들과는 독립적으로 관리할 수 있다. 이 데이터는 시각화할 때 별개의 UI를 적용하여 화면에 표시할 수도 있다.
# Axis data structure
- Axis data는 channel data 내에 포함되어 있다. 
- channel offset을 시작으로 하여 첫 2 바이트는 axis의 개수를 나타낸다. 처음 channel이 생성될 때는 이 값은 0으로 초기화된다. 
- axis 개수가 1 이상인 경우에는 그 다음 (6바이트 * axis 개수)가 axis의 number(2바이트)와 axis offset(4바이트)을 나타낸다. 
- 각각의 axis offset에서부터 첫 2바이트는 link count, 즉 링크 개수를 나타낸다. axis와 마찬가지로 처음 axis가 생성되면 이 값은 0으로 초기화된다. 링크 개수 다음에는 (6바이트 * 링크 개수)만큼 link data가 저장된다.
- 
# Axis 생성
- Axis를 생성하기 위해서는 생성하려는 node, channel 정보와, 생성하려는 axis의 종류를 함수에게 알려주어야 한다. axis의 종류는 번호로 구분된다. 0은 forward link, 1은 backward link, 이런 식으로 사전에 정의되어 있다. 
- channel data 내에서 axis에 대한 정보를 저장하는 방식은 다음과 같다.  먼저 axis의 개수를 2바이트로 나타낸다. 그리고 (6 bytes * axis의 개수)만큼 공간을 할당한다. 6 bytes는 axis number를 나타내는 2바이트와 axis data의 시작지점인 offset을 나타내는 4바이트로 이루어진다. offset의 기준점은 channel data의 시작점을 기준으로 한다. 즉, axis가 1개만 있다면, 첫번째 axis data의 offset은 2 + 6 = 8이 된다.  
- 처음에 생성된 node는 한 개의 channel을 가지지만 axis는 가지지 않는다. 따라서 처음 생성된 node에 link를 추가하는 등의 작업을 하려면 반드시 axis를 추가해 주어야 한다. 
- 처음 초기화할 때 channel은 1개 생성하지만, axis를 미리 생성하지 않는 이유는 axis는 종류가 있어서, 초기화 할 때 axis의 종류를 미리 지정하는 것은 큰 의미가 없기 때문이다. 
- axis 생성 함수는  [[Functions#create_axis|create_axis]] 참조.
## Axis 생성 시 data 이동
- 기존에 이미 axis가 하나 이상 있는 상태에서 새로운 axis를 추가한다고 생각해 보자. channel data의 시작점에서 2바이트는 axis의 개수이므로 이 값은 1 더해주면 된다. 그 다음은 (6bytes * aixs 개수)만큼 공간을 할당하고 각각의 6 bytes중 2바이트는 axis_number, 4바이트는 axis_offset인데, 이 axis_offset은 새로운 axis를 생성할 때에는 모든 axis의 offset을 6 바이트만큼 증가시키고 axis data도 모두 6바이트만큼 뒤로 평행이동 해야 한다. 왜냐하면 (6 bytes * aixs 개수) 뒤에부터 첫 번째 axis data가 시작되기 때문이다. 이 작업을 하지 않으면 정확한 axis data를 찾아갈 수 없게 된다. axis를 제거할 때도 마찬가지로 6바이트씩 offset을 감소시켜야 한다. 
## Axis 생성 시 axis data 초기화
- 새로운 axis 생성 시 axis data는 2바이트를 할당받는다. 이 2바이트는 link count를 0으로 초기화하기 위한 값이다. 
## Axis 생성시 node resize
- axis를 생성하는 과정에서 공간을 추가로 할당해야 하는 경우가 생길 수 있다. 이 경우에는 free space에 원하는 공간이 존재하는지 먼저 확인한 후, 존재하면 그 공간을 할당받아야 하고, 공간이 없는 경우에는 새로 저장공간을 할당받고 기존의 공간은 free space에 반납해야 한다. free space는 RAM에서의 공간을 관리하는 게 아니라 binary file에 저장되는 데이터 공간을 관리하는 객체이므로 RAM에서는 적절하게 기존 메모리 공간을 해제해야 한다. 
- axis 생성 시 node data를 resize해야 할 경우가 생길 수 있다. 이 경우에는 필요한 size를 먼저 계산한 후 [[Free Space#resize_node_space|resize_node_space]]를 호출해야 한다. 
- required size는 axis table의 마지막 값이 가리키는 offset + 마지막 axis data size + 6(새로운 axis table 추가) + 2(새로운 axis data offset에서 link count를 나타내는 2바이트(0으로 초기화))가 되어야 한다. 
- 
## Axis 생성 과정
- [[Channel 관련 함수#get_channel_offset|get_channel_offset]]을 이용하여 현재 channel_offset을 구한다. 
	 `uint channel_offset = get_channel_offset(node, channel_index);`
- 현재 channel의 axis_count를 구한다. 
	`ushort current_count = *(ushort*)(node + channel_offset);`
- 이미 생성하려는 axis가 존재하는 경우 error 반환 [[Axis 관련 함수#has_axis|has_axis]] 
```c
    if (has_axis(node, channel_offset, axis_number)) {
        printf("Error: Axis %d already exists\n", axis_number);
        return AXIS_ERROR;
    }
```
### 새로 필요한 `axis table size` 계산
```c
    uint axis_table_size = (current_count + 1) * 6;  // Including new axis entry
```
### required size 계산
- required size 계산: 현재 channel의 last axis offset을 구한다. 여기서 axis offset은 node offset부터 계산한 것이 아니라, channel offset의 시작 지점을 0으로 하여 상대적으로 계산한 offset이다. 
- `current_count`가 0인 경우에는 required_size는 channel_offset + axis_table_size + 4로 설정하면 된다. +4인 이유는 axis
- count가 2바이트 차지하고, link count가 2바이트 차지하기 때문이다. 
```c
    uint required_size;
    if (current_count == 0) {
        required_size = channel_offset + axis_table_size + 4;  // +2 for axis count, +2 for link count
    } else {
        // Get last axis's offset and data size
        uint* last_offset_ptr = (uint*)(node + channel_offset + 2 + ((current_count - 1) * 6) + 2);
        uint last_axis_offset = *last_offset_ptr;
        ushort* last_link_count = (ushort*)(node + channel_offset + last_axis_offset);
        uint last_axis_data_size = 2 + (*last_link_count * sizeof(Link));
        required_size = channel_offset + last_axis_offset + last_axis_data_size + 6 + 2;
    }
```
### node resize
- 현재 할당된 공간보다 더 많은 공간이 필요한 경우 저장공간을 재할당한다. 
```c
    // Check if we need more space
    uint current_node_size = 1 << (*(ushort*)node);
    if (required_size > current_node_size) {
        uint new_size;
        node = resize_node_space(node, required_size, node_index, &new_size);
        if (!node) {
            printf("Error: Failed to resize node\n");
            return AXIS_ERROR;
        }
        Core[node_index] = node;
    }
```
### move existing axis data
- 기존에 있던 axis data를 6바이트 뒤로 이동한다. axis table 크기가 6바이트 커지기 때문에 axis table 뒤에 있는 axis data는 모두 밀려나는 것이다. 
- 이미 필요한 공간을 재할당 받았기 때문에, 복잡한 계산을 할 필요 없이,  axis data가 시작하는 지점부터 (node size - 6) 바이트 만큼을 그대로 이동시키는 것이 간편하다. 
```c
    // Move existing axis data forward
    if (current_count > 0) {
        uint data_start = channel_offset + 2 + (current_count * 6);
        uint data_size = current_node_size - data_start - 6;  // Entire remaining data
        // Move all axis data forward by 6 bytes
        memmove(node + data_start + 6,
                node + data_start,
                data_size);
        // Update all existing axis offsets
        for (int i = 0; i < current_count; i++) {
            uint* offset_ptr = (uint*)(node + channel_offset + 2 + (i * 6) + 2);
            *offset_ptr += 6;
        }
    }
```

# Axis 삭제
- axis를 삭제하기 위해서는 먼저 해당 axis가 지정된 node, ch에 존재해야 한다. 존재하지 않으면 에러를 반환한다. 
- axis가 존재하면 해당 axis에 저장된 모든 link를 제거하고, axis number를 channel data에서 없애고, axis count를 1 감소시킨다. 
- 제거하려는 axis가 마지막 axis가 아니라면 axis를 제거한 뒤에는 제거된 axis보다 뒤에 있는 모든 axis의 offset을 적절하게 수정해야 한다. 
- 