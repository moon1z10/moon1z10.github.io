---
title:  "GitHub Pages 생성, 테마 적용 따라하기"
excerpt: "GitHub을 블로그로 사용하기 위해 저장소를 생성하고, 테마를 적용하는 가장 쉬운 방법을 요약해본다."

categories:
  - Blog
tags:
  - GitHub
last_modified_at: 2020-03-03T14:37:00-00:00
---

### Features

- gem을 쓰고 싶지도, 뭔가를 막 어렵게 커맨드 입력하기도 귀찮다.
- 나만의 가장 심플한 방법으로 Github Page를 만들어서 블로그 처럼 써보자.
- 기본적으로 테마를 지원해주지만, 가장 많이 이용되는 Minimal-Mistakes Theme를 사용하기로 한다.

# 저장소 만들기 & 테마 적용하기


# 1. 테마 적용

## a. 테마 다운로드하기

[Minimal-Mistakes Github](https://github.com/mmistakes/minimal-mistakes) 저장소로 이동해서 Download Zip을 통해 최신 버전을 다운로드 받는다.

![](/assets/images/posts/How-to-create-github-pages/1.png)

> Download Zip

## b. 압축풀기

다운로드한 Zip폴더를 압축해제 한다.

# 2. 저장소 만들기

Github에서 저장소를 만들자.

![](/assets/images/posts/How-to-create-github-pages/2.png)

> New repository

![](/assets/images/posts/How-to-create-github-pages/3.png)

Repository name에는 username.github.io의 형식으로 저장소를 만들어준다.
그리고 나머지는 딱히 손댈것 없이 Create repository 녹색 버튼을 누르자.

# 3. Git 저장소에 테마 업로드하기

압축해제한 폴더의 이름을 위에서 생성한 저장소의 이름으로 바꿔준다.
(굳이 바꿔주지 않아도 되지만, 추후 유지보수 및 헷갈림 방지를 위해 바꿔주는 것을 추천한다.)

```bash
$ mv 압축해제_디렉토리명 username.github.io
$ git init
Reinitialized existing Git repository in /Users/seokhocho/Documents/Develop/moon1z10.github.io/.git/
$ git add .
$ git commit -m "first commit"
…
$ git push -u origin master
Username for 'https://github.com': username
Password for 'https://moon1z10@github.com': (비밀번호 입력)
Enumerating objects: 627, done.
Counting objects: 100% (627/627), done.
Delta compression using up to 4 threads
Compressing objects: 100% (611/611), done.
Writing objects: 100% (627/627), 15.17 MiB | 4.70 MiB/s, done.
Total 627 (delta 57), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (57/57), done.
To https://github.com/moon1z10/moon1z10.github.io.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
$ 
```

![](/assets/images/posts/How-to-create-github-pages/4.png)

> 정상적으로 push되었다면, 테마 파일들이 모두 commit 되어 적용된 모습을 확인할 수 있다.

# 4. 블로그 확인하기

[블로그 주소](https://moon1z10.github.io/) 로 접속해서 아래와 같은 페이지가 뜬다면 정상적으로 테마가 적용된 것이다.

포스트 글이 저장소에 push 되면 GitHub Pages에서 자동으로 이를 인식하여 업데이트가 있음을 알아차리고 Jekyll engine을 사용하여 html로 변환하는 작업을 자동으로 수행한다. 포스트 글이 업데이트 되는데 길게는 1~2분 정도가 소요될 수 있으므로 기다렸다가 새로고침 해보자.

![](/assets/images/posts/How-to-create-github-pages/5.png)

> 정상적으로 블로그가 접속되는지 확인하기

# 5. 블로그 확인하기

불필요한 파일들은 삭제하자.

![](/assets/images/posts/How-to-create-github-pages/6.png)