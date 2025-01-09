- 개발용으로 빌드할 때에는 test function들이 모두 포함되도록(tests.c, tests.h) 하고, 배포용으로 빌드할 때에는 test function들은 제외하고 빌드되도록 한다. 
# Object files
- object file들을 따로 저장하여, Makefile로 빌드할 때 수정되지 않은 object file들은 그대로 재사용하여 빌드 속도를 향상시킨다. 이를 위해서 object file을 저장하는 folder를 따로 생성하도록 Makefile을 수정해야 한다. build 과정에서도 object file이 변경되었는지 아닌지를 check해야 한다. 
- 