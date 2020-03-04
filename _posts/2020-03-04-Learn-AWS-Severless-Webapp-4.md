---
title:  "AWS Severless 서버리스 Webapp 개발 따라하기4"
excerpt: "AWS Severless 서버리스 Webapp 개발 따라하기 시리즈 포스트 입니다."

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - AWS
last_modified_at: 2020-03-04T15:35:00-00:00
---

# AWS가이드와 함께하는 Severless 서버리스 Webapp 개발 따라하기4
------------
## 모듈 4. RESTful API 배포

> "Amazon API Gateway를 사용하여 이전 모듈에서 구축한 Lambda 함수를 RESTful API로 공개합니다. 이 API는 퍼블릭 인터넷에서 액세스할 수 있습니다. 이 API는 이전 모듈에서 생성한 Amazon Cognito 사용자 풀을 사용하여 보호됩니다. 이 구성을 사용하고 공개된 API에 AJAX 호출을 수행하는 클라이언트 측 JavaScript를 추가하여 정적으로 호스팅된 웹 사이트를 동적 웹 애플리케이션으로 전환합니다​.
첫 번째 모듈에서 배포한 정적 웹 사이트에는 이 모듈에서 구축할 API와 상호 작용하도록 구성된 페이지가 이미 있습니다. /ride.html의 페이지에는 유니콘 탑승을 요청하는 간단한 지도 기반 인터페이스가 있습니다. /signin.html 페이지를 사용하여 인증한 후 사용자는 지도의 한 지점을 클릭하여 픽업 위치를 선택한 다음 오른쪽 상단 모서리에 있는 "Request Unicorn(유니콘 요청)" 버튼을 선택하여 탑승을 요청할 수 있습니다.
이 모듈에서는 API의 클라우드 구성 요소를 구축하는 데 필요한 단계에 초점을 집중하지만, 이 API를 호출하는 브라우저 코드의 작동 방식에 관심이 있는 경우 웹 사이트의 ride.js 파일을 살펴볼 수 있습니다. 이 경우 애플리케이션은 jQuery의 ajax() 메서드를 사용하여 원격 요청을 수행합니다."

### 1단계. 새 REST API 생성

- 콘솔 - 서비스에서 API Gateway 선택
- 시작을 누르면 자동으로 API > 생성으로 넘어간다.
- API 이름, 엔드포인트 유형(최적화된 에지) 선택
> 참고: 인터넷에서 퍼블릭 서비스에 액세스하려면 Edge optimized(최적화된 엣지)가 가장 좋습니다. 일반적으로 리전 엔드포인트는 같은 AWS 리전 내에서 주로 액세스하는 API에 사용됩니다.
- API 생성

### 2단계. Cognito 사용자 풀 권한 부여자 생성

> "Amazon API Gateway는 Cognito 사용자 풀에서 반환된 JWT 토큰을 사용하여 API 호출을 인증합니다."

- Authorizers(권한 부여자)를 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/32.png)
- 새로운 권한 부여자 생성
- 아래와 같이 입력 및 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/33.png)
- 생성
- 기존 웹 사이트 도메인에서 `/ride.html` 로 이동
- Auth 토큰을 복사
- 테스트 클릭
- Auth 토큰 입력후, 테스트 버튼 클릭
- 응답 코드 200 및 클레임 확인
![](/assets/images/posts/Learn-AWS-Severless-Webapp/34.png)

### 3단계. 새 리소스 및 방법 생성

- 좌측 메뉴 '리소스' 선택
- 작업 버튼에서 '리소스 생성' 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/35.png)
- 리소스 이름에 ride 입력, 리소스 경로에 자동으로 ride가 입력된다.
- ​Enable API Gateway CORS(API Gateway CORS 활성화)를 선택
- 리소스 생성
- `/ride` 가 선택된 상태에서 작업 버튼 클릭
- 메서드 생성
![](/assets/images/posts/Learn-AWS-Severless-Webapp/36.png)
- POST 선택 및 확인표시 체크
- 통합 유형에 Lambda 함수을 선택
- Lambda 프록시 통합 사용 선택
- 리전 선택
- 이전에 생성한 Lambda 함수 이름 입력
- 저장
- 확인
![](/assets/images/posts/Learn-AWS-Severless-Webapp/37.png)
- 메서드 요청 카드 선택
- 승인 옆의 연필 아이콘 선택
- 드롭다운 목록에서 `WildRydes Cognito` 사용자 풀 선택, 확인 표시
![](/assets/images/posts/Learn-AWS-Severless-Webapp/38.png)

### 4단계. API 배포

- 작업 버튼 선택
- API 배포 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/39.png)
- [새 스테이지] 선택
- 단계 이름 : `prod` 입력
- 배포 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/40.png)
- 호출 URL 기록해 두자.


### 5단계. 웹 사이트 구성 업데이트

> "방금 생성한 단계의 호출 URL을 포함하도록 웹 사이트 배포에서 /js/config.js 파일을 업데이트합니다. Amazon API Gateway 콘솔에 있는 단계 편집기 페이지의 상단에서 호출 URL을 직접 복사하여 사이트 /js/config.js 파일의 _config.api.invokeUrl 키에 붙여 넣어야 합니다."

- `config.js` 파일을 수정하여 업로드 한다.
- invokeUrl 에 기록해둔 호출 URL을 넣는다.
- `config.js` 업로드

### 6단계. 구현 검증

- /ride.html 로 이동해서 지도 아무데나 클릭
- 우측 상단의 Request Unicorn 을 선택하면 유니콘 아이콘이 클릭 위치로 날아간다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/41.png)

- DynamoDB에 가서 생성한 테이블의 항목 탭으로 이동
- 지도를 클릭하여 유니콘을 요청할 때마다 데이터베이스에 정보가 쌓이는 것을 확인할 수 있다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/42.png)

## 모듈 5. 리소스 삭제

- 현재 상태로 놔두면 과금이 될 수도 있다. 샘플 테스트 프로젝트 이므로, 가이드를 따라서 모두 삭제하자.

------------

샘플 프로젝트에 구현되어 있는 인증 관련 javascript 라든가, Lambda의 구현 방식 (여기서는 Node.js 이용)을 다시 한번 리뷰하고 어떻게 적용해야 되는지 시간있을 때 한번씩 보는 것을 추천 드린다.

------------

이전 글 : [AWS Severless 서버리스 Webapp 개발 따라하기 3](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-3/)

------------