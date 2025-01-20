# Create Cycle
- input으로 연결될 (node_index, ch_index) pair들의 정보와, 연결할 axis number를 받아와서, (node, ch) pair들을 순서대로 create_link 함수를 이용해서 연결하여 cycle을 완성하면 된다. 
# Sentence
- 문장(sentence)은 하나의 cycle로 이루어진다. sentence는 token의 연결로 이루어지므로, sentence data를 읽는 방식은 sentence cycle을 구성하는 vertex의 token data들을 읽어서 연결하는 과정으로 설명할 수 있다. 
## Create Sentence
### Handle create sentence
- sentence를 생성하기 위해서는 먼저 주어진 data를 tokenize 해야 한다. 이를 위해서 search_token 함수를 사용한다([[Vertex#Search Token|Search Token]]). 
```c
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

    TokenSearchResult *result_first = search_token(current_pos, remaining_len);
```
- 만약 input으로 들어온 data와 완전히 일치하는 token이 한 번에 발견되면, 더이상 코드를 진행시킬 필요가 없다. 그냥 해당 token vertex에 빈 channel을 하나 찾거나 새로 만들어서 loop를 만들어 주면 된다. 
```c
    if (remaining_len == (size_t)result_first->matched_length)
    { // if the remaining length is 0, then we are at the end of the string
        printf("result->matched_length == remaining_len\n");
        int channel_index = recycle_or_create_channel(result_first->vertex_index);
        if (channel_index == CHANNEL_ERROR)
        {
            printf("Error: Failed to create channel for vertex %u\n", result_first->vertex_index);
            free_search_result(result_first);
            return ERROR;
        }
  
        if (create_loop(result_first->vertex_index, channel_index, 2) != LINK_SUCCESS)
        {
            printf("Error: Failed to create loop for vertex %u\n", result_first->vertex_index);
            free_search_result(result_first);
            return ERROR;
        }
        free_search_result(result_first);
        return SUCCESS;
    }
```
- 그렇지 않은 경우는 최소한 token 2개 이상으로 구성된 sentence를 생성해야 하는 case이다. 
- 이 경우에는 input으로 들어온 sentence data를 모두 tokenize 완료할 때까지 반복문을 돌아야 하는데, 남은 길이는 `remaining_len`변수로 관리하며 이 값이 0이 되면 반복문을 탈출한다. 
```c
    bool need_search = true;
    // Tokenize input using search_token
    while (remaining_len > 0 && count < MAX_SENTENCE_TOKENS) {
        TokenSearchResult *result = search_token(current_pos, remaining_len);
```
- 먼저 경우를 두 가지로 나눈다. 반복문을 도는 과정에서 연속되는 token이 발견된 경우와 그렇지 않은 일반적인 경우이다. 
- repeat token이 발견된 경우에는 recycle_or_create_channel이 아니라 직접 create_channel을 호출하고 `channels[count]`에 이전 token의 channel보다 1 증가시킨 값을 대입한다. 그 이유는 recycle_or_create_channel 함수는 axis_count=0인 channel을 찾으면 재활용하려고 하는데, 아직 `channels[count-1]`은 현재 channel과 link가 연결되지 않은 상태이기 때문에, recycle_or_create_channel 함수는 `channels[count-1]`과 같은 값을 반환한다. 
```c
        tokens[count] = result->vertex_index;
        if (tokens[count] == tokens[count-1]) // repeat token
        {
            create_channel(tokens[count]);
            channels[count] = channels[count-1] + 1;
        }
        else // ordinary token sequence
        {
            channels[count] = recycle_or_create_channel(result->vertex_index);
        }
```
- 그 다음 `prev_toekn`과 `current_token`을 axis 2로 연결시킨다.
- `count=0`인 경우에는 처음 loop를 도는 상황이기 때문에 prev_token이 없으므로 이 과정은 건너 뛴다. current_pos, remaining_len을 업데이트하고 count++ 이후 다음 loop로 이동하면 된다. 
```c
        need_search = true;
        ushort prev_channel_count = 0;
        if (count > 0) {
        ...
        }
        current_pos += result->matched_length;
        remaining_len -= result->matched_length;
        count++;
        free_search_result(result);
```
- count >0인 경우
- 이 경우에는 prev_token이 이전 반복문에서 할당되었기 때문에, prev_token과 current_token을 연결시킨다.  [[Link#Create Link|Create Link]]
```c
create_link(tokens[count - 1], channels[count - 1], tokens[count], channels[count], 2); // create a link between the previous token and the current token
```
- 이후에는 prev_token의 channel을 탐색하면서 tokens[count]와 같은 token과 연결된 token이 있는지 탐색한다. 만약 있다면 두 개의 연속된 token을 가진 서로 다른 sentence가 생성되는 것인데, 이 경우 동일한 순서의 두 개의 token을 합쳐서 새로운 token을 만들고 기존의 token으로 연결된 sentence는 새로운 token으로 대체하면 sentence를 구성하는 token의 수를 줄일 수 있고, database의 저장공간을 줄일 수 있다. 
- 하지만 prev_token의 channel_count가 2인 경우에는 ch 1밖에 없는 상태이고 이 channel은 현재 생성하려는 sentence에 속하기 때문에 channel 탐색을 진행할 필요가 없으므로 건너뛴다. 
```c
            prev_channel_count = get_channel_count(Core[get_vertex_position(tokens[count - 1])]);
            if (prev_channel_count <= 2)
                need_search = false;
```
- 그 이외의 경우, 즉 channel count가 3 이상인 경우에는 탐색을 진행한다. 
- 탐색은 ch 1부터 prev_channel_count - 1까지 진행한다. ch 0을 제외한 모든 channel을 다 탐색한다. 다만 이 channel 중에는 현재 생성중인 sentence를 구성하는 channel도 한 개 이상 포함되어 있다. 적어도 하나는 반드시 현재 생성 중인 sentence를 구성하는 prev_channel이다.
```c
            if (need_search)
            {
                uint prev_vertex = tokens[count - 1];
                // Check each channel for matching next token
                for (ushort ch = 1; ch < prev_channel_count; ch++)
                {
```
### ch 탐색할 필요가 없는 경우: 
1. axis_count = 2
- axis_count =2 인 경우는 clear_channel을 이용해 지워진 channel이므로 다음으로 가리키는 token이 없으므로 다음 loop로 바로 건너뛴다. 
```c
    if (get_axis_count(Core[get_vertex_position(prev_vertex)], 2) == 0)
    {
      printf("axis 2 is empty\n");
      continue; // skip if the axis 2 is empty
   }
```
 2. ch == channels[count - 1]
    - 탐색하려는 ch이 prev_channel인 경우에는 탐색을 할 필요가 없다. 어차피 다음으로 가리키는 token이 현재 생성하려고 하는 sentence의 토큰(`tokens[count]`)이기 때문이다. 이 경우도 loop를 건너 뛴다. 
```
                    if (ch == channels[count - 1])
                        continue; // skip the current channel
```
- 이후 각 ch이 가리키는 다음 token과 channel을 계산하고 next_token도 받아온다. 이 next_token이 현재 tokens[count]의 data와 같은지를 비교해서 token을 새로 생성할지 결정하게 된다. 
```c
   uint next_vertex;
   ushort next_channel;
   if (get_link(prev_vertex, ch, (ushort)2, (ushort)0, &next_vertex, &next_channel) != LINK_SUCCESS)   continue;                   
   char *next_token = get_token_data(next_vertex);
```
- 만약 비교 결과가 같다면 combined token을 새로 생성하고 channel을 2개 더 생성한다. 
- 새로 생성한 ch 중 하나는 기존에 있던 sentence를 구성하는 두 개의 token을 대체하기 위해 사용되고, 나머지 하나의 ch은 새로 생성중인 sentence를 구성하는 ch로 사용된다. 
```c
if (strcmp(next_token, result->token_data) == 0)
{
   // Create combined token
   int new_vertex = create_token_vertex(prev_vertex, result->vertex_index);
   if (create_multi_channels(new_vertex, 2) != CHANNEL_SUCCESS)
   {
       printf("Error: Failed to create channels for vertex %u\n", new_vertex);
       free_search_result(result);
       return ERROR;
   }
```






### create new token vertex
- search_token 함수를 이용해서 token vertex를 받아온 뒤에 remaining data가 있다면 받아온 token vertex의 ch 1부터 channel_count만큼 loop를 돈다. loop를 돌면서 각각의 ch의 axis 2에 연결된 다음 vertex들의 token data가 remaining data의 앞부분과 일치하는지 비교하여, 일치하는 ch이 발견되면 이 때 새로운 token vertex를 생성해야 한다. 그 이유는 두 token을 합쳐서 새로운 token을 생성하지 않으면 두 token의 나열된 순서가 동일한 두 개의 sentence cycle이 생성될텐데, 이 경우에는 두 개의 token을 합쳐서 하나의 token으로 만들면 sentence를 구성하는 token의 개수를 줄일 수 있고, 데이터를 더욱 적은 용량으로 저장할 수 있기 때문이다. 
- 새로 생성되는 token vertex는 create_channel을 이용해서 channel을 두 개 더 생성해야 한다. channel 1은 기존의 cycle을 구성하던 두 개의 token들을 대체하기 위해 사용되고, ch 2는 새로 생성되는 sentence cycle을 구성하기 위해 사용된다. 일단 token vertex를 생성한 뒤에 create_channel을 이용해서 하나의 channel만 생성하고, 새로 생성되는 sentence cycle은 해당 함수에서 알아서 channel을 생성하므로 결과적으로 2개의 channel이 추가된다. 
- 기존에 존재하던 cycle을 구성하는 두 개의 연속된 token vertex들을 새로 생성된 vertex로 대체하기 위해서는 cycle에서 두 개의 token에 대한 연결을 끊고, 새로운 vertex를 삽입하는 과정을 거쳐야 한다. 두 개의 token으로 연결된 path를 제거하는 과정은 delete path from cycle function을 이용하고, 새로운 token vertex 1개를 삽입하는 과정은 insert path into cycle 함수를 이용한다. 
- Search Token 함수를 이용해서 주어진 data를 token vertices array로 변환한 다음 sentence cycle을 생성하는 함수에게 작업을 넘기면 된다.([[Cycle#Create Cycle|create cycle]]) 
### Case 1: Two token cycles
- 만약 2개의 token만으로 이루어진 sentence cycle이 있는데(A, B token으로 이루어져 있다고 하자), ABC라는 sentence를 추가할 때에는 기존의 A->B->A로 구성된 cycle에서 A, B 두 개를 삭제하는 것은 불가능하므로, 그냥 AB token vertex를 새로 만들고, 기존의 AB cycle은 clear channel을 이용해 초기화한 뒤, AB token의 loop로 대체한다. 이 loop는 AB token의 ch 1이 axis 2를 이용해 자기 자신을 가리키는 구조를 갖게 된다. 물론 이와 별도로 새로운 AB->C->AB로 구성되는 cycle은 AB token의 ch 2를 사용하여 cycle을 생성하게 된다. 
### Case 2: sentence data = token data
- 새로 생성하려는 sentence data가 기존에 있는 token data와 동일한 경우의 처리가 문제된다. 
- 이 경우에는 기존에 있는 token vertex에 ch을 추가하여 추가한 ch의 axis 2로 loop를 생성하면 된다. 
```c
        TokenSearchResult* result = search_token(current_pos, remaining_len);
        if (!result) break;
        if (result->matched_length == remaining_len) {
            if (create_channel(result->vertex_index) != CHANNEL_SUCCESS) {
                printf("Error: Failed to create channel for vertex %u\n", result->vertex_index);
                return ERROR;
            }
            ushort channel_index = get_channel_count(Core[get_vertex_position(result->vertex_index)]) - 1;
            if (create_loop(result->vertex_index, channel_index, 2) != LINK_SUCCESS) {
                printf("Error: Failed to create loop for vertex %u\n", result->vertex_index);
                return ERROR;
            }
            return SUCCESS;
        }
```
## Get Sentence
- sentence data를 읽기 위해서는 sentence의 시작 vertex index와 channel index를 input으로 주어야 한다. 
- input으로 받은 vertex, ch의 axis 2(sentence axis)에 cycle이 존재하는지 확인하고 존재한다면 순차적으로 cycle을 돌면서 각각의 cycle을 구성하는 vertex에 대해 get token을 이용해 token data를 읽어서 cycle을 모두 돌 때까지 읽어들이면서 data를 이어주면 sentence data가 완성된다. 
- 같은 sentence cycle이더라도 진입점을 다르게 주면 token의 시작지점이 달라지므로 다른 sentence data가 출력될 수 있다. 
- 
# Insert Path Into Cycle
- 이미 존재하는 Cycle을 구성하는 특정 vertex, ch을 기준으로 일정 길이의 (vertex, ch)로 이루어진 Path data를 삽입하여 더 긴 새로운 Cycle을 생성하는 함수이다. 
# Delete Path from Cycle
- 이미 존재하는 Cycle을 구성하는 특정 vertex, ch을 기준으로 특정 개수의 (vertex, ch) 들을 Cycle로부터 제거하고 다시 Cycle을 이어붙여서 길이가 줄어든 새로운 Cycle을 생성하는 함수이다. 
- 제거된 Path를 구성하는 (vertex, ch)들은 서로에 대한 연결도 모두 끊고, axis_count를 0으로 초기화해야 한다.  이는 clear_channel 함수를 이용하면 된다. 