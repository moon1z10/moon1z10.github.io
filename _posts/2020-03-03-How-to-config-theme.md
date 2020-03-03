---
title:  "GitHub Pages 테마 환경설정"
excerpt: "테마 적용시 기본 환경 설정에 대하여 공유합니다."

categories:
  - Blog
tags:
  - GitHub
last_modified_at: 2020-03-03T14:45:00-00:00
---

## 주요 세팅

주요 세팅은 [_config.yml](https://github.com/moon1z10/moon1z10.github.io/blob/master/_config.yml)로 대체합니다.

```yml
# Site Settings
locale                   : "en-US"
title                    : "여기에 타이틀을 작성하세요."
title_separator          : "-"
subtitle                 : # site tagline that appears below site title in masthead
name                     : "이름을 넣으세요"
description              : "설명을 넣으세요"
url                      : # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : # the subpath of your site, e.g. "/blog"
repository               : # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
teaser                   : # path of fallback teaser image, e.g. "/assets/images/500x300.png"
logo                     : "로고 이미지를 넣고 주소를 예시와 같이 넣으세요." # path of logo image to display in the masthead, e.g. "/assets/images/88x88.png"
```

```yml
# Site Author
author:
  name             : "작성자의 이름을 넣으세요."
  avatar           : # path of avatar image, e.g. "/assets/images/bio-photo.jpg"
  bio              : ""
  location         : "위치를 넣으세요."
  email            :
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      url: mailto:이메일 주소를 넣으세요
    - label: "Website"
      icon: "fas fa-fw fa-link"
      # url: "https://your-website.com"
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url: "https://twitter.com/"
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      url: "https://facebook.com/페이스북주소를 넣으세요."
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/github주소를 넣으세요."
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      # url: "https://instagram.com/"
```