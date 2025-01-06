# Node
- [[Node]]
# Channel
- 채널은 노드마다 1개 이상 부여될 수 있는 개념이다. 같은 노드 내에서 서로 다른 채널을 가진 경우에는, Node에 저장된 데이터는 공유하지만, 각각의 채널은 서로 독립적이다. 따라서 한 채널에서의 연결 관계들은 다른 채널의 연결 관계들과 완전히 독립적이다.
- 각 노드는 적어도 하나의 채널을 가진다. 

## channel creation
- 채널을 새로 생성할 

# Axis
- 각 채널은 다른 노드의 채널과 link로 연결되는데, 이 때 axis라는 개념이 이용된다. axis는 link의 속성을 구분해 주는 개념이다. 예를 들면 forward link와 backward link는 서로 다른 axis(axis 0와 axis 1)으로 구분된다. forward 및 backward 뿐만 아니라 더 많은 axis를 정의하여 사용할 수 있다. 
- axis 3를 time axis로 정의하면 각 채널마다 axis 3로 채널이 생성된 시각에 대한 정보(8 bytes)를 저장하는 데이터와 연결시킬 수 있고, 그럼, 채널의 생성 시각, 수정시각 등 시간에 대한 정보를 forward, backward link들과는 독립적으로 관리할 수 있다. 이 데이터는 시각화할 때 별개의 UI를 적용하여 화면에 표시할 수도 있다.
## Axis 생성
- Axis를 생성하기 위해서는 생성하려는 node, channel 정보와, 생성하려는 axis의 종류를 함수에게 알려주어야 한다. axis의 종류는 번호로 구분된다. 0은 forward link, 1은 backward link, 이런 식으로 사전에 정의되어 있다. 
- channel data 내에서 axis에 대한 정보를 저장하는 방식은 다음과 같다.  먼저 axis의 개수를 2바이트로 나타낸다. 그리고 (6 bytes * axis의 개수)만큼 공간을 할당한다. 6 bytes는 axis number를 나타내는 2바이트와 axis data의 시작지점인 offset을 나타내는 4바이트로 이루어진다. offset의 기준점은 channel data의 시작점을 기준으로 한다. 즉, axis가 1개만 있다면, 첫번째 axis data의 offset은 2 + 6 = 8이 된다.  
- 처음에 생성된 node는 한 개의 channel을 가지지만 axis는 가지지 않는다. 따라서 처음 생성된 node에 link를 추가하는 등의 작업을 하려면 반드시 axis를 추가해 주어야 한다. 
- 처음 초기화할 때 channel은 1개 생성하지만, axis를 미리 생성하지 않는 이유는 axis는 종류가 있어서, 초기화 할 때 axis의 종류를 미리 지정하는 것은 큰 의미가 없기 때문이다. 
- axis 생성 함수는  [[Functions#create_axis|create_axis]] 참조.
- axis를 생성하는 과정에서 공간을 추가로 할당해야 하는 경우가 생길 수 있다. 이 경우에는 free space에 원하는 공간이 존재하는지 먼저 확인한 후, 존재하면 그 공간을 할당받아야 하고, 공간이 없는 경우에는 새로 저장공간을 할당받고 기존의 공간은 free space에 반납해야 한다. free space는 RAM에서의 공간을 관리하는 게 아니라 binary file에 저장되는 데이터 공간을 관리하는 객체이므로 RAM에서는 적절하게 기존 메모리 공간을 해제해야 한다. 
- 
# Link
- 각각의 node의 channel은 임의의 다른 node의 channel과 연결될 수 있다. 이를 link라는 개념으로 설명한다. 
- node의 channel offset을 이용해서 해당 channel의 데이터 시작점으로 이동하면, axis의 개수에 대한 정보가 저장되어 있다. 그리고 axis의 offset을 이용해서 axis의 정보가 시작하는 위치로 이동하면 link 정보가 저장되어 있다. link는 해당 채널이 가리키는 다른 node, channel 쌍에 대한 정보를 가지고 있다. node를 나타내기 위해 4바이트를, channel을 나타내기 위해 2바이트를 사용한다. 따라서 하나의 link를 나타내기 위해서 총 6바이트가 필요하다.
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. 
## Link 생성
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. [[Graph Based Database#Axis 생성|Axis 생성]]
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
- 프로그램이 실행되면 먼저 데이터가 저장된 binary file들이 있는지, 있다면, RAM에 올려야 하는 데이터들을 읽어서 RAM에 올리는 등 초기화 작업을 진행해야 한다. 참조 : [[Functions#`init_system`|init_system]]
- 먼저 binary-data folder가 있는지 확인하여 없으면 생성한다. 
- map.bin file이 있는지 확인하고, 없으면 생성한다. map.bin file이 있으면 CoreMap에 mapping information을 올린다. 
- data.bin file이 있는지 확인하고, 없으면 database를 새로 생성한다(참조: [[Functions#`create_DB`|create_DB]]). 데이터베이스를 새로 생성하는 경우에는 data.bin 파일로 저장을 해 두어야 한다. data.bin file이 있는 경우에는 map.bin file을 참조하여 index 0부터 255까지 Core variable에 올린다. 
- free-space.bin file이 있는지 확인하고, 없으면 생성한다. 없는 경우에는 FreeSpace를 초기화한 뒤에 binary file을 저장해 놓는다. 