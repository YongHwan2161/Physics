# handle_command
- 명령어를 다루어 적절한 함수를 호출하도록 도와주는 함수이다. 로컬 환경에서는 CLI로 이용할 수 있고, web 환경에서도 동일한 명령어 체계를 사용하여 Web에서의 명령어도 다룰 수 있도록 확장할 예정이다. 
```c
int handle_command(char* command) {
    char* cmd = strtok(command, " \n");
    if (!cmd) return CMD_SUCCESS;
    char* args = strtok(NULL, "\n");
    if (strcmp(cmd, "create-axis") == 0) {
        return handle_create_axis(args);
    }
    else if (strcmp(cmd, "help") == 0) {
        print_help();
        return CMD_SUCCESS;
    }
    else if (strcmp(cmd, "exit") == 0) {
        return CMD_EXIT;
    }
    else {
        printf("Unknown command. Type 'help' for available commands.\n");
        return CMD_ERROR;
    }
}
```

# handle_create_axis
- node, ch, axis 정보를 입력하면 해당하는 axis를 node, ch data에 생성하는 함수이다. 
- axis.h의 [[Axis 관련 함수#create_axis|create_axis]] 를 호출한다. 
```c
int handle_create_axis(char* args) {
    int node_index, channel_index, axis_number;
    // Parse arguments
    if (sscanf(args, "%d %d %d", &node_index, &channel_index, &axis_number) != 3) {
        printf("Error: Invalid arguments\n");
        printf("Usage: create-axis <node_index> <channel_index> <axis_number>\n");
        return CMD_ERROR;
    }
    // Validate input
    if (node_index < 0 || node_index >= 256) {
        printf("Error: Node index must be between 0 and 255\n");
        return CMD_ERROR;
    }
    // Create the axis
    int result = create_axis(node_index, channel_index, axis_number);
    if (result == AXIS_SUCCESS) {
        printf("Successfully created axis %d in node %d, channel %d\n",
               axis_number, node_index, channel_index);
        return CMD_SUCCESS;
    } else {
        printf("Failed to create axis\n");
        return CMD_ERROR;
    }
}
```
# handle_check_axis
- 특정 node, ch에 특정 axis가 이미 존재하는지 확인하는 함수.
- axis.h의 [[Axis 관련 함수#has_axis|has_axis]]를 호출한다. 
```c
int handle_check_axis(char* args) {
    int node_index, channel_index, axis_number;
    // Parse arguments
    if (sscanf(args, "%d %d %d", &node_index, &channel_index, &axis_number) != 3) {
        printf("Error: Invalid arguments\n");
        printf("Usage: check-axis <node_index> <channel_index> <axis_number>\n");
        return CMD_ERROR;
    }
    // Validate input
    if (node_index < 0 || node_index >= 256) {
        printf("Error: Node index must be between 0 and 255\n");
        return CMD_ERROR;
    }
    // Get node and check if it exists
    if (!Core[node_index]) {
        printf("Error: Node %d does not exist\n", node_index);
        return CMD_ERROR;
    }
    // Get channel offset
    uint channel_offset = get_channel_offset(Core[node_index], channel_index);
    // Check if axis exists
    bool exists = has_axis(Core[node_index], channel_offset, axis_number);
    printf("Axis %d %s in node %d, channel %d\n",
           axis_number,
           exists ? "exists" : "does not exist",
           node_index,
           channel_index);
    return CMD_SUCCESS;
}
```
# handle_list_axes
- 특정 node, ch에 있는 모든 axes를 찾아서 반환한다. 
```c
int handle_list_axes(char* args) {
    int node_index, channel_index;
    // Check if arguments are provided
    if (!args) {
        print_argument_error("list-axes", "<node_index> <channel_index>", true);
        return CMD_ERROR;
    }
    // Parse arguments
    int parsed = sscanf(args, "%d %d", &node_index, &channel_index);
    if (parsed != 2) {
        print_argument_error("list-axes", "<node_index> <channel_index>", false);
        return CMD_ERROR;
    }
    // Validate input
    if (node_index < 0 || node_index >= 256) {
        printf("Error: Node index must be between 0 and 255\n");
        return CMD_ERROR;
    }
    // Get node and check if it exists
    if (!Core[node_index]) {
        printf("Error: Node %d does not exist\n", node_index);
        return CMD_ERROR;
    }
    // Get channel offset
    uint channel_offset = get_channel_offset(Core[node_index], channel_index);
    // Get axis count
    ushort axis_count = *(ushort*)(Core[node_index] + channel_offset);
    printf("\nAxes in node %d, channel %d:\n", node_index, channel_index);
    if (axis_count == 0) {
        printf("No axes found\n");
        return CMD_SUCCESS;
    }
    printf("Total axes: %d\n", axis_count);
    // List all axes by scanning through axis data
    printf("\nAxis numbers:\n");
    int axis_data_offset = channel_offset + 2;  // Skip axis count
    // Create an array to store axis numbers for sorting (optional)
    ushort* axis_numbers = (ushort*)malloc(axis_count * sizeof(ushort));
    // Read all axis numbers
    for (int i = 0; i < axis_count; i++) {
       axis_numbers[i] = *(ushort*)(Core[node_index] + axis_data_offset + (i * 6));
    }
    // Optional: Sort axis numbers for better readability
    for (int i = 0; i < axis_count - 1; i++) {
        for (int j = 0; j < axis_count - i - 1; j++) {
            if (axis_numbers[j] > axis_numbers[j + 1]) {
                ushort temp = axis_numbers[j];
                axis_numbers[j] = axis_numbers[j + 1];
                axis_numbers[j + 1] = temp;
            }
        }
    }
    // Print axis numbers with special labels for known types
    for (int i = 0; i < axis_count; i++) {
        const char* axis_type = "";
        switch(axis_numbers[i]) {
            case AXIS_FORWARD:
                axis_type = "(Forward link)";
                break;
            case AXIS_BACKWARD:
                axis_type = "(Backward link)";
                break;
            case AXIS_TIME:
                axis_type = "(Time axis)";
                break;
        }
        printf("- Axis %d %s\n", axis_numbers[i], axis_type);
    }
    free(axis_numbers);
    return CMD_SUCCESS
}
```

# handle_print_node
- node에 대한 정보를 출력해 주는 함수
-  node size, Core position(RAM에서의 index), file offset, load status, 그리고 node data를 16진수로 보여준다. 
```c
int handle_print_node(char* args) {
    int node_index;
    // Parse arguments
    int parsed = sscanf(args, "%d", &node_index);
    if (parsed != 1) {
        print_argument_error("print-node", "<node_index>", false);
        return CMD_ERROR;
    }
    // Validate input
    if (node_index < 0 || node_index >= 256) {
        printf("Error: Node index must be between 0 and 255\n");
        return CMD_ERROR;
    }
    // Check if node exists
    if (!Core[node_index]) {
        printf("Error: Node %d does not exist\n", node_index);
        return CMD_ERROR;
    }
    // Get node size
    ushort node_size = 1 << (*(ushort*)Core[node_index]);
    // Print node information header
    printf("\nNode %d Information:\n", node_index);
    printf("Size: %d bytes\n", node_size);
    printf("Core Position: %d\n", CoreMap[node_index].core_position);
    printf("File Offset: 0x%08lX\n", CoreMap[node_index].file_offset);
    printf("Load Status: %s\n", CoreMap[node_index].is_loaded ? "Loaded" : "Not loaded");
    // Print node data in hexadecimal format
    printf("\nMemory Contents:\n");
    printf("Offset    00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F    ASCII\n");
    printf("--------  -----------------------------------------------    ----------------\n");
    for (int i = 0; i < node_size; i += 16) {
        // Print offset
        printf("%08X  ", i);
        // Print hex values
        for (int j = 0; j < 16; j++) {
            if (i + j < node_size) {
                printf("%02X ", Core[node_index][i + j]);
            } else {
                printf("   ");
            }
        }
        // Print ASCII representation
        printf("   ");
        for (int j = 0; j < 16 && i + j < node_size; j++) {
            char c = Core[node_index][i + j];
            printf("%c", (c >= 32 && c <= 126) ? c : '.');
        }
        printf("\n");
    }
    return CMD_SUCCESS;
}
```
# handle_print_free_space
- free space 정보를 출력해 주는 함수
```c
int handle_print_free_space(char* args) {
    if (args) {
        print_argument_error("print-free-space", "", false);
        return CMD_ERROR;
    }
    if (!free_space) {
        printf("Error: Free space manager not initialized\n");
        return CMD_ERROR;
    }
    printf("\nFree Space Information:\n");
    printf("Total free blocks: %d\n", free_space->count);
    printf("Free node indices: %d\n", free_space->index_count);
    if (free_space->count > 0) {
        printf("\nFree Blocks:\n");
        printf("Size (bytes)    Offset\n");
        printf("------------    ------\n");
        for (uint i = 0; i < free_space->count; i++) {
            printf("%-14u    0x%08lX\n",
                   free_space->blocks[i].size,
                   free_space->blocks[i].offset);
        }
    }
    if (free_space->index_count > 0) {
        printf("\nFree Node Indices:\n");
        for (uint i = 0; i < free_space->index_count; i++) {
            printf("%d ", free_space->free_indices[i]);
        }
        printf("\n");
    }
    return CMD_SUCCESS;
}
```