---
title:  "Github Pages에 스크린샷 이미지 쉽게 붙여넣기"
excerpt: "VSCode를 이용한 스크린샷 이미지 쉽게 붙여넣는 방법 설명"

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - Github
  - VSCode
  - Paste Image
  - Extensions
last_modified_at: 2024-04-01T08:56:00-00:00
---
------------

# Github Pages에 스크린샷 이미지 쉽게 붙여넣기

Github 블로그(Pages)에 글을 포스팅할 때, 스크린샷 이미지를 쉽게 붙여넣는 방법 소개 입니다.

처음 이용할 때(2020년)는 잘 썼었는데, 4년 만에 다시 하려니 설정을 까먹어서 남겨 놓는다.

## 1. VSCode 다운로드 및 Git clone

```bash
$> git init
$> git config --global --add safe.directory D:/Github_Pages
$> git remote add origin https://github.com/moon1z10/moon1z10.github.io.git
$> git pull origin master
```

## 2. VScode 확장 프로그램(Extensions)에서 Paste Image 설치

설치 후, 설정하는 방법이 있다. Workspace의 Paste Image 설정을 변경하기

```json
{
    "pasteImage.path": "${projectRoot}/assets/images/posts/${currentFileNameWithoutExt}",
    "pasteImage.defaultName": "YMMDD-HHmmss",
    "pasteImage.insertPattern": "${imageSyntaxPrefix}/assets/images/posts/${currentFileNameWithoutExt}/${imageFileName}${imageSyntaxSuffix}"
}
```


------------