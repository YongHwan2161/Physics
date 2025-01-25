# Insert data
- 메모리상의 `char*` 배열의 특정 위치에 다른 `char*` 배열을 삽입하는 함수이다. 
- 삽입하는 위치보다 뒤에 있는 데이터들은 삽입되는 데이터의 크기만큼 밀려난다. 이는 `memmove`를 이용해 구현할 수 있다. 
# node data unload
- 메모리에서 자주 사용되지 않는 node data는 해제할 수 있어야 한다. binary file에는 모든 data가 저장되어 있으므로, 다음에 필요하면 binary file에서 load하면 된다. 
## data unload 과정
- unload하려는 node index를 받아오면, CoreMap에서 이를 unload하고, 메모리를 해제한다. 
# Synchronize RAM and binary files
- CoreMap 또는 `Core[node_index]`가 변화하면 무조건 해당하는 node_index 정보를 binary file과 동기화해야 한다. 그렇지 않으면 프로그램 실행 중에는 정상적으로 작동하다가도 프로그램을 종료 후 다시 실행할 때는 binary file에서 data를 불러와서 다시 Core에 load하기 때문에 이전과 다른 상태가 되거나 에러가 발생한다. 
- 이상적인 방법은 Core, CoreMap이 변할 때마다 무조건 binary file과 동기화하는 것이다. 그리고 안전 장치로, 프로그램을 종료할 때 먼저 동기화를 한 뒤 종료하는 것이다. 
- 특정 node data를 binary file과 동기화하는 함수는 [[Memory 관련 함수#save_node_to_file|save_node_to_file]] 이다.
- 동기화해야 하는 작업: node data가 변화할 때
  - create_link, delete_link, [[Axis 관련 함수#create_axis|create_axis]], [[Axis#Axis 삭제|delete_axis]], create_channel, clear_channel, create_new_node, change_vertex
  - 