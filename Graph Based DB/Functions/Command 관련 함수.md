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
# 