# Insert data
- 메모리상의 `char*` 배열의 특정 위치에 다른 `char*` 배열을 삽입하는 함수이다. 
- 삽입하는 위치보다 뒤에 있는 데이터들은 삽입되는 데이터의 크기만큼 밀려난다. 이는 `memmove`를 이용해 구현할 수 있다. 
# node data unload
- 메모리에서 자주 사용되지 않는 node data는 해제할 수 있어야 한다. binary file에는 모든 data가 저장되어 있으므로, 다음에 필요하면 binary file에서 load하면 된다. 
## data unload 과정
- unload하려는 node index를 받아오면, CoreMap에서 이를 unload하고, 메모리를 해제한다. 