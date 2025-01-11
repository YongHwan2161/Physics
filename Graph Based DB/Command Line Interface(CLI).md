- 생성된 함수들을 test하기 위해서는 명령줄을 이용해서 원하는 명령을 실행할 수 있는 환경이 구현되어 있어야 한다. 이를 위해서 프로그램이 실행되면 명령어를 입력할 수 있도록 구현한다. 
- main 함수 내에서 다음과 같이 코드를 작성하면 프로그램 실행 이후 CLI를 사용할 수 있다. 입력은 enter로 구분된다. 
```c
    printf("CGDB Command Line Interface\n");
    printf("Type 'help' for available commands\n");
    char command[256];
    while (1) {
        printf("\n> ");
        if (!fgets(command, sizeof(command), stdin)) {
            break;
        }
        int result = handle_command(command);
        if (result == CMD_EXIT) {
            break;
        }
    }
```
- handle_command 함수는 다음을 참조한다. [[Command 관련 함수#handle_command|handle_command]]
# support up and down key
- up or down 방향키를 누르면 이전 명령어 또는 다음 명령어로 바로 이동할 수 있는 기능이다. 
- 이전에 입력한 명령어를 다시 입력하고 싶을 때 일일이 다시 입력하지 않아도 되어서 편리하다. 
- 이 기능을 구현하기 위해서는 입력한 명령어들을 프로그램이 기억하고 있어야 한다. 최대 몇 개의 명령어를 기억할 지 정해주어야 한다. 
- down key를 눌렀을 때 다음 명령어가 없으면 빈 칸을 띄어서 새로운 명령어를 입력할 수 있도록 해야 한다. 
- 