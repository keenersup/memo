# 개요
+ [1.시작](#시작)
+ [2.log](#git-log)
+ [3.commit](#git-commit)
+ [4.폴더관리](#폴더관리)
+ [5.archive](#git-archive)
+ [6.rebase](#git-rebase)
+ [7.config](#git-config)
+ [8.branch](#git-branch)
+ [9.remote](#git-remote)
---
<br/>

## 시작
```bash
git add .
git commit -m "message"
git remote add origin "https://github.com/[기타주소].git"
git push origin master
# 강제 옵션
git push -f origin master
```
---
<br />

## git log
```bash
git log
// 확인 후 종료시 q

# 옵션
git log -p -3
// 커밋 구체적인 내용 위에서부터 3

git log --pretty=format:"%h -> %an, %ar : %s" --graph
// hash, author name, 시간, 제목 을 그래프 형태로 표현한다.
// 깃허브 커밋태그가 더 잘 보임.
```
---
<br />

## git commit
- commit 수정하기
```bash
git commit --amend
```
<br />

- commit 취소하기
```bash
// [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
git reset --soft HEAD^
// [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
git reset --mixed HEAD^ // 기본 옵션
git reset HEAD^ // 위와 동일
git reset HEAD~2 // 마지막 2개의 commit을 취소
// [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
git reset --hard HEAD^
```
---
<br />

## 폴더관리
- git 파일 삭제하기
```bash
git rm [file name]
```
<br />

- git 저장소에서만 삭제하고 실제 파일은 남겨두고 싶은 경우
만약 실제 물리적으로 디스크에는 파일을 남겨두고 Git 저장소에서만 파일을 삭제하고 싶은 경우에는 --cached 옵션을 사용하여 삭제합니다.
이후 마찬가지로 git commit 을 통해 삭제 내역을 커밋해 줘야 합니다.
```bash
git rm --cached [file name]
// 예: git rm -r --cached ../git_command
```
<br />

- 쓸데없는 폴더가 포함되었을 경우
.gitignore 에 제외할 폴더나 파일 추가
```bash
git rm --cached -r .
git add .
```
<br />

- 이전 내용으로 변경
```bash
# git log 로 변경지점 git hash id 확인
git reset --hard [git hash id]
```
<br />

TIP git reset 명령은 아래의 옵션과 관련해서 주의하여 사용해야 한다.
**reset 옵션**
– soft : index 보존(add한 상태, staged 상태), 워킹 디렉터리의 파일 보존. 즉 모두 보존.
– mixed : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 보존 (기본 옵션)
– hard : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 삭제. 즉 모두 취소.

<br />

TIP 만약 워킹 디렉터리를 원격 저장소의 마지막 commit 상태로 되돌리고 싶으면, 아래의 명령어를 사용한다.
- 단, 이 명령을 사용하면 원격 저장소에 있는 마지막 commit 이후의 워킹 디렉터리와 add했던 파일들이 모두 사라지므로 주의해야 한다.
```bash
$ git reset --hard HEAD
```
<br />

- orphan option 이용하여 master 브랜치 변경하기
```bash
#Check out to a temporary branch:
git checkout --orphan TEMP_BRANCH
// --orpahn 옵션을 사용하면 부모 커밋이 없는 새로운 브랜치를 만들 수 있다.
#Add all the files:
git add -A

#Commit the changes:
git commit -am "Initial commit"

#Delete the old branch:
git branch -D master

#Rename the temporary branch to master:
git branch -m master

#Finally, force update to our repository:
git push -f origin master
```
<br />

- git reset 복구
```bash
git reflog
```
<br />

```
3f6db14 HEAD@{0}: HEAD~: updating HEAD
d27924e HEAD@{1}: checkout: moving from d27924e0fe16776f0d0f1ee2933a0334a4787b4c
57e53a0 HEAD@{2}: modify : bug 수정
[...]
```

이런식으로 이전까지했던 작업들 reflog를 확인해 몇번째 HEAD로 이동할지 확인한다.
만약 HEAD@{1}로 이동할꺼라면
git reset --hard HEAD@{1}

출처: https://88240.tistory.com/284 [shaking blog]

---
<br />

## git archive
```bash
git archive --format=zip master -o master.zip
git archive --format=zip master -o ../master.zip
```
---
<br />

## git rebase
```bash
# 처음 commit 부터 수정
git rebase -i --root

# 앞쪽 세개 확인
git rebase -i HEAD~3
창 뜨면
pick => reword
저장하고 나오면 새창이 바로 뜬다
수정하면 끝.
시
# hash 값 위로 확인
git rebase -i [hash]

# 삭제
삭제하고픈 커밋에 drop.
```
---
<br />

## git config
```bash
git config --list
git config --global user.name "keenersup"
git config --global user.email "keenersup@gmail.com"
git config --global credential.helper 'cache --timeout=3600'
git config --global core.editor vim
```
---
<br />

## git branch
- branch 확인
```bash
git branch
```
<br />

- branch 만들기
```bash
git branch [branch name] 
# git branch develop
```
<br />

- checkout branch 변경
```bash
git checkout develop
```
<br />

- merge 하기
```bash
git checkout master
git merge develop
```
<br />

- branch 삭제
```bash
git branch -d develop
```
<br />

- conflict error 발생시
conflict 난 부분 수정하면 끝.
---
<br />

## git remote 
```bash
git remote

git remote show origin

git remote add [이름] "주소"

# 이름 변경
git remote rename

git remote -v // 전체 확인 

git remote rm [remote name]
```
---
<br />
