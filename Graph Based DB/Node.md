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

# Node data loading
- node data는 기본적으로 binary file에 저장되어 있고, RAM에 올라와 있지 않다. 필요한 node data가 있으면 그 때마다 binary file에서 필요한 node data를 읽어와야 한다. 이렇게 하는 이유는 node data가 많아지면 그 모든 것을 RAM에 모두 올릴 수는 없기 때문이다. 
- node data를 읽기 위해서는 node의 index와 offset을 알려주는 map data가 있어야 한다. 이 데이터를 이용해서 binary file에서 해당 node data가 위치한 offset으로 이동한 뒤 data size를 읽고(2 bytes), data size만큼 RAM으로 올리면 된다. 
- RAM에 올려진 node data들은 Core 변수에 저장된다. 참조: [[Variables#`Core`|Core]]
- 
# Node 생성
- node를 생성할 때에는 $2^4=16$ bytes가 필요하다. 
-  처음 node를 생성하면, data size 2bytes + 채널 개수 2bytes + ch 0의 offset을 기록하는 4bytes + axis 개수를 나타내는 2 bytes(axis 개수는 처음에는 0) + 나머지는 0으로 초기화하여 적어도 16바이트가 필요하기 때문이다. 
- 초기값은 전역변수로 `initValues`에 저장해 놓고 이를 참조하여 메모리를 초기화한다. 참조: [[Variables#`initValues`|initValues]]
- node를 생성하는 함수는 [[Functions#`create_new_node`|create_new_node]] 참조.
# Node 삭제
- 기존에 생성되어 있던 node를 삭제하고 싶은 경우 node의 데이터를 모두 지우면 되는데, binary file 내에서는 index 순으로 데이터가 저장되어 있기 때문에, 중간 지점의 index에 해당하는 node 데이터를 지운다고 해서, 그 뒤의 모든 데이터를 지운 데이터만큼 앞으로 이동시킬 수도 없고, index 번호를 변경하는 것도 비효율적이다. 따라서 이미 index가 부여된 node를 삭제하는 경우에는, 해당 node의 인덱스는 사라지지 않고, 단지 삭제된 것과 유사한 효과를 부여함으로써 관리해야 한다. 
- 삭제된 node는 free space를 관리하는 별도의 자료구조에 의해 처리된다. node가 삭제되면 삭제된 node의 인덱스와 node size가 free space에 저장된다. free space에 저장된 저장된 공간들은 추후에 새로 node를 생성할 때 다시 재활용될 수 있다. 
- free space는 관리의 편리함을 위해서 2의 제곱 단위로 공간을 관리하며 최소 크기는 16 bytes가 될 것이다. 즉, 16, 32, 64, ... bytes 단위로 저장공간이 관리된다.  이를 위해서는 노드를 생성하거나 수정할 때에도 반드시 2의 제곱 단위로 저장공간을 관리해야 한다. 
