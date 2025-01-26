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
        if (!has_axis(node_index, i, STRING_AXIS)) continue;
        if (get_link_count(node_index, i, STRING_AXIS) == 0) continue;
        Vertex next_vertex = get_next_vertex(node_index, i, STRING_AXIS);
        if (is_start_string_vertex(next_vertex)) continue;
```
