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
- 