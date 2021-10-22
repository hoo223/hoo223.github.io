---
title:  "Git push시 에러 발생 해결법 - error: failed to push some refs to ~"
excerpt: ""

categories:
  - Git
tags:
  - [git push error, git push 오류]

toc: false
toc_sticky: true
use_math: false
 
date: 2021-10-22
last_modified_at: 2021-10-22
sitemap :
  changefreq : daily
  priority : 1.0
---

# Error description

로컬 repository에서 작업 후 원격 저장소로 push 할 때 다음과 같은 에러가 발생할 수 있다. 

~~~
$ git push
To https://github.com/user_name/user_name.github.io.git
 ! [rejected]          master -> master (fetch first)
error: failed to push some refs to 'https://github.com/hoo223/hoo223.github.io.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
~~~

내용을 보면, 로컬에는 없는 어떤 작업이 원격 저장소에 push 되어 서로 일치하지 않아서 update가 거부되었다고 한다. 

# Solution
이를 해결하기 위해서는 git pull을 통해 로컬을 원격과 같도록 업데이트 해준 후 push를 하면 된다. 

그래서 git pull을 했 로컬에서 작업하던 코드들이 날아가는 경험을  

## git stash
git stash 명령어를 이용하면 local의 변경 사항을 스택에 임시로 저장해 놓고 working directory를 초기화 할 수 있다. 

이 상태에서 git pull을 실행하면 에러 없이 정상적으로 동작하게 된다. 

## git stash list      
저장된 stash 목록은 다음과 같이 확인 가능하다.

ex)
~~~
$ git stash list      
stash@{0}: WIP on master: ffecd108 post update
~~~

## git stash apply
저장된 작업을 다시 불러와 적용하고 싶다면
~~~
$ git stash apply -> 가장 최근의 stash를 불러옴
~~~
or      
~~~
$ git stash apply <stash 이름>
~~~

## git stash drop
apply를 하더라도 스택에는 stash가 남아있다. stash를 스택에서 제거하고 싶다면
> git stash drop -> 가장 최근의 stash를 제거

or

~~~
git stash drop <stash 이름>
~~~

ex)
~~~
$ git stash drop      
Dropped refs/stash@{0} (6d55cddbb418bd9c7fe23b95e2b7768440a582f2)
~~~

## git stash pop
apply와 drop을 동시에 적용한다.

# 활용 방안

이 명령어는 local에서 진행하던 작업을 잠시 멈추고 다른 작업을 위해 branch를 변경해야 하는데 commit은 하기 싫은 경우 사용할 수 있다. 


# Reference
[[Git (6)] git pull 에러 해결방법 (Your local changes to the following files would be overwritten by merge)](https://goddaehee.tistory.com/253)

[[Git] git stash 명령어 사용하기](https://gmlwjd9405.github.io/2018/05/18/git-stash.html)