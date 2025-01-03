# git init
새로운 저장소 초기화하기
```shell
mkdir /path/newDir
cd /path/newDir
git init
```
# git config
전역 사용자명/이메일 구성하기
`git config --global user.name "Your name"`
`git config --global user.email "Your email address"`
저장소별 사용자명/이메일 구성하기(해당 저장소 디렉터리로 이동 후)
`git config user.name "Your name"`
`git config user.email "Your email address"`
전역 설정 정보 조회
`git config --global --list`
저장소별 설정 정보 조회
`git config --list`
출력 예시:
```shell
~/.../Documents/Yonghwan $ git config --list
safe.directory=/storage/emulated/0/Documents/Yonghwan
user.email=ant713800@gmail.com
user.name=YongHwan2161
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/YongHwan2161/Physics.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
```

# git clone
저장소 복제하기
`git clone <저장소 url>`

# git remote
새로운 원격 저장소 추가하기(??)
`git remote add <원격 저장소><저장소 url>`
> 예시: `git remote add origin master`

# git add
- `git add <파일/디렉토리 경로>`
- 현재 폴더의 모든 파일들을 추가하려면 `git add .`
- `git add -A`: 프로젝트 내의 모든 변경 내용을 몽땅 스테이징 영역으로 넘기고 싶을 때 사용. 최상위 디렉토리에서는 `git add .`가 같은 동작을 함
- `git add -p`
- `git add` 명령어는 다음 변경(commit)을 기록할 때까지 변경분을 모아놓기 위해서 사용한다. ㄸ라서 `git commit`을 통해 명시적으로 기록을 남기기 전까지는 아무리 `git add` 명령어를 많이 실행해도 git 저장소의 변경 이력에는 어떤 영향도 주지 않는다.
- `git add`명령어를 사용하면 현재 작업 디렉토리에 있는 모든 또는 일부 변경 내용을 스테이징 영역으로 이동시킬 수 있다.
# staging area
- staging area는 작업 디렉토리와 git 저장소의 변경 이력 사이에 징검다리 역할을 한다. 작업 디렉토리는 아직 커밋할 준비가 안된 변경 내용을 자유롭게 수정할 수 있는 공간인 반면에, staging area는 commit할 준비가 된 변경 내용이 git 저장소에 기록되기 전에 대기하는 장소라고 볼 수 있다. 
# git status
- `git status`는 작업 디렉터리와 스테이징 영역의 상태를 확인하기 위해서 사용한다.  
- `changes to be committed`: 이 영역은 스테이징 영역에 넘어가 있는 변경 내용을 보여준다.
- `changes not staged for commit`: 이 영역은 아직 워킹 디렉토리에 있는 변경 내용을 보여준다.
- `untacked files`: 아직 워킹 디렉토리에 있는 아직 한 번도 해당 git 저장소가 관리한 적이 없는 새로운 파일을 보여준다. 
# git rm
## 추적 상태 제거
git에 올리고 싶지 않은 파일/폴더들은 `.gitignore`에 넣어서 git tracking을 방지할 수 있다. 하지만 이미 git으로 관리중이던 파일의 경우에는 `.gitignore`에 추가해도 git이 계속 추적을 하기 때문에 commit할 때 계속 변경사항으로 뜬다. 이를 방지하기 위해서는 `rm` 명령어를 사용해서 git에서 제거해 줘야 한다.  `--cached` 옵션은 파일을 삭제하지는 않고 추적만 안하도록 하기 위한 것이다.
```bash
git rm -r --cached whatever_you_want/
```
# git commit
- `git commit`: vi editor를 실행시킴 
- `git commit -m "commit messaage"`
- 다음과 같은 에러 발생 시:
```bash
.../Documents/Yonghwan $ git commit -m "test"
error: object file .git/objects/00/8e7879358f62667bf67d94645bac24ace0072d is empty
fatal: unable to read tree (008e7879358f62667bf67d94645bac24ace0072d)
[master .../Documents/Yonghwan $
```
