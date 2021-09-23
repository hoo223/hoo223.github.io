---
title:  "Github Blog 만들기"
excerpt: "Github Blog를 만들고 포스팅 해보기 "

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]

toc: true
toc_sticky: true
 
date: 2021-09-14
last_modified_at: 2021-09-19
sitemap :
  changefreq : daily
  priority : 1.0
---

## 1. Github Blog 생성
### Reference

* 블로그 생성 : [왕초보를 위한 Github 블로그 만들기 (1)](https://zeddios.tistory.com/1222)      
* Ruby 설치 (Windows) : [RubyInstaller](https://rubyinstaller.org/)
    * 3.0 버전 설치 시 오류 발생 -> 2.7 버전으로 설치
* [Ubuntu에 Jekyll 설치하기](https://velog.io/@ilcm96/install-jekyll-on-ubuntu)

***

## 2. Posting 해보기
### Reference 
* [[Github 블로그] 블로그 포스팅하는 방법](https://ansohxxn.github.io/blog/posting/)

***

## 3. 테마 적용
사용 테마 : [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)

1. fork 후 repository 이름을 USERNAME.github.io로 변경
2. 불필요한 파일 및 폴더 제거
    >.editorconfig    
    .gitattributes    
    .github   
    /docs   
    /test   
    CHANGELOG.md    
    minimal-mistakes-jekyll.gemspec   
    README.md   
    screenshot-layouts.png    
    screenshot.png    

3. 필요한 폴더 생성
    > _posts  
    _pages    
    _drafts   

4. Gemfile 수정
    > source "https://rubygems.org"   
    >
    > gem "jekyll", "~> 3.5"    
    > gem "minimal-mistakes-jekyll"   
    > gem "kramdown-parser-gfm"   
    
5. 라이브러리 의존성 설치   
    At local repository
    > $ bundle install

6. 적용 확인 
    > `bundle exec jekyll serve`

7. 원격 서버에 push
    > `git add .`       
    `git commit -m "your_comment"`      
    `git push`


### Reference
* [[Github 블로그] 깃허브(Github) 블로그를 생성해 보자.](https://ansohxxn.github.io/blog/i-made-my-blog/)
* [Minimal Mistakes 를 적용한 Github Page 만들기](https://pnurep.github.io/blogging/github-page-minimal-mistakes/#)
