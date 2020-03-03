---
title:  "GitHub Pages 카테고리, 태그 설정"
excerpt: "카테고리와 태그를 설정하고 메뉴에 추가하는 방법을 설명합니다."

toc: true
toc_sticky: true

categories:
  - Blog
tags:
  - GitHub
last_modified_at: 2020-03-03T15:13:00-00:00
---

# 1. 메뉴 세팅

카테고리와, 태그를 메뉴에 먼저 추가해보자.

[navigation.yml](https://github.com/moon1z10/moon1z10.github.io/blob/master/_data/navigation.yml)로 대체합니다.

```yml
# main links
main:
  - title: "Categories"
    url: /categories/
  - title: "Tags"
    url: /tags/
```

- _config.yml의 아래 부분에 상세히 어떻게 세팅하는지 나와있다.

```yml
# Archives
#  Type
#  - GitHub Pages compatible archive pages built with Liquid ~> type: liquid (default)
#  - Jekyll Archives plugin archive pages ~> type: jekyll-archives
#  Path (examples)
#  - Archive page should exist at path when using Liquid method or you can
#    expect broken links (especially with breadcrumbs enabled)
#  - <base_path>/tags/my-awesome-tag/index.html ~> path: /tags/
#  - <base_path>/categories/my-awesome-category/index.html ~> path: /categories/
#  - <base_path>/my-awesome-category/index.html ~> path: /
category_archive:
  type: liquid
  path: /categories/
tag_archive:
  type: liquid
  path: /tags/
```

# 2. 카테고리와 태그 페이지를 작성하자.

1. "_pages" 폴더를 생성한다.
2. "category-archive.md", "tag-archive.md" 파일을 생성한다.

```yml
# category-archive.md
---
title: "카테고리"
layout: categories
permalink: /categories/
author_profile: true
---
```

```yml
# tag-archive.md
---
title: "Posts by Tag"
permalink: /tags/
layout: tags
author_profile: true
---
```