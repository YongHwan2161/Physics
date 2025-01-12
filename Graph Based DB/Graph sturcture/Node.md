# Overview
- Node는 데이터를 구분하는 기본 단위이다. 
- Node는 고유한 index가 0부터 순차적으로 부여된다. 
- 처음 database가 초기화 될 때에는 0부터 255까지의 index가 자동으로 생성된다. 각각의 node는 index 번호가 node에 저장된 1바이트의 데이터를 나타낼 수 있다.  
- 각 node의 정보는 바이트 단위로 binary file에 저장이 된다. 프로그램 실행중에는 필요한 node에 대한 index정보를 가리키는 별도의 테이블을 이용해서 필요한 노드 데이터를 저장장치(ssd or hdd)로부터 읽어들일 수 있다. 
- 즉 데이터를 저장하는 데 필요한 바이너리 파일은 두 종류이다. 하나는 각각의 node data가 저장된 `data.bin` file이고, 다른 하나는 index마다 해당 index의 node data가 저장된 binarary file사에서의 offset을 mapping해 주는 `map.bin`파일이다. 
- 데이터는 index 단위로 구분된다. 즉, 프로그램 실행시에는 node data들의 vector로 메모리에서 관리할 수 있다. 각 vector의 요소들은 바이트 단위의 배열(unsigned char[])이다. 
- 각 노드 data의 시작은 data size를 저장하는 2byte로 시작한다. data size는 2의 n제곱 단위로 data의 size를 나타낸다. data size는 자기 자신도 포함한다. 즉 실제 node data를 나타내는데 100 bytes가 필요하다면 node size는 100보다 큰 가장 작은 2의 제곱수인 128 bytes가 할당되고, data size에는 7이 저장된다($2^7=128$ 이므로). 가장 작은 data size는 $2^4=16$ bytes이다. $\rightarrow$ [[Graph Based Database#Node 생성|Node 생성]] 참조.
- data size 다음에는 channel 개수를 알려주는 2 bytes가 기록된다. 
- channel 개수 다음에는 각 채널의 offset을 가리키는 4 bytes * (채널 개수) 만큼의 데이터가 기록된다. 
- 채널 데이터에 접근하기 위해서는 채널 수 정보 다음부터 원하는 채널번호의 offset을 찾아서 해당 offset으로 이동하면 된다. offset은 node data의 시작점을 기준으로 계산한다. 

# Specialized Nodes
## node 0 ~ 255
- node 0부터 255까지는 데이터베이스 생성시 자동으로 생성된다. 이들은 node_index 자체가 1바이트의 데이터를 표현한다. 
# Node data loading
- node data는 기본적으로 binary file에 저장되어 있고, RAM에 올라와 있지 않다. 필요한 node data가 있으면 그 때마다 binary file에서 필요한 node data를 읽어와야 한다. 이렇게 하는 이유는 node data가 많아지면 그 모든 것을 RAM에 모두 올릴 수는 없기 때문이다. 
- node data를 찾을 때는 먼저 Core 변수에 node data가 저장되어 있는지 확인해야 한다. 확인후 이미 RAM에 올라와 있는 경우에는 Core에 있는 데이터를 그대로 가져오면 되고, 없는 경우에는 binary file에서 데이터를 불러와야 한다. 
- node data를 읽기 위해서는 node의 index와 offset을 알려주는 map data가 있어야 한다. 이 데이터를 이용해서 binary file에서 해당 node data가 위치한 offset으로 이동한 뒤 data size를 읽고(2 bytes), data size만큼 RAM으로 올리면 된다. 
- RAM에 올려진 node data들은 Core 변수에 저장된다. 참조: [[Variables#`Core`|Core]]
- 
# Node 생성
- node를 생성할 때에는 $2^4=16$ bytes가 필요하다. 
-  처음 node를 생성하면, data size 2bytes + 채널 개수 2bytes + ch 0의 offset을 기록하는 4bytes + axis 개수를 나타내는 2 bytes(axis 개수는 처음에는 0) + 나머지는 0으로 초기화하여 적어도 16바이트가 필요하기 때문이다. 
- 초기값은 전역변수로 `initValues`에 저장해 놓고 이를 참조하여 메모리를 초기화한다. 참조: [[Variables#`initValues`|initValues]]
- node를 생성하는 함수는 [[Functions#`create_new_node`|create_new_node]] 참조.
## node 생성 과정
- node는 순차적으로 생성된다. 재활용할 수 있는 node가 있는지 먼저 탐색하고, 없다면 새로운 node를 생성해야 한다. 
- 새로 생성되는 node에 index를 부여하기 위해서는 현재 존재하는 node index 중 가장 큰 값을 알아야 한다. 전체 node index 개수를 관리하는 전역변수를 하나 선언해야 한다. node index의 개수는 map.bin file의 가장 처음 4바이트에 기록되어 있기 때문에 처음 map.bin을 로드할 때 전역변수에 이 값을 저장해 두었다가 필요할 때 사용 및 업데이트하고 map.bin file과 동기화해 주면 된다. 
```c
    extern unsigned int CurrentNodeCount;
```
- CoreMap 초기화 할 때 CurrentNodeCount를 업데이트한다. 
```c
    FILE* map_file = fopen(MAP_FILE, "rb");
    if (map_file) {
        // Skip number of nodes
        fread(&CurrentNodeCount, sizeof(uint), 1, map_file);
        fseek(map_file, sizeof(uint), SEEK_SET);
        // Read all offsets
        for (int i = 0; i < CurrentNodeCount; i++) {
            fread(&CoreMap[i].file_offset, sizeof(long), 1, map_file);
        }
        fclose(map_file);
    } else {
        printf("Error: Failed to open map.bin\n");
    }
```
- node를 추가할 때에는  [[Variables#`initValues`|initValues]]를 사용하여 새로운 node data를 생성한 후, `Core[CurrentNodeCount + 1]`에 node data의 포인터를 저장하고, `CoreSize`와 `CurrentNodeCount`를 1 증가시킨 후, CoreMap을 업데이트해야 한다. 
```c
    uchar* newNode = (uchar*)malloc(16 * sizeof(uchar));  // Always allocate 16 bytes initially
    printf("Creating new node at index %d\n", CurrentNodeCount);
    for (int i = 0; i < 16; ++i) {
        newNode[i] = initValues[i];
    }
    CurrentNodeCount++;
    Core[CurrentNodeCount - 1] = newNode;
    CoreSize++;
    CoreMap[CurrentNodeCount - 1].core_position = CurrentNodeCount - 1;
    CoreMap[CurrentNodeCount - 1].is_loaded = 1;
    uint last_node_size = 1 << (*(ushort*)Core[CurrentNodeCount - 1]);
    printf("Last node size: %d\n", last_node_size);
    uint file_offset = CoreMap[CurrentNodeCount - 1].file_offset + last_node_size;
    CoreMap[CurrentNodeCount - 1].file_offset = file_offset;
    save_node_to_file(CurrentNodeCount - 1);
    printf("Node created at index %d\n", CurrentNodeCount - 1);
```

# Node 삭제
- 기존에 생성되어 있던 node를 삭제하고 싶은 경우 node의 데이터를 모두 지우면 되는데, binary file 내에서는 index 순으로 데이터가 저장되어 있기 때문에, 중간 지점의 index에 해당하는 node 데이터를 지운다고 해서, 그 뒤의 모든 데이터를 지운 데이터만큼 앞으로 이동시킬 수도 없고, index 번호를 변경하는 것도 비효율적이다. 따라서 이미 index가 부여된 node를 삭제하는 경우에는, 해당 node의 인덱스는 사라지지 않고, 단지 삭제된 것과 유사한 효과를 부여함으로써 관리해야 한다. 
- 삭제된 node는 garbage node에 따로 저장된다. garbage node는 256번 index를 부여받고, database 생성시 초기에 미리 생성되어 있다. 삭제된 node들은 garbage node와 link되는데, garbage node의 ch 0, axis 0이 삭제된 node_index, ch 0을 가리키도록 link가 생성되고, 삭제된 node들의 linked list의 마지막 node의 ch 0은 다시 garbage node의 ch0과 link되어 cycle을 형성하게 된다. 참조: [[Graphs and Their Representation#Cycle|Cycle]]
- 삭제된 node를 재활용하고 싶을 때는 garbage node의  ch 0이 가리키는 node가 있는지만 찾아서 가져오면 된다. 그리고 그 다음 node를 다시 garbage node와 연결시켜서 cycle을 다시 만들어 주면 된다. 
- garbage node의 ch 0부터 시작되는 cycle은 항상 다시 자신의 ch 0으로 돌아와야 하므로, 처음 garbage node를 초기화할 때에는 ch 0 axis 0이 자기 자신의 ch 0을 가리키도록 loop를 만들어 주어야 한다.  
# print-node
- node에 대한 정보를 출력하는 command이다. 
- 출력해야 하는 정보는 node size, node offset, core position, is loaded.
- 그리고 정보 출력시 channel count, channel offset, 그리고 각각의 channel에 대한 axis count, axis offset, 그리고 각각의 axis에 대한 link count, link data를 모두 알아보기 쉽게 정리해서 출력해야 한다. 

# allocated node data 
- node data는 power of 2 값으로 할당이 된다. 따라서 할당된 공간 중 일부는 유효한 값이 기록되지 않은 영역이 있을 수 있다. 할당된 공간 중 유효한 공간의 크기를 node data size를 기록한 2바이트 뒤에 4바이트를 할당하여 저장할 필요성이 있을까?
- 장점은 channel, axis, link 등을 추가할 때 새로 삽입된 데이터의 뒷부분 데이터들을 모두 이동시켜야 하는데, 이 경우 이동시켜야 할 size를 한번에 알아낼 수 있다. 
- 단점은 저장공간의 크기가 커진다. 모든 node마다 4바이트를 추가해야 하므로(별로 큰 손해는 아닌 것 같기도 함). 그리고 node data의 유효한 크기가 변경될 때마다 이 값을 함께 업데이트해야 하므로, 코드가 복잡해 질 수 있다(그러나 유효한 공간의 크기를 계산하는 코드가 더 복잡함... 마지막 channel의 offset에서 마지막 axis의 offset을 구하고, 마지막 axis의 link count를 구한 다음 최종 offset을 구해야 한다.) 유효한 size를 변경하는 코드는 한 줄이면 됨.
- 유효한 공간의 크기를 매번 계산하는 복잡함을 줄이는 대신 저장공간을 node마다 4바이트 추가하는 게 더 이득인 것 같기도 함.
- 초기화 할 때 initValues는 [[Variables#`initValues`|initValues]]와 같이 셋팅한다. 
## 위와 같이 변경하면 수정해야 하는 사항
- channel offset을 모두 4바이트 증가시켜야 한다. 이에 따라 [[Channel 관련 함수#get_channel_count|get_channel_count]]와 [[Channel 관련 함수#get_channel_offset|get_channel_offset]]를 수정해야 한다. 
- axis offset은 수정할 필요가 없다. axis offset은 channel offset을 기준으로 하여 상대적으로 결정되기 때문이다. 
- 
# Validate Node
- node index가 유효한 범위 내에 있는지, node data가 Core에 존재하는지 확인하는 함수이다. 특정 함수를 호출하기 전에 먼저 node의 유효성을 확인한 다음 함수를 호출해야 한다. 호출한 함수에게 유효성 확인을 떠넘기지 말아야 한다. 예를 들면 create_axis 함수를 호출한다고 할 때, 함수를 호출하기 전에 먼저 node, channel, axis등의 유효성을 확인한 후 문제가 없는 경우에만 create_axis 함수를 호출해야 한다. 그렇지 않고, create_axis 함수 안에서 유효성을 모두 확인하라고 떠넘기면 create_axis 함수의 코드가 불필요하게 길어지게 되고 비효율적이다. 다른 함수를 호출할 때에도 마찬가지이다. 전달하는 인자에 대한 유효성을 먼저 확인한 후 문제가 없는 경우에만 함수를 호출하는 방식을 취해야 한다.