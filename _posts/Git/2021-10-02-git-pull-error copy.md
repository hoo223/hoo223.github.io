---
title:  "Git pull시 에러 발생 해결법 - error: Your local changes to the following files would be overwritten by merge"
excerpt: ""

categories:
  - Git
tags:
  - [git pull error, git pull 오류]

toc: false
toc_sticky: true
use_math: false
 
date: 2021-10-02
last_modified_at: 2021-10-02
sitemap :
  changefreq : daily
  priority : 1.0
---

# Error description

여러 컴퓨터에서 한 코드에 대한 작업을 하다보면 각 컴퓨터에서 작업한 내용을 원격 repository에 push하고 다른 컴퓨터에서 pull하여 작업을 이어나가게 된다. 

이때 pull하기 전에 local에서 코드를 먼저 수정하고 pull을 하게되면 원격과 충돌이 일어나면서 아래와 같은 에러가 발생한다. 
~~~
error: Your local changes to the following files would be overwritten by merge:
~~~

# Solution
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