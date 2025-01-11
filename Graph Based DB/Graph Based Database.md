# Node
- [[Node]]
# Channel
- 채널은 노드마다 1개 이상 부여될 수 있는 개념이다. 같은 노드 내에서 서로 다른 채널을 가진 경우에는, Node에 저장된 데이터는 공유하지만, 각각의 채널은 서로 독립적이다. 따라서 한 채널에서의 연결 관계들은 다른 채널의 연결 관계들과 완전히 독립적이다.
- 각 노드는 적어도 하나의 채널을 가진다. 

## channel creation
- channel 0은 node가 생성될 때 기본적으로 생성된다. 
- channel은 항상 순차적으로 생성된다(axis와 다른 점). 
- channel을 생성할 때에는 node index만 전달하면 된다. create channel 함수는 전달된 node의 channel count를 1 증가시키고, channel entry 4 bytes를 추가하고, channel offset을 적절하게 계산한 다음, channel data에 2 bytes를 추가하여 axis count를 0으로 초기화하여 대입하면 된다. 
### 채널 생성 과정
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
# delete channel
- 생성된 channel을 삭제해도, 실제로 channel이 삭제되지는 않는다. 다만, channel 내의 모든 data가 초기화될 뿐이다(axis 개수가 0으로 변경됨). 그 이유는 channel을 삭제하려면 더 높은 번호의 channel들의 channel 번호를 변경해야 하는데, link 정보에 node와 channel이 들어가기 때문에, channel 번호는 절대 변경되어서는 안 된다. 변경된 channel 번호를 참조하는 모든 link들을 업데이트하는 것은 비효율적이다. 어차피 삭제된 channel은 나중에 재활용될 수 있기 때문에, 그대로 두고 channel data만 초기화하는 것이 효율적이다. 
- 

# [[Axis]]

# [[Link]]
# Free Space
- [[Free Space]]
# DB가 존재하는지 확인 및 load
- 처음 프로그램이 실행되면 현재 directory에 binary file이 존재하는지 먼저 확인한다. 만약 이미 binary file이 존재한다면 그 파일을 그대로 사용하고, 존재하지 않는다면, 새로 database를 생성해야 한다. database 파일이 이미 있는지 확인하는 함수는 [[Functions#`check_and_init_DB`|check_and_init_DB]] 참조 
- 데이터베이스가 이미 존재하는 경우에는 데이터베이스에 존재하는 binary file을 메모리로 로드하여 사용할 수 있다. 참조: [[Functions#`load_DB`|load_DB]], [[Functions#`load_node_from_file`|load_node_from_file]]

# Database 생성
- 다음과 같이 `uchar**` 포인터를 사용하여 node에 대한 정보들을 저장한다.
- `uchar*`는 byte array의 시작주소를 가리키는 pointer이다.
```c
    uchar** Core;
```
- pointer를 사용하여 데이터가 저장된 메모리 위치만 참조하므로, node data의 size를 포인터가 가리키는 메모리 주소의 처음 부분에 저장해야 한다. node size를 나타내기 위해 4바이트를 사용한다. 
- 채널 개수를 나타내기 위해 2바이트를 사용하고, 최소 채널이 1개 있어야 하므로, 채널 1개를 나타내기 위해 4바이트를 사용하고, 처음에는 채널0에 아무 연결 관계도 저장하지 않기 때문에, axis 개수로 2bytes 만 지정하면 된다. 총 12 bytes가 필요하며 다음과 같이 초기화하면 된다. 처음에는 256개의 node를 생성해야 한다. 256개의 노드를 생성하는 함수는 다음 참조: [[Functions#`create_new_node`|create_new_node]], [[Functions#create_DB|create_DB]]

# Database 저장
- 데이터 베이스는 기본적으로 HDD 또는 SSD 등 영구적인 저장장치에 저장되어 있어야 하며, RAM에는 일시적으로 필요한 데이터만 올려서 사용해야 한다. 따라서 새로운 Node를 생성하거나 기존의 node data를 수정할 때마다 데이터베이스가 저장된 binary file의 내용을 적절하게 수정해야 한다. 
- 저장되는 banary file은 총 2가지이다. 하나는 진짜 node 및 channel간의 연결관계가 저장되어 있는 database file이고, 다른 하나는 각 node의 인덱스와 해당 인덱스의 node data가 저장되어 있는 database file 내에서의 offset을 mapping해 주는 데이터가 저장된 banary file이다. 이 file은 프로그램 실행시 RAM에 모두 올려 놓고 사용해도 된다(용량이 크지 않기 때문, 물론 이것마저도 용량이 클 정도로 데이터가 많이 쌓인다면 램에 모두 올리지 않고 필요할 때만 참조할 수도 있다).
## Binary Files 구조
1. data.bin
   - 실제 node data가 저장되는 파일
   - 각 node의 데이터가 연속적으로 저장됨
   - 각 node의 데이터는 size(4bytes) + 실제 데이터로 구성
2. map.bin
   - node index와 data.bin에서의 offset을 매핑하는 파일
   - 파일 시작에 전체 node 개수(4bytes) 저장
   - 각 node의 offset 정보가 index 순서대로 저장(각 8bytes)
## 저장 구현
- [[Functions#`save_node_to_file`|save_node_to_file]], [[Functions#save_DB|save_DB]] 함수 참조.
- 
이러한 구조를 통해 프로그램이 다시 시작될 때 map.bin 파일을 읽어서 각 node의 위치를 빠르게 찾을 수 있으며, 필요한 node의 데이터만 data.bin 파일에서 읽어올 수 있다.

# Initialization
- binary-data folder가 있는지 확인하고 없으면 생성한다. 
- 다음, data.bin과 map.bin file이 있는지 확인한다. 둘 중 하나라도 없으면 database를 새로 생성하고 저장한다(이 때 map.bin도 같이 저장되어야 함) database를 새로 생성할 때에는 반드시 CoreMap도 함께 생성해야 한다. 
```c
    // Check if map.bin exists
    FILE* map_file = fopen(MAP_FILE, "rb");
    FILE* data_file = fopen(DATA_FILE, "rb");
    
        if (!map_file || !data_file) {
        // Need to create new database
        if (map_file) fclose(map_file);
        if (data_file) fclose(data_file);
        create_DB();
        save_DB();
        return DB_NEW;
    }
```
- create_DB 함수는 다음과 같이 작성한다.  이때 offset도 적절하게 setting해 주어야 한다. init_core_mapping 함수에서 모든 offset을 0으로 초기화해 버리기 때문이다.
```c
void create_DB() {
    printf("Creating new database...\n");
    Core = (uchar**)malloc(MaxCoreSize * sizeof(uchar*));
    init_core_mapping();
    for (int i = 0; i < 256; ++i) {
        create_new_node(i);
        CoreMap[i].core_position = i;
        CoreMap[i].is_loaded = 1;
        CoreMap[i].file_offset = 4 + (16 * i);  // Each node starts with 16 bytes, plus 4 bytes header
        CoreSize++;
    }
}
```

- 프로그램이 실행되면 먼저 데이터가 저장된 binary file들이 있는지, 있다면, RAM에 올려야 하는 데이터들을 읽어서 RAM에 올리는 등 초기화 작업을 진행해야 한다. 참조 : [[Functions#`init_system`|init_system]]
- map.bin file이 있는지 확인하고, 없으면 생성한다. map.bin file이 있으면 CoreMap에 mapping information을 올린다. 
- data.bin file이 있는지 확인하고, 없으면 database를 새로 생성한다(참조: [[Functions#`create_DB`|create_DB]]). 데이터베이스를 새로 생성하는 경우에는 data.bin 파일로 저장을 해 두어야 한다. data.bin file이 있는 경우에는 map.bin file을 참조하여 index 0부터 255까지 Core variable에 올린다. 
- free-space.bin file이 있는지 확인하고, 없으면 생성한다. 없는 경우에는 FreeSpace를 초기화한 뒤에 binary file을 저장해 놓는다. 

# [[Command Line Interface(CLI)]]

# CoreMap
- data.bin file에서 node index와 node offset을 mapping해 주는 data structure이다. 
- free space에서 node space resize할 때마다 반드시 CoreMap을 update해 주어야 한다. 그래야 프로그램 재시작 시 node offset을 정확하게 찾아서 읽을 수 있다. 
- CoreMap의 정보를 확인하는 command: 