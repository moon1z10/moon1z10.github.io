---
title:  "Table of Contents 추가 방법"
excerpt: "목차 만드는 방법을 배우자."

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Blog
tags:
  - GitHub
last_modified_at: 2020-03-03T16:00:00-00:00
---

# 1. TOC : Table of Contents
TOC는 H1~H6의 헤더 목록을 목차로 표시해주는 기능이다.
우리가 사용하는 Minimal-Mistakes 테마에서는 TOC 기능을 오른쪽 사이즈 바에 표시해줄 수 있다.

작성하는 Post나 Page들에 아래와 같이 입력해준다.

```yml
toc: true
toc_sticky: true
```

# 2. 설정
toc_sticky 설정은 TOC를 사이드바에 고정하는 역할을 한다.
페이지가 스크롤되더라도 화면에서 화면에 붙어서 스크롤시 화면에서 보이지 않는 현상을 막아준다.

```yml
toc_label: "페이지 주요 목차"
```

toc_label은 제목을 직접 설정할 수 있게 해준다.
기본은 “On this page”로 화면에 나온다.
사용하고 싶은 문구를 문자열 값으로 입력하면 된다.

[TOC 추가하는 방법 Changelist](https://github.com/moon1z10/moon1z10.github.io/commit/8ef7c42d5b7756aac85f5ff9a1ca98d725702f60) 링크에서 비교해서 확인해보자.