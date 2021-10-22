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


### Additional 
그런데 git pull을 했을 때 로컬에서 하던 작업이 사라지는 현상을 겪었던거 같은데 정확히 어떤 상황에서 발생하는지 알 수 없었다. 같은 상황을 다시 겪게 된다면 추가적인 해결책을 작성해봐야 겠다. 