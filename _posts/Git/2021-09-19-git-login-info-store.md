---
title:  "Git push시 username, password 입력 자동화"
excerpt: ""

categories:
  - Git
tags:
  - [자동 로그인, Gitconfig, Git 설정]

toc: false
toc_sticky: true
use_math: false
 
date: 2021-09-19
last_modified_at: 2021-09-19
sitemap :
  changefreq : daily
  priority : 1.0
---

 Local repository에서 원격 repository로 push 할 경우 username과 password를 요구한다. 

 > Username for 'https://github.com':       
 > Password for 'https://hoo223@github.com':

 이때 비밀번호는 token을 요구한다 (ID/Personal Access Token 방식의 Token Authentication).
 * [Token 생성 방법](https://velog.io/@ruddms936/github-%ED%86%A0%ED%81%B0-%EC%83%9D%EC%84%B1)
 
 매번 적기 귀찮을 경우 아래 명렁어를 설정하면 인증을 캐시에 저장하여 처음 한 번만 인증하면 된다.

 > git config credential.helper store 

 만료 기간을 설정하고 싶을 때는 

 > git config --global credential.helper 'cache --timeout 3600]'      

 시간은 초(s) 단위로 입력 가능하다. 위 코드는 3600초 (= 1시간) 을 설정한 예시이다.

---

# Reference

https://git-scm.com/docs/git-credential-cache      

[Git 아이디 패스워드 입력 안하는 방법](https://webisfree.com/2017-05-19/git-%EC%95%84%EC%9D%B4%EB%94%94-%ED%8C%A8%EC%8A%A4%EC%9B%8C%EB%93%9C-%EC%9E%85%EB%A0%A5-%EC%95%88%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)        

 [github 토큰 생성](https://velog.io/@ruddms936/github-%ED%86%A0%ED%81%B0-%EC%83%9D%EC%84%B1)