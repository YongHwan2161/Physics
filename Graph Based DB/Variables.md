# `Core`
```c
uchar** Core;
```
- RAM에 올려진 node data들의 메모리상 위치를 가리키는 pointer들의 배열을 가리키는 pointer이다(이중 pointer). 
- 특정 index의 node data를 얻기 위해서는 먼저 Core에 이 node data가 저장되어 있는지 탐색한 다음 있으면 그 데이터를 가져오면 되고, 없다면, binary file에서 읽어들여야 한다. 이를 관리하기 위해서는 어떤 index의 node data가 현재 Core에 저장되어 있는지에 대한 상태를 추적할 수 있어야 한다. 이는 index와 Core 상에서의 위치(배열의 몇 번째에 해당 index의 node data를 가리키는 pointer가 저장되어 있는지)를 mapping시켜 주는 변수가 하나 더 필요하다. 이 변수는 binary file상에서 node data의 offset에 대한 정보를 저장하는 변수와는 다른 역할을 한다. 
- 
# `initValues`
- node를 새로 생성할 때 초기화되는 값을 미리 전역변수로 설정해 놓는다. 
```c
uchar initValues[16] = {
    4,  0,     // data size (2^4 = 16 bytes)
    1,  0,     // number of channels (1)
    8,  0, 0, 0,   // offset for channel 0 (starts at byte 8)
    0,  0,     // number of axes (0)
    0,  0, 0, 0, 0, 0    // remaining bytes initialized to 0
};
```