# create link error
- channel 0와 channel 1이 있는 상태에서 channel 0에 link를 추가하려고 하면 segmentation fault error가 발생한다. 
- `create_axis`를 이용해서 axis를 먼저 생성하고 나서 link를 생성해도 같은 에러가 발생한다. 
- 오류 원인: check_and resize node 함수 호출 이후에 node pointer를 변경해 주지 않아서 발생한 오류임. 공간 재할당 이후에는 반드시 이후 코드에서는 업데이트된 포인터를 이용해야 함.
```c
    int resize_result = check_and_resize_node(node, required_size, source_node);
    if (resize_result == FREE_SPACE_ERROR) {
        printf("Error: Failed to resize node\n");
        return LINK_ERROR;
    }
    node = Core[source_node];
```

# clear channel error
- ch 0과 ch 1을 생성 후, ch 0에 link를 하나 생성한 상태에서, ch 0을 clear하면, ch 1의 offset이 잘못 변경됨.
- 오류 원인 delete_size를 계산해서 channel offset을 변경하는데, 2를 빼야 하는데 더했음. 여기서 2는 axis count를 표현하기 위한 공간임.
```c
    uint delete_size = channel_end_offset - channel_offset - 2;
```
# node data