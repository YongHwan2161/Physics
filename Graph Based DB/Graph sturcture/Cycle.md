# Create Cycle
- input으로 연결될 (node_index, ch_index) pair들의 정보와, 연결할 axis number를 받아와서, (node, ch) pair들을 순서대로 create_link 함수를 이용해서 연결하여 cycle을 완성하면 된다. 
# Sentence
- 문장(sentence)은 하나의 cycle로 이루어진다. sentence는 token의 연결로 이루어지므로, sentence data를 읽는 방식은 sentence cycle을 구성하는 vertex의 token data들을 읽어서 연결하는 과정으로 설명할 수 있다. 
## Create Sentence
- Sentence를 생성하기 위해서는 input으로 sentence를 구성하는 token vertex index들의 정보를 주어야 한다. 
- input으로 들어온 token vertex index들을 순차적으로 create link 함수를 이용해 axis 2(sentence axis)로 연결하여 cycle을 형성한다. 여기서 ch index를 어떻게 정할지가 중요한다. 각각의 token vertex를 link로 연결할 때 우선 create channel을 이용하여 새로운 channel을 만들고 서로 연결하여 cycle을 형성해야 한다. 
- 기존 channel을 사용하지 않고 새로운 channel을 생성해야 하는 이유는, 기존 channel을 그대로 쓰면 기존 channel을 이용해서 형성한 다른 sentence cycle과 경로가 겹쳐버리기 때문이다. 
- sentence를 생성하기 위해서는 먼저 주어진 data를 tokenize 해야 한다. 이를 위해서 search_token 함수를 사용한다([[Vertex#Search Token|Search Token]]). 
- search_token 함수를 이용해서 token vertex를 받아온 뒤에 remaining data가 있다면 받아온 token vertex의 ch 1부터 channel_count만큼 loop를 돈다. loop를 돌면서 각각의 ch의 axis 2에 연결된 다음 vertex들이 remaining data의 앞부분과 일치하는지 비교하여, 일치하는 ch이 발견되면 이 때 새로운 token vertex를 생성해야 한다. 그 이유는 두 token의 나열된 순서가 동일한 두 개의 sentence cycle이 생성될텐데, 이 경우에는 두 개의 token을 합쳐서 하나의 token으로 만들면 sentence를 구성하는 token의 개수를 줄일 수 있고, 데이터를 더욱 적은 용량으로 저장할 수 있기 때문이다. 
- 기존에 존재하던 cycle을 구성하는 두 개의 연속된 token vertex들을 새로 생성된 vertex로 대체하기 위해서는 cycle에서 두 개의 token에 대한 연결을 끊고, 새로운 vertex를 삽입하는 과정을 거쳐야 한다. 
- Search Token 함수를 이용해서 주어진 data를 token vertices array로 변환한 다음 sentence cycle을 생성하는 함수에게 작업을 넘기면 된다.([[Cycle#Create Cycle|create cycle]]) 
- 
## Get Sentence
- sentence data를 읽기 위해서는 sentence의 시작 vertex index와 channel index를 input으로 주어야 한다. 
- input으로 받은 vertex, ch의 axis 2(sentence axis)에 cycle이 존재하는지 확인하고 존재한다면 순차적으로 cycle을 돌면서 각각의 cycle을 구성하는 vertex에 대해 get token을 이용해 token data를 읽어서 cycle을 모두 돌 때까지 읽어들이면서 data를 이어주면 sentence data가 완성된다. 
- 같은 sentence cycle이더라도 진입점을 다르게 주면 token의 시작지점이 달라지므로 다른 sentence data가 출력될 수 있다. 
- 
# Insert Path Into Cycle
- 이미 존재하는 Cycle을 구성하는 특정 vertex, ch을 기준으로 일정 길이의 (vertex, ch)로 이루어진 Path data를 삽입하여 더 긴 새로운 Cycle을 생성하는 함수이다. 
- 