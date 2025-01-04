# Node
- Node는 데이터를 구분하는 기본 단위이다. 
- Node는 고유한 index가 0부터 순차적으로 부여된다. 
- 처음 database가 초기화 될 때에는 0부터 255까지의 index가 자동으로 생성된다. 각각의 node는 index 번호가 node에 저장된 1바이트의 데이터를 나타낼 수 있다.  
- 각 node의 정보는 바이트 단위로 binary file에 저장이 된다. 프로그램 실행중에는 필요한 node에 대한 index정보를 가리키는 별도의 테이블을 이용해서 필요한 노드 데이터를 저장장치(ssd or hdd)로부터 읽어들일 수 있다. 
- 즉 데이터를 저장하는 데 필요한 바이너리 파일은 두 종류이다. 하나는 각각의 node data가 저장된 `data.bin` file이고, 다른 하나는 index마다 해당 index의 node data가 저장된 binarary file사에서의 offset을 mapping해 주는 `map.bin`파일이다. 
- 데이터는 index 단위로 구분된다. 즉, 프로그램 실행시에는 node data들의 vector로 메모리에서 관리할 수 있다. 각 vector의 요소들은 바이트 단위의 배열(unsigned char[])이다. 
- 각 노드 data의 시작은 data size를 저장하는 4byte로 시작한다. data size는 자기 자신은 제외한다. 즉 실제 node data를 나타내는데 100 bytes가 필요하다면 node size에는 96이 기록된다. 이렇게 하는 이유는 데이터를 읽거나 쓸 때 먼저 node size 4 bytes를 읽고, 읽은 값만큼 바로 읽거나 쓰면 되기 때문이다. node size에 4를 빼지 않은 값을 저장하면 데이터를 저장장치에 저장하거나 읽을 때 4 bytes를 항상 빼줘야하기 때문에 비효율적이다.
- data size 다음에는 channel 개수를 알려주는 2 bytes가 기록된다. 
- channel 개수 다음에는 각 채널의 offset을 가리키는 4 bytes * (채널 개수) 만큼의 데이터가 기록된다. 
- 채널 데이터에 접근하기 위해서는 채널 수 정보 다음부터 원하는 채널번호의 offset을 찾아서 해당 offset으로 이동하면 된다. offset은 node data의 시작점을 기준으로 계산한다. 

# Channel
- 채널은 노드마다 1개 이상 부여될 수 있는 개념이다. 같은 노드 내에서 서로 다른 채널을 가진 경우에는, Node에 저장된 데이터는 공유하지만, 각각의 채널은 서로 독립적이다. 따라서 한 채널에서의 연결 관계들은 다른 채널의 연결 관계들과 완전히 독립적이다.
- 각 노드는 적어도 하나의 채널을 가진다. 

# Axis
- 각 채널은 다른 노드의 채널과 link로 연결되는데, 이 때 axis라는 개념이 이용된다. axis는 link의 속성을 구분해 주는 개념이다. 예를 들면 forward link와 backward link는 서로 다른 axis(axis 0와 axis 1)으로 구분된다. forward 및 backward 뿐만 아니라 더 많은 axis를 정의하여 사용할 수 있다. 
- axis 3를 time axis로 정의하면 각 채널마다 axis 3로 채널이 생성된 시각을에 대한 정보(8 bytes)를 저장하는 데이터와 연결시킬 수 있고, 그럼, 채널의 생성 시각, 수정시각 등 시간에 대한 정보를 forward, backward link들과는 독립적으로 관리할 수 있다. 이 데이터는 시각화할 때 별개의 UI를 적용하여 화면에 표시할 수도 있다.
- 
# Database 생성
- 다음과 같이 `vector<uchar *>`의 데이터구조에 node에 대한 정보들을 저장한다. 
- `uchar *`는 byte array의 시작주소를 가리키는 pointer이다. 
```cpp
    std::vector<uchar *> Core;
```
- pinter를 사용하여 데이터가 저장된 메모리 위치만 참조하므로, node data의 size를 포인터가 가리키는 메모리 주소의 처음부분에 저장해야 한다. node size를 나타내기 위해 4바이트를 사용한다. 
- 