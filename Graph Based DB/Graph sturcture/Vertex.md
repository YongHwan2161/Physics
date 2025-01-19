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

# Specialized Vertices
## vertex 0 ~ 255
- vertex 0부터 255까지는 데이터베이스 생성시 자동으로 생성된다. 이들은 vertex_index 자체가 1바이트의 데이터를 표현한다. 
## vertex 256: Garbage Vertex
- 삭제된 vertex들이 저장되는 휴지통이다. 삭제된 vertex들은 나중에 vertex를 새로 생성할 때 재활용될 수 있다. 
# Token
## get Token vertex data
- Token vertex는 데이터가 저장되는 최소 단위이다. 각각의 Token vertex들은 고유한 데이터를 저장하고 있다. 
- Token vertex에 저장된 데이터를 읽어들이는 방법: 해당 token vertex index의 ch 0에서 axis 2에 있는 2개의 link 는 각각 해당 Token vertex의 데이터를 두 부분으로 나누어서 구성된 두 개의 token vertex index와 ch 0을 가리키고 있다. 이 두 개의 제 1 자식 token vertex의 데이터를 하나로 합치면 부모 vertex의 data가 된다. 두 개의 제1자식 token vertex의 데이터는 각각 두 개씩 총 4개의 제2 자식 token vertex를 axis 2로 가리키고 있고, 이 4개의 제2 자식 token vertex의 data를 연결하면 부모 vertex의 data가 되는 식이다.
- 위와 같은 방식으로 탐색을 하다가 vertex index 0에서 255 중 하나의 vertex index가 나타나면 그 때는 탐색이 종료된다. 그리고 해당 제 n 자식 노드는 vertex index 0에서 255 중 하나이고, data는 vertex index와 같은 0에서 255 중 하나로 결정된다. 즉, vertex 0~255까지는 실제 data를 1바이트 저장하고 있는 vertex들이라고 볼 수 있고, 이 방식을 사용하면 어떠한 token vertex에 대해서도 저장된 data를 읽어올 수 있다.
- 탐색 방법은 stack structure의 pop과 push를 사용하여 아주 효율적으로 구현이 가능하다. 
## create token 
- 새로운 token이 생성되기 위해서는 반드시 기존의 token data 두개를 연결해서 생성할 수 있는 token만 생성 가능하다. 기존의 token data가 '0xAA'와 0xBB이면, 0xAABB의 data를 저장하는 token은 생성 가능하지만, 0xAABBCC 와 같은 token은 생성하지 못한다. 0xAABBCC data를 저장하는 token을 생성하기 위해서는 0xAABB와 0xCC token이 기존에 존재해야만 가능하다(다른 조합도 가능하다, 0xAA와 0xBBCC 등).
- 새로운 token을 생성하는 함수는 input으로 기존에 존재하는 두 개의 token vertex index을 받아야 한다. 두 vertex의 순서가 중요하다. 순서가 바뀌면 새로 생성되는 token의 data가 변하기 때문이다.
- 새로운 token vertex를 생성해야 하므로, create vertex를 이용해서 새로운 vertex를 생성한다(new vertex라고 하자). 기존의 vertex들은 각각 first vertex, second vertex 라고 하자. 
- first vertex의 ch 0, axis 0을 이용해서 new vertex와 연결한다. axis 0은 기존에 존재하는 token을 탐색하기 위한 axis이다. 0xAA의 token이 존재하면, 0xAABB, 0xAACC 등 0xAA로 시작하는 모든 토큰들은 0xAA의 ch 0, axis 0에 link로 추가되어 있다고 볼 수 있다. 즉 axis 0으로 연결된 token이 있다면 그 token의 data는 무조건 그 전의 token data를 포함하여 더 많은 데이터를 저장하는 token이 된다. 이런 방식으로 data의 입력이 많아질 때, 공통되는 data를 하나의 token에 추상화하여 저장할 수 있고, 더 긴 data를 저장하는 token이 많아질 수록 database의 저장공간 효율이 증가하게 된다.
- 그다음 new vertex의 ch 0, axsi 1를 이용해서 first vertex와 second vertex를 연결한다. axis 1은 token data를 읽어들이기 위해 사용하는 axis이다.
## Create Token Logic
- token 생성은 사용자가 직접 조작할 일이 없어야 한다(테스트용을 제외하고). 즉, sentence를 생성할 때, token 생성이 필요하다면 자동으로 token이 생성되어야 한다. 그러기 위해서는 기존의 token 두 개를 합쳐서 새로운 token을 생성하는 조건을 지정해야 한다. 
- 새로운 token이 생성되는 조건은 특정 token vertex의 ch 1부터 ch을 증가시키면서 axis 2(sentence axis)를 탐색하다가, 해당 ch의 axis 2에 연결된 다음 token의 정보
## Search Token
- 일정 길이의 char* array를 input으로 받으면 data의 앞에서부터 시작하여 일치하는 token이 존재하는지 찾는 함수이다. 
- 맨 앞의 data가 0xAABBCCDD로 시작한다고 가정하면 먼저 vertex 0xAA의 ch 0, axis 0(token search axis)에 있는 link들을 탐색한다.  
- 각각의 link들이 가리키는 다음 vertex index들의 token data를 조사해서 0xBB로 시작하며 input data와 일치하는 token data가 있는지 조사해서 없으면 현재 token data와 vertex index를 반환하고, 일치하는 token data를 가진 vertex가 있으면 한 단계 더 들어가서 그 다음 다시 일치하는 token data를 가진 vertex를 찾는 과정을 더이상 일치하는 token data를 가진 vertex를 찾을 수 없을 때까지 탐색한 다음 결과를 반환한다. 
- 위 과정을 거치면 sentence를 구성하는 token의 개수를 char* array 의 size만큼이 아니라 훨씬 더 줄일 수 있고 database의 저장공간을 훨씬 줄이면서 데이터의 원본을 그대로 저장할 수 있게 된다. 
- 
# Vertex data loading
- node data는 기본적으로 binary file에 저장되어 있고, RAM에 올라와 있지 않다. 필요한 node data가 있으면 그 때마다 binary file에서 필요한 node data를 읽어와야 한다. 이렇게 하는 이유는 node data가 많아지면 그 모든 것을 RAM에 모두 올릴 수는 없기 때문이다. 
- node data를 찾을 때는 먼저 Core 변수에 node data가 저장되어 있는지 확인해야 한다. 확인후 이미 RAM에 올라와 있는 경우에는 Core에 있는 데이터를 그대로 가져오면 되고, 없는 경우에는 binary file에서 데이터를 불러와야 한다. 
- node data를 읽기 위해서는 node의 index와 offset을 알려주는 map data가 있어야 한다. 이 데이터를 이용해서 binary file에서 해당 node data가 위치한 offset으로 이동한 뒤 data size를 읽고(2 bytes), data size만큼 RAM으로 올리면 된다. 
- RAM에 올려진 node data들은 Core 변수에 저장된다. 참조: [[Variables#`Core`|Core]]
- 
# Vertex 생성
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

# Vertex 삭제
- 기존에 생성되어 있던 node를 삭제하고 싶은 경우 node의 데이터를 모두 지우면 되는데, binary file 내에서는 index 순으로 데이터가 저장되어 있기 때문에, 중간 지점의 index에 해당하는 node 데이터를 지운다고 해서, 그 뒤의 모든 데이터를 지운 데이터만큼 앞으로 이동시킬 수도 없고, index 번호를 변경하는 것도 비효율적이다. 따라서 이미 index가 부여된 node를 삭제하는 경우에는, 해당 node의 인덱스는 사라지지 않고, 단지 삭제된 것과 유사한 효과를 부여함으로써 관리해야 한다. 
- 삭제된 node는 garbage node에 따로 저장된다. garbage node는 256번 index를 부여받고, database 생성시 초기에 미리 생성되어 있다. 삭제된 node들은 garbage node와 link되는데, garbage node의 ch 0, axis 0이 삭제된 node_index, ch 0을 가리키도록 link가 생성되고, 삭제된 node들의 linked list의 마지막 node의 ch 0은 다시 garbage node의 ch0과 link되어 cycle을 형성하게 된다. 참조: [[Graphs and Their Representation#Cycle|Cycle]]
- 삭제된 node를 재활용하고 싶을 때는 garbage node의  ch 0이 가리키는 node가 있는지만 찾아서 가져오면 된다. 그리고 그 다음 node를 다시 garbage node와 연결시켜서 cycle을 다시 만들어 주면 된다. 
- garbage node의 ch 0부터 시작되는 cycle은 항상 다시 자신의 ch 0으로 돌아와야 하므로, 처음 garbage node를 초기화할 때에는 ch 0 axis 0이 자기 자신의 ch 0을 가리키도록 loop를 만들어 주어야 한다.  
## Node 삭제 과정
- Node 삭제 시 삭제하려는 node의 앞 부분 16 bytes를 [[Variables#`initValues`|initValues]]로 초기화한다. 참조: [[Vertex 관련 함수#initialize_node|initialize_node]]
```c
    uint node_position = CoreMap[node_index].core_position;        initialize_node(&Core[node_position]);
```
- Garbage node의 ch 0에서 삭제하려는 node의 ch 0으로 axis 0을 link하고, 원래 gargage node의 ch 0이 가리키던 node의 ch 0으로 삭제하려는 node의 ch 0을 link하면 된다. 
- 원래 garbage node의 ch 0이 가리키던 node는 garbage node의 ch 0 data에서 axis 0 data의 첫번째 link 위치에서 uint를 읽어들이면 된다. 
- 삭제한 node는 Core에서 unload한다. 
```c
    uint channel_offset = get_channel_offset(Core[GarbageNodeIndex], 0, 0);
    uint axis_offset = get_axis_offset(Core[GarbageNodeIndex], 0, 0);
    uint first_garbage_node = *(uint*)(Core[GarbageNodeIndex] + channel_offset + axis_offset + 2);
    delete_link(GarbageNodeIndex, 0, first_garbage_node, 0, 0);
    create_link(GarbageNodeIndex, 0, node_index, 0, 0);
    create_link(node_index, 0, first_garbage_node, 0, 0);
  
    Core[node_position] = NULL;
    CoreMap[node_index].core_position = -1;
    CoreMap[node_index].is_loaded = 0;
  
    CoreSize--;
}
```

# print-vertex
- vertex에 대한 정보를 출력하는 command이다. 
- 출력해야 하는 정보는 vertex size, vertex offset, core position, is loaded.
- 그리고 정보 출력시 channel count, channel offset, 그리고 각각의 channel에 대한 axis count, axis offset, 그리고 각각의 axis에 대한 link count, link data를 모두 알아보기 쉽게 정리해서 출력해야 한다. 

# allocated vertex data 
- node data는 power of 2 값으로 할당이 된다. 따라서 할당된 공간 중 일부는 유효한 값이 기록되지 않은 영역이 있을 수 있다. 할당된 공간 중 유효한 공간의 크기를 node data size를 기록한 2바이트 뒤에 4바이트를 할당하여 저장할 필요성이 있을까?
- 장점은 channel, axis, link 등을 추가할 때 새로 삽입된 데이터의 뒷부분 데이터들을 모두 이동시켜야 하는데, 이 경우 이동시켜야 할 size를 한번에 알아낼 수 있다. 
- 단점은 저장공간의 크기가 커진다. 모든 node마다 4바이트를 추가해야 하므로(별로 큰 손해는 아닌 것 같기도 함). 그리고 node data의 유효한 크기가 변경될 때마다 이 값을 함께 업데이트해야 하므로, 코드가 복잡해 질 수 있다(그러나 유효한 공간의 크기를 계산하는 코드가 더 복잡함... 마지막 channel의 offset에서 마지막 axis의 offset을 구하고, 마지막 axis의 link count를 구한 다음 최종 offset을 구해야 한다.) 유효한 size를 변경하는 코드는 한 줄이면 됨.
- 유효한 공간의 크기를 매번 계산하는 복잡함을 줄이는 대신 저장공간을 node마다 4바이트 추가하는 게 더 이득인 것 같기도 함.
- 초기화 할 때 initValues는 [[Variables#`initValues`|initValues]]와 같이 셋팅한다. 
## 위와 같이 변경하면 수정해야 하는 사항
- channel offset을 모두 4바이트 증가시켜야 한다. 이에 따라 [[Channel 관련 함수#get_channel_count|get_channel_count]]와 [[Channel 관련 함수#get_channel_offset|get_channel_offset]]를 수정해야 한다. 
- axis offset은 수정할 필요가 없다. axis offset은 channel offset을 기준으로 하여 상대적으로 결정되기 때문이다. 
- 
# Validate vertex
- node index가 유효한 범위 내에 있는지, node data가 Core에 존재하는지 확인하는 함수이다. 특정 함수를 호출하기 전에 먼저 node의 유효성을 확인한 다음 함수를 호출해야 한다. 호출한 함수에게 유효성 확인을 떠넘기지 말아야 한다. 예를 들면 create_axis 함수를 호출한다고 할 때, 함수를 호출하기 전에 먼저 node, channel, axis등의 유효성을 확인한 후 문제가 없는 경우에만 create_axis 함수를 호출해야 한다. 그렇지 않고, create_axis 함수 안에서 유효성을 모두 확인하라고 떠넘기면 create_axis 함수의 코드가 불필요하게 길어지게 되고 비효율적이다. 다른 함수를 호출할 때에도 마찬가지이다. 전달하는 인자에 대한 유효성을 먼저 확인한 후 문제가 없는 경우에만 함수를 호출하는 방식을 취해야 한다.

# Error Log
- [[Error handling#Garbage Node error|Garbage Node error]]
- [[Error handling#node data from file|node data from file]]