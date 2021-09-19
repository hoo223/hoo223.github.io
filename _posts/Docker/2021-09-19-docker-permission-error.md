---
title:  "[Docker 명령어 실행 시 config.json 파일 관련 permission denied 오류]"
excerpt: ""

categories:
  - Docker
tags:
  - [Permission, Error]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-19
last_modified_at: 2021-09-19
---

# Error Description 
docker 명령어 실행시 아래와 같은 오류가 발생할 경우 

> Error loading config file: /home/$USER/.docker/config.json: open /home/$USER/.docker/config.json: permission denied

# Solution
~/.docker 폴더에 있는 파일들의 소유주를 $USER로 변경

> sudo chown "$USER":"$USER" /home/"$USER"/.docker -R

---

## Reference
[sudo 안 붙이고 docker 사용하기](https://medium.com/@yms9654/sudo-%EC%95%88-%EB%B6%99%EC%9D%B4%EA%B3%A0-docker-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-797a2821b9db)   
[[UNIX/Linux]권한 관리(chmod, chown, chgrp, umask)](https://eunguru.tistory.com/93)