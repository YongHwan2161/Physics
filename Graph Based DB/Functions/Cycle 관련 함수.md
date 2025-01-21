# handle_create_cycle
- 먼저 tokens, channels 배열을 선언하고, count를 0으로 초기화하고,  current_pos와 remaining_len을 입력받은 데이터의 길이로 초기화한다. 
```c
#define MAX_SENTENCE_TOKENS 100

int handle_create_sentence(char* args) {
    if (!args || !*args) {
        print_argument_error("create-sentence", "<text>", false);
        return ERROR;
    }
    uint tokens[MAX_SENTENCE_TOKENS];
    ushort channels[MAX_SENTENCE_TOKENS];
    int count = 0;
    const char* current_pos = args;
    size_t remaining_len = strlen(args);
```
- [[Node#Search Token|search token]]을 이용해 일치하는 token을 찾는다. 
```c
        TokenSearchResult* result = search_token(current_pos, remaining_len);
        if (!result) {
            printf("Error: Failed to search token\n");
            return ERROR;
        }
```
## Case 1: token_data == sentence_data
- 입력받은 길이(remaining_len)가 탐색한 token의 length와 동일한 경우, 즉 입력받은 data와 동일한 data를 이미 저장하고 있는 token을 찾은 경우에는, 찾은 token vertex에 빈 channel을 찾거나 새로 ch을 추가하고([[Channel#recycle or create ch|recycle or create ch]]), 그 ch에 axis 2 loop를 추가한다. 
```c
        if (remaining_len == (size_t)result->matched_length) { // if the remaining length is 0, then we are at the end of the string
            printf("result->matched_length == remaining_len\n");
            int channel_index = recycle_or_create_channel(result->vertex_index);
            if (channel_index == CHANNEL_ERROR) {
                printf("Error: Failed to create channel for vertex %u\n", result->vertex_index);
                return ERROR;
            }
            if (create_loop(result->vertex_index, channel_index, 2) != LINK_SUCCESS) {
                printf("Error: Failed to create loop for vertex %u\n", result->vertex_index);
                return ERROR;
            }
            return SUCCESS;
        }
```
- remaining_len이 0이 될 때까지 반복문을 실행한다.
```c
    while (remaining_len > 0 && count < MAX_SENTENCE_TOKENS) {
```
- 첫 번째 반복문을 도는 경우(count == 0)
- 다시 token을 찾아서 `result`에 기록한다. 
```c
        TokenSearchResult *result = search_token(current_pos, remaining_len);
        if (!result) break;
        tokens[count] = result->vertex_index;
        channels[count] = recycle_or_create_channel(result->vertex_index);
        if (channels[count] == CHANNEL_ERROR) {
            printf("Error: Failed to create channel for vertex %u\n", result->vertex_index);
            free_search_result(result);
            return ERROR;
        }
```
- 만약 result->vertex_index의 ch_count가 2이면 ch 탐색을 할 필요가 없으므로, 값을 저장하고, 다음 반복문으로 jump한다. 
- `channels[count]`에 값을 저장할 때 [[Channel#recycle or create ch|recycle_or_create_channel]]을 사용하므로 ch_count = 2인 경우에도 더이상 ch 탐색을 할 필요가 없다.  
```c
        need_search = true;
        if (channel_count == 2) {
            need_search = false;
        }
```
- 두 번째 이상 반복문을 도는 경우(count > 0 && need_serarch)
- prev_vertex에 이전 반복문에서 찾았던 token vertex index를 대입한다. 
- 그리고 ch count만큼 반복문을 도는데, [[Link#get Link|get Link]]를 이용해서 다음으로 가리키는 next_vertex, next_channel 정보를 받아온다. 
```c
        if (count > 0 && need_search) {
            uint prev_vertex = tokens[count-1];
            // Check each channel for matching next token
            for (ushort ch = 1; ch < channel_count; ch++) {
                uint next_vertex;
                ushort next_channel;
                if (get_link(prev_vertex, ch, (ushort)2, (ushort)0, &next_vertex, &next_channel) != LINK_SUCCESS) {
                    continue;
                }
```
- next token data를 [[Node#get Token vertex data|get token data]]를 이용해 받아온다.
```c
                char* next_token = get_token_data(next_vertex);
                printf("next_token: %s\n", next_token);
                if (!next_token) continue;
```
- next token data가 result의 token_data와 같다면, 새로운 token을 생성해야 한다. 
- new_vertex에 ch을 하나만 생성하는 이유는 기존에 존재하는 sentence cycle에서 delete path and inset new vertex할 때 사용할 ch 1을 생성하는 것이고, 새로 추가될 sentence는 ch 2를 지나가지만 ch 2는 create senten cycle 함수에서 알아서 생성하므로 미리 생성할 필요가 없다. 
```c
                      if (strcmp(next_token, result->token_data) == 0) {
                        // Create combined token
                        int new_vertex = create_token_vertex(prev_vertex, result->vertex_index);
                        create_channel(new_vertex);
```
- new_vertex 가 제대로 생성되었다면, prev_token, ch을 지나는 cycle info를 받아온다. 
```c
                        if (new_vertex >= 0) {
                            cycleInfo* existing_cycle = get_cycle_info(prev_vertex, ch, 2);
```
- 만약 cycle count가 2이면, delete path form cycle을 이용해서 2개의 token을 삭제하는 게 불가능하다. 이 경우는 특수 case로, cycle의 모든 vertex, ch 을 clear하고 new vertex의 ch 1에 axis 2 loop를 생성한다. 만약 sentence 보다 상위 구조와 연관되어 있다면, new_vertex의 ch1을 기존의 prev_vertex, ch의 위치와 치환해야 한다(후속작업 필요).
```c
    if (existing_cycle && existing_cycle->count == 2) {
       printf("existing_cycle->count == 2\n");
       clear_cycle(existing_cycle);                               
       create_loop(new_vertex, 1, 2);
   }
```
- 그렇지 않은 경우에는 cycle에서 2개의 vertex token, ch을 뽑아내고, new_vertex, ch 1을 대시 삽입한다. 
```c
    else
    {
    delete_path_from_cycle(tokens[count - 1], channels[count - 1], 2, 2);
    if (existing_cycle)
        free_cycle_info(existing_cycle);
    uint new_path[1] = {(uint)new_vertex};
    ushort new_channels[1] = {0};
    insert_path_into_cycle(tokens[count - 1], channels[count - 1],
                           new_path, new_channels, 1, 2);
    }
```
- tokens array에서 prev token위치를 new token vertex로 바꾼다(실제로 생성되는 sentence token이므로)
- count--를 이용해 다음 루프에서 다시 count 위치에 새로운 token을 추가하도록 한다. 
```c
// Update tokens array
tokens[count-1] = new_vertex;
count--;
```
## Case 2: 반복되는 token
- input으로 주어진 sentence에 반복되는 token이 있는 경우 처리가 문제된다. 
- 예를 들면 "ABCABCABC"라는 문자열을 입력하는 경우를 생각해 볼 수 이다. 
- 