# Overview
- 각 채널은 다른 노드의 채널과 link로 연결되는데, 이 때 axis라는 개념이 이용된다. axis는 link의 속성을 구분해 주는 개념이다. 예를 들면 forward link와 backward link는 서로 다른 axis(axis 0와 axis 1)으로 구분된다. forward 및 backward 뿐만 아니라 더 많은 axis를 정의하여 사용할 수 있다. 
- axis 3를 time axis로 정의하면 각 채널마다 axis 3로 채널이 생성된 시각에 대한 정보(8 bytes)를 저장하는 데이터와 연결시킬 수 있고, 그럼, 채널의 생성 시각, 수정시각 등 시간에 대한 정보를 forward, backward link들과는 독립적으로 관리할 수 있다. 이 데이터는 시각화할 때 별개의 UI를 적용하여 화면에 표시할 수도 있다.
# Axis data structure
- Axis data는 channel data 내에 포함되어 있다. 
- channel offset을 시작으로 하여 첫 2 바이트는 axis의 개수를 나타낸다. 처음 channel이 생성될 때는 이 값은 0으로 초기화된다. 
- axis 개수가 1 이상인 경우에는 그 다음 (6바이트 * axis 개수)가 axis의 number(2바이트)와 axis offset(4바이트)을 나타낸다. 
- 각각의 axis offset에서부터 첫 2바이트는 link count, 즉 링크 개수를 나타낸다. axis와 마찬가지로 처음 axis가 생성되면 이 값은 0으로 초기화된다. 링크 개수 다음에는 (6바이트 * 링크 개수)만큼 link data가 저장된다.
- 
# Axis 종류
```
#define TOKEN_SEARCH_AXIS 0
#define TOKEN_DATA_AXIS 1
#define string_AXIS 2
#define PROPERTY_AXIS 3
```
- Axis 0: Token Search Axis: token data를 얻을 때 탐색하는 axis이다. 각 axis는 반드시 2개의 link entry를 가지고 있어서 2진 트리 구조를 탐색하여 token data를 읽어온다.
- Axis 1: Token Data Axis: data가 저장된 배열을 token으로 분리하기 위해서 이미 존재하는 token을 탐색할 때 이용되는 axis이다.
- Axis(2): string_Axis; 이 axis로 연결된 string cycle은 token data를 연결하여 긴 데이터를 저장할 수 있다. 
- Axis(3): property axis: 각각의 vertex가 어떤 고유한 성질을 지니는지를 알고 싶으면 propety axis가 가리키는 node의 index 번호를 불러오면 된다. ch은 0을 가리킨다(아직은 의미가 없지만, 의미를 부여할 수도 있음) 예를들면, property axis가 node 0을 가리킨다면 이는 string의 시작점을 가리키는 vertex라고 약속할 수 있다. 데이터의 종류가 다양해질 수록 vertex의 성질을 나타내기 위해 더 많은 node가 필요할테지만, vertex의 성질은 node의 개수만큼 다양하게 지정할 수 있으므로 문제없다. 
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
- current_actual_size에서 8을 더한 값을 required size로 설정한다. 6바이트는 추가되는 axis entry size이고, 2바이트는 axis data에서 link count 2 bytes를 표시하기 위해 필요하다.  
```c
    // Get current actual size and calculate new required size
    uint current_actual_size = *(uint*)(node + 2);
    uint required_size = current_actual_size + 8;  // Add 6 bytes for new axis entry
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
- 그리고 기존의 axis table에서 axis_offset도 모두 6바이트 증가시킨다. 
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
### new axis offset 계산
- 새로 생성한 axis의 offset을 계산한다. 
- axis_count가 0인 상태에서 axis를 추가하는 경우에는 new_axis_offset은 8로 고정이다(axis count(2 bytes) + axis_table(4 bytes)).
- axis_count가 1 이상인 상태에서 axis를 추가하는 경우에는 {기존에 존재하던 마지막 axis의 offset + 2(link count) + (6 * link count) + 6(new axis entry)}이 new_axis_offset의 값이 되어야 한다. 
- new_axis_offset을 더 간단하게 계산하려면 위에서 이미 구한 required size에서 channel offset을 빼고, 2를 더 빼면 된다. 
```c
    uint new_axis_offset;
    if (current_count == 0) {
        new_axis_offset = 8;  // Fixed offset: axis count(2) + first axis table entry(6)
    } else {
        // Use already calculated required_size
        new_axis_offset = required_size - channel_offset - 2;  // -2 for new link count
    }
```
### channel offset update
- 만약 channel count가 2 이상인 경우, 그리고 현재 axis가 속한 channel 보다 더 큰 index의 channel이 존재하는 경우, 이후의 모든 channel offset을 6씩 증가시켜야 한다. 
```c
    //update channel offset
    ushort channel_count = *(ushort*)(node + 6);
    for (ushort i = channel_index + 1; i < channel_count; i++) {
        *(uint*)(node + 8 + (i * 4)) += 6;
    }
```
# Axis 삭제
- axis를 삭제하기 위해서는 먼저 해당 axis가 지정된 node, ch에 존재해야 한다. 존재하지 않으면 에러를 반환한다. 
- axis가 존재하면 해당 axis에 저장된 모든 link를 제거하고, axis number를 channel data에서 없애고, axis count를 1 감소시킨다. 
- 제거하려는 axis가 마지막 axis가 아니라면 axis를 제거한 뒤에는 제거된 axis보다 뒤에 있는 모든 axis의 offset을 삭제된 바이트 수(deleted axis entry(6 bytes) + 2(link count) + 6*(link count)) 만큼 앞으로 이동해야 한다. 
- Axis를 삭제할 때에는 node data resize 과정을 수행할 필요가 없다. 어차피 데이터의 크기가 줄어들기 때문에, 굳이 더 작은 공간으로 재할당할 필요는 없다. 
## Axis 삭제 과정
### current_axis_count 계산
```c
    // Get current axis count
    ushort* axis_count = (ushort*)(node + channel_offset);
    ushort current_axis_count = *axis_count;
```
### 삭제하려는 axis의 index 계산
- axis number는 channel과 달리 정렬되어 있지 않기 때문에, 삭제하려는 axis가 axis entry의 몇 번째에 위치하는지 찾아야 한다. 찾아서 axis_index에 저장한다. 
- 이 때 삭제하려는 axis_index를 제외한 모든 axis_entry에서 offset을 -6만큼 감소시킨다. 
```c
    // Find the axis to delete
    int axis_index = -1;
    uint axis_offset = 0;
    for (int i = 0; i < *axis_count; i++) {
        ushort current_axis = *(ushort*)(node + channel_offset + 2 + (i * 6));
        if (current_axis == axis_number) {
            axis_index = i;
            axis_offset = *(uint*)(node + channel_offset + 2 + (i * 6) + 2);
        }
        else {
            *(uint*)(node + channel_offset + 2 + (i * 6) + 2) -= 6;
        }
    }
```
- axis data를 삭제할 때 두 영역을 제거해야 한다. axis entry 6 bytes와 axis data를 제거해야 하는데, axis data를 먼저 제거하고 나머지 data들을 제거한 size만큼 앞으로 이동시킨 다음, axis entry 6 bytes를 제거하고 다시 나머지 data를 6바이트 앞으로 이동시킨다. 
- 먼저 axis data를 삭제하는 과정을 본다. 삭제하려는 'axis_index'를 이용하여 계산한 'axis_offset'부터 (2 + (6 * link_count))만큼 제거하면 된다. 제거하는 과정은 따로 없고 그냥 뒤에 있는 data를 memmove를 이용하여 (2 + (6 * link_count))만큼 앞으로 이동시키면 된다. 이동시켜야 하는 data의 size는 `current_actual_size - (channel_offset + data_offset + (2 + 6 * link_count))`이다. 
```c
    uint current_actual_size = *(uint*)(node + 2);
    ushort link_count = *(ushort*)(node + channel_offset + axis_offset);
    uint move_start = channel_offset + axis_offset + 2 + (link_count * 6);
    uint move_size = current_actual_size - move_start;
    memmove(node + channel_offset + axis_offset,
            node + move_start,
            move_size);
```
- 그 다음 `axis_entry` 6 bytes를 제거한다. 역시 `memmove`를 이용해서 6바이트를 앞으로 당기는데, `move_start`는 `channel_offset + 2 + (6 * (axis_index + 1))`이고, `move_end`는 `channel_offset + 2 + (6 * axis_index)`가 된다. `move_size`는 `current_actual_size - move_start`가 된다. 
```c
    move_start = channel_offset + 2 + ((axis_index + 1) * 6);
    uint move_dest = move_start - 6;
    move_size = current_actual_size - move_dest;
    memmove(node + move_dest,
            node + move_start,
            move_size);
```
### channel_offset 수정
- `current_channel`보다 큰 channel이 있으면 `channel_entry`에서 offset을 `6 + 2 + (link_count * 6)`만큼 감소시키면 된다. `current_channel`보다 작은 channel들은 변경할 필요가 없다. 
```c
    ushort channel_count = *(ushort*)(node + 6);
    for (ushort i = channel_index + 1; i < channel_count; i++) {
        *(uint*)(node + 8 + (i * 4)) -= 6 + 2 + (link_count * 6);
    }
```
- `actual_size`와 `axis_count`를 적절하게 변경한다. 그리고 변경된 node data는 binary file과 동기화한다. 
```c
    uint new_actual_size = current_actual_size - size_reduction;
    // Update axis count and actual size
    (*axis_count)--;
    *(uint*)(node + 2) = new_actual_size;  // -6 for removed axis entry
    // Save changes to data.bin
    if (!save_node_to_file(node_index)) {
        printf("Error: Failed to save node\n");
        return AXIS_ERROR;
    }
    return AXIS_SUCCESS;
```

# Error
## 