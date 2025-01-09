# Makefile

```makefile
GCC = gcc

CFLAGS = -Wall -Wextra -g

TARGET = cgdb

  

# Source directories
SRC_DIR = src
OBJ_DIR = obj
  
# Create source file list
SRCS = $(wildcard *.c $(SRC_DIR)/*.c $(SRC_DIR)/*/*.c)
# Create object file list by replacing source paths with object paths
OBJS = $(SRCS:%.c=$(OBJ_DIR)/%.o)
  
.PHONY: all clean directories
  
all: directories $(TARGET)
  
# Create necessary directories for object files
directories:
    @mkdir -p $(OBJ_DIR)
    @mkdir -p $(OBJ_DIR)/$(SRC_DIR)
    @mkdir -p $(OBJ_DIR)/$(SRC_DIR)/tests
  
$(TARGET): $(OBJS)
    $(GCC) $(OBJS) -o $(TARGET)
  
# Pattern rule for object files
$(OBJ_DIR)/%.o: %.c
    @mkdir -p $(dir $@)
    $(GCC) $(CFLAGS) -c $< -o $@
  
run: $(TARGET)
    ./$(TARGET)
  
clean:
    rm -rf $(OBJ_DIR)
    rm -f $(TARGET)
```
