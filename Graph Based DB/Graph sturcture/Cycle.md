# Create Cycle
- input으로 연결될 (node_index, ch_index) pair들의 정보와, 연결할 axis number를 받아와서, (node, ch) pair들을 순서대로 create_link 함수를 이용해서 연결하여 cycle을 완성하면 된다. 
# Sentence
- 문장(sentence)은 하나의 cycle로 이루어진다. sentence는 token의 연결로 이루어지므로, sentence data를 읽는 방식은 sentence cycle을 구성하는 vertex의 token data들을 읽어서 연결하는 과정으로 설명할 수 있다. 
## Create Sentence
- Sentence를 생성하기 위해서는 input으로 sentence를 구성하는 token vertex index들의 정보를 주어야 한다. 
- input으로 들어온 token vertex index들을 순차적으로 create link 함수를 이용해 axis 2(sentence axis)로 연결하여 cycle을 형성한다. 여기서 ch index를 어떻게 정할지가 중요한다. 각각의 token vertex를 link로 연결할 때 우선 create channel을 이용하여 새로운 channel을 만들고 서로 연결하여 cycle을 형성해야 한다. 
- 기존 channel을 사용하지 않고 새로운 channel을 생성해야 하는 이유는, 기존 channel을 그대로 쓰면 기존 channel을 이용해서 형성한 다른 sentence cycle과 경로가 겹쳐버리기 때문이다. 
- 