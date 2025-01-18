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
- [[Vertex#Search Token|search token]]을 이용해 일치하는 token을 찾는다. 
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
- 다시 token을 찾아서 `tokens[count], channels[count]`에 기록한다. 
```c
        result = search_token(current_pos, remaining_len);
        if (!result) break;
  
        tokens[count] = result->vertex_index;
        channels[count] = recycle_or_create_channel(result->vertex_index);
        if (channels[count] == CHANNEL_ERROR) {
            printf("Error: Failed to create channel for vertex %u\n", result->vertex_index);
            return ERROR;
        }
```