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
# Node 관련 Error
## node data from file
- node data를 해제한 뒤에 다시 load하고 나서 print-node 명령을 입력하면 segmentation fault error 발생.
- 원인: node data를 load하는 과정에서 Core[]의 index를 돌면서 null pointer를 찾아서 null pointer에 data를 load하도록 수정해야 한다. 
```c
    for (int i = 0; i < MaxCoreSize; i++) {
        if (Core[i] == NULL) {
            CoreMap[node_index].core_position = i;
            CoreMap[node_index].is_loaded = 1;
            load_node_from_file(data_file, offset, node_index);
            CoreSize++;
            break;
        }
    }
```
- Core array를 초기화할 때, 먼저 모두 null로 초기화하고 data를 load해야 한다. 
```c
    // Load initial set of nodes
    Core = (uchar**)malloc(MaxCoreSize * sizeof(uchar*));
  
    for (int i = 0; i < MaxCoreSize && i < 256; i++) {
        Core[i] = NULL;
        load_node_to_core(i);
    }
```
## Garbage Node error
- 프로그램을 종료했다가 재시작할 때마다 ch 0의 axis count가 1씩 증가함..
- 프로그램 시작 시 initialize_system 함수에서 axis_count를 증가시킨 다음 다시 저장하는 코드가 있는 부분을 찾아야 함.
- init_core_mapping 함수에서는 data를 읽어들이기만 하고 다시 저장하는 코드는 없음.
- initialize_database 함수 내에 Garbage node에 loop를 생성하는 코드가 추가되어 있어서 프로그램 실행할 때마다 loop를 새로 생성하려고 시도하는 것 같음.
- create_loop 함수를 create_DB 함수 안에만 넣어서 database를 생성할 때 한 번만 동작하도록 수정해서 해결.
## Garbage node 생성 오류
- database 생성시 , create_loop 함수를 이용해서 garbage node에 loop를 만드는데, 이때, node 256인 garbage node에서 공간 재할당이 일어나므로, 원래 offset인 0x1000의 16 bytes는 free space에 반납하고, 0x1010부터 32바이트를 새로 할당받으므로, data.bin의 offset 0x1010부터 32바이트가 추가로 기록되어야 하는데, 0x1000부터 32바이트가 기록됨. 
- 근데, 데이터 초기화 완료 후 직접 command handler를 이용해서 loop를 생성하면 정상적으로 동작함. create_DB 함수 안에서 loop를 생성할 때만 저장되는 offset에 오류 발생.
- free_space를 확인해 보면 제대로 free_space에 16바이트가 정확한 offset으로 저장되어 있음.
- create_axis 내에서 재할당이 이루어지므로, 문제의 원인은 create_axis 내에 있을 확률이 높음.
- 근데 프로그램 실행 이후에 command handler를 이용해서 axis 또는 link를 생성할 때는 멀쩡히 작동함.
- 