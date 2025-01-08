# resize_node_space
- node space를 재할당할 때 free space에 필요한 크기의 free block이 있으면 이를 new node에 할당하고, free space에서 삭제하며, 기존의 old node space는 free space에 추가하는 작업이 정상적으로 이루어지는지 확인하는 테스트
- 다음과 같이 명령어를 작성하여 16바이트와 32바이트의 free space가 생기도록 한다. 
```shell
create-axis 0 0 1
create-axis 0 0 2
create-axis 0 0 3
```
- 그리고 node 1에서 axis를 추가해서 free space의 32바이트 block을 
- 