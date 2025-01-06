```makefile
CC = gcc
CFLAGS = -Wall -Wextra -g
TARGET = cgdb
SRCS = CGDB.c
OBJS = $(SRCS:.c=.o)
HEADERS = CGDB.h
  
.PHONY: all clean
  
all: $(TARGET)
  
$(TARGET): $(OBJS)
    $(CC) $(OBJS) -o $(TARGET)
  
%.o: %.c $(HEADERS)
    $(CC) $(CFLAGS) -c $< -o $@
  
run: $(TARGET)
    ./$(TARGET)
  
clean:
    rm -f $(OBJS) $(TARGET)
```
