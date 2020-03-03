---
title:  "카테고리 등록해보기"
excerpt: "특정 카테고리 1개만 보이는 페이지 및 메뉴 생성하기"

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Blog
tags:
  - GitHub
last_modified_at: 2020-03-03T17:00:00-00:00
---

# 1. Category 등록

_pages 폴더에 페이지를 하나 추가해준다.

```yml
---
title: "알고리즘"
permalink: /categories/Algorithm/
layout: category
author_profile: true
taxonomy: 알고리즘
---
```

- 알고리즘 카테고리 1개를 만들었다.
- permalink는 categories 하위의 카테고리 이름이다.
- layout은 category이다.

# 2. 해당 카테고리 페이지를 볼 수 있도록 메뉴에 추가

메뉴에 카테고리를 추가해준다.

```yml
# main links
main:
  - title: "Algorithm"
    url: /categories/Algorithm/
    ...
```

메뉴의 해당 링크를 클릭하면 알고리즘 페이지로 연결된다.

[카테고리 등록하는 방법](https://github.com/moon1z10/moon1z10.github.io/commit/248f68424bb669e7be1f6942892e63f2e7229f05) 링크에서 비교해서 확인해보자.