---
title:  "AWS Severless 서버리스 Webapp 개발 따라하기2"
excerpt: "AWS Severless 서버리스 Webapp 개발 따라하기 시리즈 포스트 입니다."

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - AWS
last_modified_at: 2020-03-04T15:14:00-00:00
---

# AWS가이드와 함께하는 Severless 서버리스 Webapp 개발 따라하기2
------------
## 모듈 2. 사용자 관리

이메일을 통한 회원 가입, 이메일 주소를 확인, 사이트에 로그인 등 사용자 관리 페이지 구성 부분이다.
Amazon Cognito 서비스를 이용하여 손쉽게 구현할 수 있다.

> "사용자가 등록 신청을 제출하면 Amazon Cognito에서 확인 코드가 담긴 확인 이메일을 해당 주소로 보내 줍니다. 사용자는 계정 확인을 위해 사이트로 돌아가서 자신의 이메일 주소와 이메일로 받은 확인 코드를 입력합니다. 가짜 이메일 주소로 테스트해 보려는 경우, Amazon Cognito 콘솔에서 사용자 계정을 확인할 수도 있습니다."

아니...이거는 원래 개발자가 다 개발했어야 하는 내용들인데, 그냥 서비스를 가져다 쓰면 된단다...개발자는 뭐해먹고 살어ㅠㅠ

> "사용자는 로그인할 때 사용자 이름(또는 이메일)과 암호를 입력합니다. 그러면 JavaScript 함수가 Amazon Cognito와 통신하여 SRP(Secure Remote Password) 프로토콜로 인증하고, 다시 JWT(JSON 웹 토큰)를 받습니다. 다음 모듈에서는 사용자의 자격 증명에 대한 정보가 들어 있는 이 JWT를 Amazon API Gateway로 구축한 RESTful API와 비교하여 인증합니다."

![](/assets/images/posts/Learn-AWS-Severless-Webapp/16.png)

### 1단계. Amazon Cognito 사용자 풀 생성

> "Cognito 사용자 풀을 사용하여 애플리케이션에 등록 및 로그인 기능을 추가하거나, Cognito Identity Pools를 사용하여 Facebook, Twitter, Amazon 같은 소셜 자격 증명 공급자를 통해 사용자를 인증하거나 SAML 자격 증명 솔루션 또는 자체 자격 증명 시스템을 사용할 수 있습니다. 이 모듈에서는 제시된 등록 및 로그인 페이지의 백엔드로 사용자 풀을 사용하겠습니다."

간단히 아래와 같다.

방법 1. Cognito User Pools 사용
방법 2. Cognito Identity Pools 사용 (Third party, 자체 개발한 증명 시스템 사용 가능)

여기서는 방법 1을 선택하는 방법을 소개하고 있다.

- Amazon 콘솔 상단의 "서비스"를 눌러서 `Cognito`를 입력하고 서비스로 이동한다.
- 사용자 풀 생성
- 풀 이름을 넣고, "기본값 검토" 선택
- 다른 옵선 선택않고, 페이지 하단의 "풀 생성" 버튼 클릭
- 풀 ID를 기록해두자.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/17.png)

### 2단계. 사용자 풀에 앱 추가

- 앱 클라이언트 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/18.png)
- 앱 클라이언트 추가​를 선택
- 앱 클라이언트 이름 지정
- 클라이언트 보안키 생성 옵션을 ​선택 취소 ("현재 브라우저 기반 애플리케이션에는 클라이언트 암호를 사용할 수 없습니다.")
![](/assets/images/posts/Learn-AWS-Severless-Webapp/19.png)
- 앱 클라이언트 ID를 기록해두자.

### 3단계. 웹 사이트 버킷에서 config.js 파일 업데이트

- S3 서비스로 이동
- `js/config.js` 파일을 선택하여 다운로드
- 다운로드한 `config.js` 파일을 열어서 기록해 두었던 내용을 작성한다.
- region값은 `userPoolId` 값의 '_' 앞에 값을 복사해서 붙여 넣으면 된다.
(해당 값이 region value)
![](/assets/images/posts/Learn-AWS-Severless-Webapp/20.png)
- 수정이 완료되면 버킷에 업로드

### 4단계. 구현 테스트

- 웹 사이트 도메인의 `/register.html`을 방문하거나 사이트 홈 페이지에서 Giddy Up!(가자!) 버튼을 선택
- 실제 본인 이메일로 가입해보면 인증 메일이 전송된다. (스팸으로 분류되어 있을 수도 있으므로 스팸함도 확인하자. 이와 관련해서 instructions에는 이를 방지하기 위한 내용이 추가되어 있다.)
> [Amazon Simple Email Service를 사용하도록 사용자 풀을 구성하여 자체 도메인에서 이메일을 보내는 것이 좋습니다.](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-message-customizations.html#cognito-user-pool-settings-ses-authorization-to-send-email)
- 인증코드를 입력하여 이메일 인증이 완료되면 API가 구성되지 않았다는 알림이 나타난다.(성공)
![](/assets/images/posts/Learn-AWS-Severless-Webapp/21.png)
- Cognito에 가서 사용자 새로고침을 해보면 가입이 CONFIRMED된 상태를 확인할 수 있다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/22.png)
- 이번에는 가짜 이메일로 가입해보자
![](/assets/images/posts/Learn-AWS-Severless-Webapp/23.png)
- Cognito 사용자 및 그룹에서 새로고침 버튼을 누르면 UNCONFIRMED 상태의 가짜 이메일이 인증을 기다리고 있는 것을 확인할 수 있다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/24.png)
- 가짜 이메일 주소를 누른다.
- 사용자 확인 클릭
- `/signin.html` 페이지로 이동해서 가짜 이메일과 비밀번호를 입력하면 사용자 인증이 완료되어 로그인 되는 것을 확인할 수 있다.
- 하지만 여전히 Cognito에서는 "이메일 확인됨"이 확인되지 않음으로 잘 나온다.

이미 샘플 프로젝트는 `js/cognito-auth.js` 파일을 통해 인증 구현이 완료되어 있으므로, 관련하여 궁금한 사람들은 해당 파일을 리뷰해서 수정해서 사용하면 될 듯 하다.

이것으로 모듈2의 단계가 끝났다. 다음 모듈3의 단계를 따라가보자.

------------

이전 글 : [AWS Severless 서버리스 Webapp 개발 따라하기 1](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-1/)
다음 글 : [AWS Severless 서버리스 Webapp 개발 따라하기 3](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-3/)

------------