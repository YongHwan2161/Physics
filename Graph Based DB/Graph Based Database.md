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

# Link
- 각각의 node의 channel은 임의의 다른 node의 channel과 연결될 수 있다. 이를 link라는 개념으로 설명한다. 
- node의 channel offset을 이용해서 해당 channel의 데이터 시작점으로 이동하면, axis의 개수에 대한 정보가 저장되어 있다. 그리고 axis의 offset을 이용해서 axis의 정보가 시작하는 위치로 이동하면 link 정보가 저장되어 있다. link는 해당 채널이 가리키는 다른 node, channel 쌍에 대한 정보를 가지고 있다. node를 나타내기 위해 4바이트를, channel을 나타내기 위해 2바이트를 사용한다. 따라서 하나의 link를 나타내기 위해서 총 6바이트가 필요하다.
- link를 생성하기 위해서는 적어도 하나의 axis가 존재해야 한다. 
- link를 생성하는 함수는 인자로 source_node, source_ch, dest_node, dest_ch, num_axis를 받아야 한다. 그리고 인자로 받아온 axis 번호가 현재 node, ch data에 생성되어 있는 확인하고, 없다면 새로 axis를 만들어야 한다. 

# free space
- data.bin에 저장되는 데이터는 반드시 index 순서로 저장되는 것이 아니다. 또한 모든 저장공간이 차곡차곡 쌓여 있어서 빈 공간이 없는 구조도 아니다. 그 이유는 이미 특정 index의 node data가 할당되어 있는 상태에서 node data의 크기가 커지는 경우에는 새로 저장공간을 할당해 줘야 하는데, 이 때 이 하나의 node data  때문에, 나머지 index의 node data가 저장된 binary file을 수정하는 것은 매우 비효율적이기 때문이다. 이 때는 기존에 node data가 저장된 공간은 free space로 회수하고, binary file의 끝 위치 혹은 free space에서 관리하는 해당하는 크기의 공간을 새로 할당하는 방식을 사용해야 한다. 그래야 변경하려는 data의 node 정보만 수정하고 나머지 data들은 건들지 않을 수 있다. 
- free space는 binary file로 관리되어야 하며(그래야 프로그램 종료시에도 정보를 유지할 수 있으므로), 프로그램 실행시에는 모두 RAM에 올려놓고 사용한다(용량이 크지 않으므로 다 올려도 됨).
- free space binary file과 map.bin, 그리고 data.bin 파일은 수정사항이 발생할 때 모두 함께 업데이트 되어야 한다. 어느 하나만 업데이트 되고 나머지는 업데이트되지 않는다면 오류가 발생할 수 있으므로 반드시 세 binary file은 동기화 되어 있어야 한다. 
- free space는 삭제된 node index의 목록도 관리해야 한다. 삭제된 index는 나중에 재활용해야 하기 때문이다. 
## free space가 생기는 경우
### node data 삭제:
- node data를 삭제하면 삭제한 node data가 free space로 이동한다. 해당하는 node data의 size와 offset이 free space에 저장되는 것이다. 그리고 CoreMap에서는 node data를 unload하고, data.bin에서 해당 node data를 모두 0으로 초기화한다. 
### node data 수정
  - node data를 수정하는 과정에서 ch을 추가하거나, link를 추가하는 등의 작업으로 인해서 node data size가 증가하다가 기존에 할당된 크기를 넘어서는 경우에는 새로 공간을 할당해 줘야 한다. 이 때, 기존에 할당된 공간은 free space로 반납하고, 새로운 공간을 재할당해 줘야 한다. 
## free space가 줄어드는 경우
### node data 생성
- node를 새로 만드는 경우에는 먼저 16바이트 공간이 free space에 존재하는지 확인한 후, 있으면 그 공간을 그대로 활용해서 node를 생성한다. 이 때 node index는 삭제된 node index가 있는 경우에는 index를 재활용하고, 그렇지 않은 경우에는 새로운 index를 부여한다. 
- 
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

