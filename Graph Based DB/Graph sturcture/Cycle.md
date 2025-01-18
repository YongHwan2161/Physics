# Create Cycle
- input으로 연결될 (node_index, ch_index) pair들의 정보와, 연결할 axis number를 받아와서, (node, ch) pair들을 순서대로 create_link 함수를 이용해서 연결하여 cycle을 완성하면 된다. 
# Sentence
- 문장(sentence)은 하나의 cycle로 이루어진다. sentence는 token의 연결로 이루어지므로, sentence data를 읽는 방식은 sentence cycle을 구성하는 vertex의 token data들을 읽어서 연결하는 과정으로 설명할 수 있다. 
## Create Sentence
- sentence를 생성하기 위해서는 먼저 주어진 data를 tokenize 해야 한다. 이를 위해서 search_token 함수를 사용한다([[Vertex#Search Token|Search Token]]). 
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