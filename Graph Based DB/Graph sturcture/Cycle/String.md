# Optimize_string_cycle
- 이미 생성된 string cycle이 input으로 주어지면, 이 string을 구성하는 token들 중 합칠 수 있는 token들이 있는지 탐색하여 있다면 새로운 token을 만들면서 이들을 합치는 작업을 한다.
- 들어온 vertices와 count에 대해서 integrate_token_data를 호출한다. 
```c
    int optimize_string_cycle(uint* vertices, int count) {
    for (int i = 0; i < count; i++) {
        if (integrate_token_data(vertices[i]) != SUCCESS) {
            printf("Error: Failed to integrate token data in node %u\n", vertices[i]);
            return CMD_ERROR;
        }
    }
    return CMD_SUCCESS;
```
- integrate_token_data 함수는 일단 들어온 node_index의 channel_count를 구한다. channel_count <= 1인 경우에는 탐색할 string이 없다는 의미이므로 함수를 종료한다. 
```c
int integrate_token_data(unsigned int node_index) {
    long node_position = get_node_position(node_index);
    if (node_position == -1) return CMD_ERROR;
    ushort channel_count = get_channel_count(Core[node_position]);
    if (channel_count <= 1) {
        return CMD_SUCCESS;
    }
    uint new_node = 0;
```
- current_vertex를 {node_index, i}로 정하고, axis가 없거나, link_count = 0인 경우에는 continue한다. next_vertex를 구해서 next_vertex가 만약 string을 시작하는 vertex인 경우에도 건너뛴다. string의 마지막 token과 string을 시작하는 token은 합쳐지면 안되기 때문이다(중요!!). 
- 반복문은 ch 1부터 channel_count -1까지 돈다.
```c
    for (int i = 1; i < channel_count; i++) {
        Vertex current_vertex = {node_index, i};
        bool new_node_created = false;
        Vertex next_vertex = get_next_vertex_check(node_index, i, STRING_AXIS);
        if (next_vertex.node == 0 && next_vertex.channel == 0) continue;
```
- 다음으로 ch i + 1부터 channel_count - 1까지 다시 반복문을 도는데, 일단 ch j의 next_vetex를 check하고, 만약 ch i와 ch_i의 vertex가 한 string 내에 속해 있는 경우(ABCCCCABDDDDD같은 경우 A와 B는 하나의 string cycle에서 같은 node_index를 가지고 ch만 다를 수 있음.)에는 반복문을 건너 뛴다. 이 경우에 token을 합치게 되면, 최소 두 개의 string을 비교하여 token을 합치는 게 아니라 하나의 string만 있어도 token이 합쳐지게 되고, 예측하기 어려운 결과가 발생할 수 있다. 어차피 같은 token 조합을 가진 다른 string이 나중에 input으로 들어오면 그 때 자동으로 합쳐지기 되므로 여기서는 건너뛴다.
```c
        for (int j = i + 1; j < channel_count; j++) {
            Vertex next_vertex2 = get_next_vertex_check(node_index, j, STRING_AXIS);
            if (next_vertex2.node == 0 && next_vertex2.channel == 0) continue;
  
            if (are_vertices_in_same_cycle(node_index, i, node_index, j, STRING_AXIS)) continue;
```
- 다음으로, 이제 어느 조건일 때 token을 합칠 것인지를 결정해야 한다. 서로 다른 string에 속한 2개의 token이 연속된 순서로 있는 경우에 이를 새로운 token으로 합치는 것이 규칙이다. 
- 이미 node_index가 같으므로, next_vertex.node가 서로 동일하다면 이는 새로운 token으로 합쳐도 된다고 판단할 수 있다. 
```c
    if (next_vertex.node == next_vertex2.node) {
```
- j ch 반복문을 돌면서 새로운 token은 단 한 번만 생성되어야 한다. 이를 보장하기 위해 new_node_create 변수를 이용한다. 
```c
   if (!new_node_created)
   {
       new_node = create_token_node(node_index, next_vertex.node);
       new_node_created = true;
   }
```
- j ch과 i ch이 새로운 token으로 합쳐질 수 있다면, 이후 i 반복문에서는 j ch을 탐색할 이유가 없다. 이런 중복을 방지히기 위해서 token을 합치기 전에 j 값을 visited_nodes 배열에 1로 저장해 놓는다. 이 값이 0이면 token을 합치는 과정이 진행되지 않은 ch이고, 1이면 이미 token을 합치는 과정이 진행되었으므로, 건너 뛰어도 된다는 의미이다. 
```c
visited_nodes[j] = 1;
```
- 이제 token을 합치는 과정이 남았다. 먼저 string cycle 정보를 받아오고, cycle_count가 1 이하인 경우는 건너뛴다. 0인 경우는 에러 상황이고, 1인 경우는 loop이므로 합칠 token이 없으므로 건너뛴다. 하지만 loop인 경우은 이미 앞에서 걸러지므로, 이 코드가 작동하는 경우는 에러 상황이라고 볼 수 있을 것이다.
```c
   cycleInfo *existing_cycle = get_cycle_info(node_index, i, 2);
   if (existing_cycle == NULL) continue;
   if (existing_cycle->count <= 1) continue;
```
- 모든 j ch을 돌면서 합칠 수 있는 token 순서쌍을 가지고 있는 ch들을 모두 조사한다. 그리고 이 ch 값들을 새로운 함수에 전달하여 그 함수에서 token 결합을 진행하기로 한다. 하나의 함수 내에서 이를 모두 처리하면 코드의 복잡도가 너무 증가하기 때문에 기능을 분리한다. 이는 [[Token#Integrate_token|Integrate_token]] 함수를 참조한다. 
- 