# resize_node_space test
- node space를 재할당할 때 free space에 필요한 크기의 free block이 있으면 이를 new node에 할당하고, free space에서 삭제하며, 기존의 old node space는 free space에 추가하는 작업이 정상적으로 이루어지는지 확인하는 테스트
- 다음과 같이 명령어를 작성하여 16바이트와 32바이트의 free space가 생기도록 한다. 
```shell
create-axis 0 0 1
create-axis 0 0 2
create-axis 0 0 3
```
- 위와 같이 3개의 axis를 생성하면 free space에는 다음과 같이 16바이트와 32바이트 block이 추가된다. 
```shell
Free Space Information:
Total free blocks: 2
Free node indices: 0

Free Blocks:
Size (bytes)    Offset
------------    ------
16                0x00000000
32                0x00001000
```
- 이 상태에서 node 1에 axis 1을 추가하면 32바이트(0x00001000) block을 node 1에 새로 할당해 주고, 기존의 node 1의 16바이트(0x00000010)을 다시 free space에 저장해야 한다. 
- 위와 같은 과정이 정상적으로 진행된다면 다음과 같은 free block이 free space에 남아있어야 한다. 
```shell
Free Space Information:
Total free blocks: 2
Free node indices: 0

Free Blocks:
Size (bytes)    Offset
------------    ------
16                0x00000000
16                0x00000010
```

# create and delete axis test
- axis를 0부터 임의의 수까지 생성했다가 다시 모두 삭제한 뒤에 처음 상태로 정보가 올바르게 전환되는지 테스트하는 코드이다. 
- 인자로 node, channel index, 생성할 axis의 최대 번호를 받아야 한다. 
# 여러 개의 channel이 있는 상태에서 axis 생성 및 삭제
- channel을 100개 생성 후, 각 채널마다 axis를 10개 생성하고 삭제한다. 
# free block offset test
- free space의 free block이 여러 개 저장되는 경우에도 free block들의 offset은 절대 겹치는 일이 발생해서는 안된다. 동일한 offset을 가지는 free block이 생기는 것은 명백하 오류이다. 이것을 테스트 하는 코드가 필요하다. 

# create link test
- 특정 node, channel, axis에서 다른 node, channel로  link를 100개 임의로 생성하며, 올바르게 link가 생성되는지 확인한다. 
- link를 생성하면서 확인해야 하는 항목은 link를 생성할 때마다 link count가 적절하게 증가하는지, link data가 올바르게 저장이 되는지이다. 
# create and delete link test
- 특정 node, channel, axis에서 다른 node, channel로  link를 100개 순차적으로 생성하고, 생성한 link를 다시 모두 제거한 다음 처음 상태로 올바르게 돌아오는지 확인한다. 