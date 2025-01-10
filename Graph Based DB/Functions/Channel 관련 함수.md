
# get_channel_count
- channel 개수를 반환하는 함수.
- node data size를 나타내는 2바이트 다음의 4바이트가 actual node size를 나타내고 그 다음 2바이트가 채널 개수를 가리키므로 node + 6를 가리키는 지점에서 2바이트를 읽어 ushort로 변환하고 반환한다. 
```c
ushort get_channel_count(uchar* node) {
    return *(ushort*)(node + 6);  // Skip size power (2) and actual size (4)
}
```

# get_channel_offset
- node data의 포인터와 channel_index를 인자로 받아서 해당 channel의 offset을 int 형식으로 반환하는 함수
- channel_index가 channel_count보다 큰 경우에는 에러를 띄우고 프로그램을 종료한다. 나중에 성능 향상을 위해 이 코드 부분은 제거할 수도 있음
```c
uint get_channel_offset(uchar* node, int channel_index) {
    ushort channel_count = get_channel_count(node);
    if (channel_index >= channel_count) {
        printf("Error: Invalid channel index %d (max: %d)\n",
               channel_index, channel_count - 1);
        exit(1);  // Fatal error: invalid memory access
    }
    return *(uint*)(node + 8 + (channel_index * 4));  // 8: size_power(2) + actual_size(4) + channels(2)
}
```

# get_channel_size
- node data pointer와 channel_index를 인자로 받아서 channel size를 반환하는 함수. 
- 
```c
ushort get_channel_size(uchar* node, int channel_index) {
    uint offset;
    // This will exit if channel_index is invalid
    offset = get_channel_offset(node, channel_index);
    // Channel size is stored in first 2 bytes of channel data
    return *(ushort*)(node + offset);
}
```

# 