---
title:  "Open AI gym 새로운 환경 만들기/등록하기"
excerpt: ""

categories:
  - Blog
tags:
  - [gym, RL, Environment]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-23
last_modified_at: 2021-09-23
sitemap :
  changefreq : daily
  priority : 1.0
---

Gazebo 환경에서 강화학습을 돌리기 위해 gym 환경으로 인터페이싱(interfacing) 하는 과정이 필요했다. 

gym에서 제공해준 환경 템플릿에 맞게 환경을 만들어 파이썬 파일(.py)로 저장 후 import하여 사용하는 것도 가능하지만, 다른 기존 환경들처럼 gym.make 함수를 통해 환경을 불러오고 싶다면 gym에 환경을 등록해 주어야 한다. 

gym github에 있는 docs 중 [creating-environment.md](https://github.com/openai/gym/blob/master/docs/creating-environments.md)를 참고하면 새로운 gym 환경을 만들고 등록할 수 있다. 

## 예시
'gym-foo'라는 새로운 환경을 만들고 등록하고자 한다면 gym-foo 폴더를 만들고 다음과 같이 구성한다.
```
gym-foo/    
  README.md   
  setup.py    
  gym_foo/    
    __init__.py   
    envs/   
      __init__.py   
      foo_env.py    
      foo_extrahard_env.py    
```
각 파일들은 다음과 같이 구성한다.

#### gym-foo/setup.py 
<script src="https://gist.github.com/hoo223/fe18a74f088daebdffdf753850183965.js"></script>

#### gym-foo/gym_foo/__init__.py
<script src="https://gist.github.com/hoo223/fa9bd8aebd838a1102c5e7500845dcea.js"></script>

#### gym-foo/gym_foo/envs/__init__.py
<script src="https://gist.github.com/hoo223/3332d7d60cc12b190fc665c72518d3b8.js"></script>

#### gym-foo/gym_foo/envs/foo_env.py
<script src="https://gist.github.com/hoo223/9c42d98d47d01ac40ca3d383ee9d8e23.js"></script>

위와 같이 구성을 완료하였으면 pip install을 통해 환경을 설치한다. 
> pip install -e gym-foo

python을 실행시켜 환경이 제대로 등록되었는지 확인한다.
> import gym    
> gym.make('gym_foo:foo-v0')