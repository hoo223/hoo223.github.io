---
title:  "Github Blog 관련 Reference"
excerpt: "Github blog를 꾸밀 때 참고한 사이트들"

categories:
  - Blog
tags:
  - [jekyll, Github, Git, minimal-mistake, mathjax]

toc: false
toc_sticky: true
 
date: 2021-09-14
last_modified_at: 2021-09-17
sitemap :
  changefreq : daily
  priority : 1.0
---

## 본문 영역 / 글씨 크기 조절, 하이퍼링크 밑줄 제거
[[Github Blog] minimal mistakes - 본문 영역 및 글자 크기](https://eona1301.github.io/github_blog/GithubBlog-Content-Width/)

## 본문에 수식 표현하기 (mathjax)
[Jekyll Github 블로그에 MathJax로 수학식 표시하기](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)

<details>
<summary>Outline 표기 ('$$') 관련 오류 수정</summary>
<div markdown="1">

위 글처럼 설정 후 outline으로 수식을 표시할 경우, 아래 그림에서 빨간색으로 표시한 것과 같이 '\\\[' , '\\ \]' 표시가 생김    
![mathjax 오류](https://user-images.githubusercontent.com/17296297/133537357-c52622ef-3de4-45d9-b151-0588b21b32a1.PNG)    
이 경우 위 글에서 생성한 _includes/mathjax_support.html 안의 내용을 아래 코드로 대체하니 표시가 사라짐    
<script src="https://gist.github.com/hoo223/5b35651825c81c67ef643c28adaf1ff4.js"></script>
* 변경 내용
  * line 9 : ["\\\\(","\\\\)"] ] 추가 
  * line 10 : ["\\\\[","\\\\]"] 추가     

</div> 
</details>

## 본문에서 가독성 높은 코드 블록 작성 (Github Gist)
[[Github Blog] MarkDown 프로그래밍 코드 작성하기](https://eona1301.github.io/github_blog/GithubBlog-Code/)

## 본문에서 접기/펼치기 기능 사용 
[[Markdown] 마크다운에서 접기 펼치기 사용](https://hello-bryan.tistory.com/205)