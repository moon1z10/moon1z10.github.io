---
title:  "GitHub Pages 블로그에 댓글 기능 넣기"
excerpt: "댓글 기능을 추가하는 방법을 설명합니다."

categories:
  - Blog
tags:
  - GitHub
last_modified_at: 2020-03-03T14:37:00-00:00
---

### Features

- 기본적으로 GitHub Pages는 댓글 기능을 지원하지 않는다.
- disqus에 가입해서 댓글 기능을 추가하는 방법을 공유한다.

# disqus 가입하기

## 1. 회원 가입

[disqus](https://disqus.com/) 사이트에 접속한다.

![](/assets/images/posts/How-to-add-comment/1.png)
> GET STARTED 선택

## 2. 이메일 인증하기

인증 메일이 전송되므로 바로 인증하여 가입을 완료한다.

## 3. "I want to install Disqus on my site"를 선택

![](/assets/images/posts/How-to-add-comment/2.png)

GitHub 블로그에 댓글 기능을 추가할 것이므로..

## 4. 생성

![](/assets/images/posts/How-to-add-comment/3.png)

Website Name을 입력하면 하단에 "Your unique disqus URL will be:"라고 나오는 부분이 있는데, 중요하다.
추후에 다시 확인이 가능하므로 굳이 적어둘 필요는 없다.
언어은 그 많은 언어들이 지원되는데 한국어가 지원이 안된다.
정말 어이 없지만, 한국 개발자들은 영어를 잘한다는 베이스를 가지고 있나보다...

## 5. 과금 선택

굳이 개인 블로그인데 과금을?
그냥 묻지도 따지지도 말고 Basic을 선택하면 된다.
참고로 사이트가 변경되어서 스크롤을 내려야 보일 수도 있다.

![](/assets/images/posts/How-to-add-comment/4.png)

## 6. 플랫폼 선택

![](/assets/images/posts/How-to-add-comment/5.png)
> Jekyll 을 선택하자

## 7. 설정

딱히 적을 건 없다. 아래와 같이 보고 작성하자.

![](/assets/images/posts/How-to-add-comment/6.png)
> Complete Setup 으로 disqus에서 더이상 할 것은 없다.

## 8. 설정 화면으로 이동한다.

![](/assets/images/posts/How-to-add-comment/7.png)

설정 화면으로 이동하면 Shortname을 확인할 수 있다.
매우 중요하다. 복사해두자.

_config.yml 에서 comments의 provider를 disqus로, shortname에 복사해둔 주소를 붙여넣자.
293라인 쯤에 있는 comments의 값이 주석처리 되어 있는데, 주석을 제거하고 true로 바꿔 저장하고 저장소에 push하면 완료.

자세한 수정 사항은 [링크](https://github.com/moon1z10/moon1z10.github.io/commit/18df909c3683f2f22def1a1499fde1840391f6a3)를 확인하자.