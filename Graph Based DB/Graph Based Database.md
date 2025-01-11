# Node
- [[Node]]
# Channel
- 채널은 노드마다 1개 이상 부여될 수 있는 개념이다. 같은 노드 내에서 서로 다른 채널을 가진 경우에는, Node에 저장된 데이터는 공유하지만, 각각의 채널은 서로 독립적이다. 따라서 한 채널에서의 연결 관계들은 다른 채널의 연결 관계들과 완전히 독립적이다.
- 각 노드는 적어도 하나의 채널을 가진다. 

## channel creation
- 채널을 새로 생성할 

# [[Axis]]

# Link
- 각각의 node의 channel은 임의의 다른 node의 channel과 연결될 수 있다. 이를 link라는 개념으로 설명한다. 
- node의 channel offset을 이용해서 해당 channel의 데이터 시작점으로 이동하면, axis의 개수에 대한 정보가 저장되어 있다. 그리고 axis의 offset을 이용해서 axis의 정보가 시작하는 위치로 이동하면 link 정보가 저장되어 있다. link는 해당 채널이 가리키는 다른 node, channel 쌍에 대한 정보를 가지고 있다. node를 나타내기 위해 4바이트를, channel을 나타내기 위해 2바이트를 사용한다. 따라서 하나의 link를 나타내기 위해서 총 6바이트가 필요하다.
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. 
## Create Link
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
- last_axis가 아니거나, last_channel이 아니면 이동시켜야 할 데이터가 반드시 있다. 
- 먼저 link data를 삽입할 위치를 찾고, 앞에서 계산한 required_size - move_start 만큼의 데이터를 6바이트 앞으로 이동시켜서 link data 6bytes가 들어가 공간을 만든다. 
- 그 다음 현재 채널의 현재 axis보다 뒤에 저장된 axis들의 offset을 모두 6바이트 증가시켜야 하는데, axis는 정렬되지 않은 상태로 저장될 수 있기 때문에 axis_count만큼 loop를 돌 수밖에 없다. axis를 순서대로 정렬시키지 않는 이유는 axis 번호는 channel과 달리 axis마다 고유한 성질을 부여할 수 있기 때문(모든 axis가 순서대로 생성되는 게 아님)
- 현재 channel 이후의 모든 채널의 offset도 6 증가시켜야 한다(channel은 반드시 0부터 증가하면서 저장되기 때문에, 현재 채널보다 큰 채널만 다루면 됨)
```c
    // Move data forward if this is not the last position
    if (!is_last_channel || !is_last_axis) {
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
- 이후에는 link data를 삽입하고 link count를 1 증가시킨 다음, data.bin을 저장하면 된다. 
```c
    // Write link data at insert position
    memcpy(node + link_insert_offset, &link, sizeof(Link));
    // Update link count
    (*(ushort*)(node + channel_offset + axis_offset))++;
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