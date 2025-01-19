- 채널은 노드마다 1개 이상 부여될 수 있는 개념이다. 같은 노드 내에서 서로 다른 채널을 가진 경우에는, Node에 저장된 데이터는 공유하지만, 각각의 채널은 서로 독립적이다. 따라서 한 채널에서의 연결 관계들은 다른 채널의 연결 관계들과 완전히 독립적이다.
- 각 노드는 적어도 하나의 채널을 가진다. 

# channel creation
- channel 0은 node가 생성될 때 기본적으로 생성된다. 
- channel은 항상 순차적으로 생성된다(axis와 다른 점). 
- channel을 생성할 때에는 node index만 전달하면 된다. create channel 함수는 전달된 node의 channel count를 1 증가시키고, channel entry 4 bytes를 추가하고, channel offset을 적절하게 계산한 다음, channel data에 2 bytes를 추가하여 axis count를 0으로 초기화하여 대입하면 된다. 
## 채널 생성 과정
- 먼저 channel_count를 구한다. 
```c
    uchar* node = Core[node_index];
    ushort* channel_count = (ushort*)(node + 6);  // Skip size power(2) and actual size(4)
```
- required size를 구한다. actual size에 6을 더하면 된다(channel entry 4 + axis count 2)
```c
    // Get current actual size and calculate required size
    uint current_actual_size = *(uint*)(node + 2);
    uint required_size = current_actual_size + 6;  // channel entry(4) + axis count(2)
```
- required size가 node size보다 크면 공간 재할당
```c
    if (required_size > current_node_size) {
        uint new_size;
        node = resize_node_space(node, required_size, node_index, &new_size);
        if (!node) {
            printf("Error: Failed to resize node\n");
            return CHANNEL_ERROR;
        }
        Core[node_index] = node;
    }
```
- channel entry 4 bytes를 추가하기 위해 나머지 data를 4 바이트 뒤로 이동
```c
    uint current_offset = 8 + ((uint)*channel_count * 4);  // Header + existing channel offsets
    uint channel_data_offset = current_actual_size + 4;  // New channel data goes at the end
    // Move existing data 4 bytes forward to make space for new channel entry
        memmove(node + current_offset + 4,          // destination (4 bytes forward)
                node + current_offset,               // source
                current_actual_size - current_offset // size of data to move
        );
```
- 기존 channel offset을 4 증가하고, 새로운 channel offset을 추가하고, axis count를 0으로 초기화하고, actual size와 channel_count를 update한다. 
```c
    //update the channel offset
    for (ushort i = 0; i < *channel_count; i++) {
        *(uint*)(node + 8 + (i * 4)) += 4;
    }
    // Add new channel offset
    *(uint*)(node + current_offset) = channel_data_offset;
    // Initialize axis count to 0 at the end
    *(ushort*)(node + channel_data_offset) = 0;
    // Update actual size
    *(uint*)(node + 2) = required_size;
    // Increment channel count
    (*channel_count)++;
```
## recycle or create ch
- clear_channel을 이용해서 초기화된 channel이 vertex에 존재할 수 있다. 이 경우에는 새로 channel을 추가할 게 아니라 빈 ch을 재활용하면 된다. 
- 이 함수는 주어진 vertex의 ch들을 순회하면서 empty channel이 있는지 확인하다가 empty channel을 발견하면 해당 ch_index를 반환하고, 마지막 ch까지 확인했는데, empty ch이 없으면 새로 ch을 추가한 뒤 추가된 ch의 index를 반환한다. 
# Clear channel
- 생성된 channel을 삭제해도, 실제로 channel이 삭제되지는 않는다. 다만, channel 내의 모든 data가 초기화될 뿐이다(axis 개수가 0으로 변경됨). 그 이유는 channel을 삭제하려면 더 높은 index의 channel들의 channel index를 1씩 감소해야 하는데, link 정보에 node와 channel이 들어가기 때문에, channel index는 함부로 변경하면 안 된다. 변경된 channel index를 참조하는 모든 link들을 업데이트하는 것은 비효율적이다. 어차피 삭제된 channel은 나중에 재활용될 수 있기 때문에, 그대로 두고 channel data만 초기화하는 것이 효율적이다. 따라서 delete channel이 아닌 clear channel로 이름을 정한다. 
## clear channel 과정
- 초기화하려는 channel index의 offset에서 axis count를 0으로 변경시켜 주고, target channel보다 index가 큰 channel들의 offset을 제거한 data size만큼 앞으로 당겨준다. 
- 
- 
