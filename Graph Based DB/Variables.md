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