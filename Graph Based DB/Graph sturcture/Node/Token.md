# Integrate_token
- 우선 새로운 token node를 생성한다. ref. [[Node#create token node|create token node]]
```c
    int new_node = create_token_node(node_index, next_node);
    if (new_node == -1) return ERROR;
```
- 