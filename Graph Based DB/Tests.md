# resize_node_space
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
- 이 상태에서 node 1에 axis 1을 추가하면 32바이트(0x00001000) block을 node 1에 새로 할당해 주고, 기존의 node 1의 16바이트(0x0000)
- 그리고 node 1에서 axis를 추가해서 free space의 32바이트 block을 할당받고, 기존의 node 1의 16바이트 공간을 free space에 추가하는지 테스트 해야 한다. 이를 테스트하기 위해서는 free-space를 확인해서 제대로 FreeBlock이 관리되고 있는지 검토하도록 코드를 작성해야 한다. 
- 