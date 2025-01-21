# [[Node]]
# [[Channel]]

# [[Axis]]

# [[Link]]
# [[Free Space]]

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
## root string 생성
- database 생성시 모든 요소들의 조상이 될 root element를 생성한다. root element는 하의 string이며, string의 시작점의 vertex를 기준으로 하위 elements가 생성될 것이다. 이 vertex의 위치는 (node, ch) pair로 표현될 수 있고, 이를 coordinate라고 부른다. root string의 coordinates를 알면 우리는 그 위치로 바로 이동할 수 있다. token이 저장된 layer보다 한 단계 더 높은 element layer에서는 모든 element들이 root를 기준으로 뻗어나가며, 모든 element들은 지정된 경로로만 탐색이 가능하며 root와 연결되어 있지 않은 다른 영역은 접근할 수 없다. 
- root string은 "Hello world!"로 생성한다. 생성 후 start vertex의 coordinates를 알고 있어야 한다. 그리고 이 coordinates를 현재 coordinates에 저장한다. coordinates는 전역변수로 관리되어야 하며, 현재 보고 있는 element의 위치를 가리킨다. 
- 
# [[Command Line Interface(CLI)]]

# CoreMap
- data.bin file에서 node index와 node offset을 mapping해 주는 data structure이다. 
- free space에서 node space resize할 때마다 반드시 CoreMap을 update해 주어야 한다. 그래야 프로그램 재시작 시 node offset을 정확하게 찾아서 읽을 수 있다. 
- CoreMap의 정보를 확인하는 command: 

# Data 입력
## Data 입력 과정
- 임의의 길이를 가진 data가 input으로 주어진다고 가정하자. 데이터는 [[Graph Based Database#Tokenization|토큰화]]되어 저장되고, 각각의 토큰은 연결되어서 cycle을 이루고, 데이터를 불러올 때는 이 cycle을 순회하면서 읽어들인 token들을 연결하여 원래의 데이터를 복원할 수 있다. 
- 
- 2 bytes data가 input으로 주어진다고 가정한다. 먼저, 처음 byte에 해당하는 vertex_index를 찾는다. 예를 들어, input data가 `250 251`이라고 하면, 처음에는 vertex 250으로 찾아간다. 
- 그다음 vertex 250의 ch 0, axis 0의 link data를 순회하면서 vertex 251이 존재하는지 확인한다. 존재하지 않으면, 
# Tokenization
- tokenize는 주어진 char 배열을 기존에 저장된 token node들로 쪼개서 표현하는 방법을 찾는 과정으로 진행된다. 
- 데이터베이스가 처음 생성되면 node 0~255까지가 token_node로 지정된다. 각각의 token_node들은 node_index와 같은 데이터를 표현하는 token이다. 
- tokenize 함수는 들어온 데이터를 token node의 순서로 표현하는 방법을 찾고, 각각의 token node의 ch 1부터 다음에 올 token node를 가리키는데, axis는 2로 연결한다. axis 2는 데이터를 표현하는 token들을 이어주는 line을 형성하는데 사용되는 axis로 설정한다. 
- 문제는 새로운 데이터를 저장할 때, 기존의 토큰들을 연결하여 데이터를 표현할 수 있는데, 데이터가 저장되었음을 나타내는 node를 새로 하나 추가할 것인지이다. 기존의 방식에서는 새로운 data가 추가될 때마다 node를 하나씩 새로 할당하였다. 그리고 새로 할당받은 node의 ch 0에서부터 token cycle이 axis 2로 연결되고, 해당 node의 데이터를 불러올 때에는  ch 0의 axis 2를 순회하면 되는 방식이었다. 이 방식을 그대로 사용하기로 한다. 
## Tokenize 순서
- 들어온 data의 첫 번재 byte에 해당하는 node_index와 ch 0을 저장한다. 