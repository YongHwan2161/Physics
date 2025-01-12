# create link error
- channel 0와 channel 1이 있는 상태에서 channel 0에 link를 추가하려고 하면 segmentation fault error가 발생한다. 
- `create_axis`를 이용해서 axis를 먼저 생성하고 나서 link를 생성해도 같은 에러가 발생한다. 
- 