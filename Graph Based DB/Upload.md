# Upload Json File
- json file을 한 번에 DB에 upload 하는 함수이다. 
- 재귀적으로 함수를 호출하여 json의 모든 내용들을 추출하면서 새로운 string을 생성해야 한다. 
- upload_from_json 함수는 input으로 json element와 parent_vertes를 받는다. 
```c
void upload_from_json(json_t *element, Vertex parent_vertex)
{
```
- json의 elements는 array, object, string으로 구분할 수 있으며, array인 경우에는 모든 array 요소들마다 다시 upload json file 함수를 호출한다. 
```c
    // If it's an array, process each element
    else if (json_is_array(element))
    {
        size_t index;
        json_t *value;
        json_array_foreach(element, index, value)
        {
            upload_from_json(value, parent_vertex);
        }
    }
```
- json eleement가 object인 경우에는 key, value로 분리할 수 있다. 
- 먼저 parent_vertex를 기준으로 key에 해당하는 string을 생성한다. 