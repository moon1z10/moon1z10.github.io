---
title:  "AWS Severless 서버리스 Webapp 개발 따라하기3"
excerpt: "AWS Severless 서버리스 Webapp 개발 따라하기 시리즈 포스트 입니다."

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - AWS
last_modified_at: 2020-03-04T15:22:00-00:00
---

# AWS가이드와 함께하는 Severless 서버리스 Webapp 개발 따라하기3
------------
## 모듈 3. 서버리스 서비스 백엔드

> "AWS Lambda와 Amazon DynamoDB를 사용하여 웹 애플리케이션에 대한 요청을 처리하기 위한 백엔드 프로세스를 만듭니다. 첫 번째 모듈에서 배포한 브라우저 애플리케이션을 사용하여 사용자는 선택한 위치로 unicorn을 전송하도록 요청할 수 있습니다. 이러한 요청을 이행하려면 브라우저에서 실행 중인 JavaScript가 클라우드에서 실행하는 서비스를 호출해야 합니다."

Dynamo DB는 말 그대로, 동적 저장 영역인 데이터 베이스
Lambda는 DB에 접근해서 CRUD 등의 역할을 하는 서비스이다.

쉽게 설명하면, 웹사이트의 경우 우리가 웹브라우저를 통해 보여지는 영역인 Front-End가 있고, 조회나 회원 가입 등을 처리시에 서버 쪽에서 동작하는 코드 영역인 Back-End가 있다.
- Front-End는 Amazon S3에 작성한 내용을 업로드하면 끝나고,(Proxy 서버나 웹서버라고 생각하면 편하다.)
- Back-End에서는 보통 서비스를 통해 조회, DB 쓰기 기능을 하는데, 이러한 동작 하나하나를 작게 쪼개서 Lambda 서비스 내에서 구현하면 된다는 소리다. 

여기서 중요한 거는 결국 CRUD가 일어나는 행동의 구현은 해야 하지만, 개발자가 서버 설치하고, 세팅하고 하는 행동을 할 필요가 없다는 점이다. (생각보다 많은 시간이 여기서 할애된다.)

물론, Lambda서비스도 세팅이 필요하지만, 서버 S/W을 H/W에 설치하고 세팅하는 일은 생각보다 복잡하며, 이슈가 많이 발생하고, 비용이 추가될 수 있다.

그리고 Lambda 서비스는 AWS에서 동작하는 서비스이므로, AWS 서비스 장애가 있지 않는 한, Lambda 서비스는 정상적으로 동작할 것이다. 이 말은 Trouble shooting이 기존 서버 관리에 비하여 더 자유롭고 쉽다는 말이다.
특정 리전에 문제가 생겨도, 다른 리전으로 옮겨버리면 버리면 그만이기 때문이다.

아직 나도 처음 따라하는 단계이므로, 이러한 Lambda가 얼마나 많은 트래픽을 커버할 수 있는지는 모르겠다. Micro-service로 아무리 쪼갠다 한들, 너무너무 복잡한 서비스는 존재할 수 있고 (이미 설계를 바꿀 수 없는 수준에 이르른 단계의 녀석들...), 그러한 녀석들의 서비스가 지연되면 Lambda가 얼마나 버텨줄지 궁금하기도 하다.
>> 물론 Lambda로 전환시에 최적화를 해야겠지만..

그리고 서비스들의 모니터링을 얼마나 쉽게 할 수 있는지도 궁금한데, 이게 무료 티어에서 가능한지도 차근차근 알아봐야할 것 같다.

> "사용자가 unicorn을 요청할 때마다 호출될 Lambda 함수를 구현하겠습니다. 이 함수는 플릿에서 unicorn을 선택하고, DynamoDB 테이블에 요청을 기록한 후 디스패치된 unicorn에 대한 세부 정보를 프런트 엔드 애플리케이션에 반환합니다."

여기까지 읽으면 unicorn이 뭐야?라고 생각할 수 있다. 즉, 가이드가 너무 불친절하다는 말이다. 여기서의 unicorn=유니콘 이다. 정말 말 그대로 유니콘 이미지가 화면 상에 움직이므로 너무 복잡하게 생각하지 말자.

아무튼 일단 구현 지침을 따라가보자.


### 1단계. Amazon DynamoDB 테이블 만들기

![](/assets/images/posts/Learn-AWS-Severless-Webapp/25.png)
- 콘솔에서 서비스를 선택후, DynamoDB를 선택
- 테이블 생성
> "테이블 이름과 파티션 키는 대/소문자를 구분합니다. 제공된 ID를 정확하게 사용해야 합니다. 다른 모든 설정에는 기본값을 사용합니다."

![](/assets/images/posts/Learn-AWS-Severless-Webapp/26.png)
- 개요 탭의 제일 하단에 ARN을 메모하자.

### 2단계. Lambda 함수를 위한 IAM 역할 만들기

> "각 Lambda 함수에는 IAM 역할이 연결되어 있습니다. 이 역할은 기능이 상호 작용하도록 허용된 다른 AWS 서비스를 정의합니다. 이 연습의 목적상 Amazon CloudWatch Logs에 로그를 쓸 수 있는 권한과 DynamoDB 테이블에 항목을 쓸 수 있는 액세스 권한을 Lambda 함수에 부여하는 IAM 역할을 만들어야 합니다."

아쉽게도 IAM이 뭔지, CloudWatch Logs는 어떠한 역할을 하는지에 대한 설명이 없다. 페이지 링크 정도 추가해주면 좋을 것 같다.

- 콘솔 서비스에서 IAM을 검색, 선택
- 좌측 역할 메뉴를 선택
- '역할 만들기' 클릭
![](/assets/images/posts/Learn-AWS-Severless-Webapp/27.png)
- AWS 서비스 그룹에서 역할 유형으로 Lambda를 선택한 후 다음: 권한을 클릭
- 필터 텍스트 상자에 `AWSLambdaBasicExecutionRole` 을 입력하고 체크
![](/assets/images/posts/Learn-AWS-Severless-Webapp/28.png)
- 태그 추가(선택 사항) : 선택사항이므로 다음: 검토 클릭
- 역할 이름을 알맞게 입력 (`WildRydesLambda`)
- 역할 만들기 클릭
- 생성한 역할 이름 클릭
- 권한 탭에서 인라인 정책 추가 선택
- 서비스 : DynamoDB, 작업 : PutItem, 리소스 : 특정
- 리소스 table에 ARN 추가 선택, DynamoDB_table ARN 지정에 이전에 기록해둔 테이블 ARN값을 붙여넣고, 추가
![](/assets/images/posts/Learn-AWS-Severless-Webapp/29.png)
- 정책 검토 클릭
- 정책 이름에 `DynamoDBWriteAccess` 를 입력하고 정책 생성

### 3단계. 요청을 처리하기 위한 Lambda 함수 만들기

- Lambda 서비스로 이동
- 함수 생성
- 새로 작성 선택, 함수 이름 : `RequestUnicorn`, Runtime : Node.js 12.x
(아니 무슨 Node.js 버전은 한도 끝도 없이 올라가...미쳤음 진짜..)
- 실행 역할 : IAM에서 생성한 역할 선택 (`WildRydesLambda`)
- 함수 생성
![](/assets/images/posts/Learn-AWS-Severless-Webapp/30.png)
- 함수 코드에 `index.js` 의 내용을 [requestUnicorn.js](https://github.com/aws-samples/aws-serverless-workshops/blob/master/WebApplication/3_ServerlessBackend/requestUnicorn.js)로 바꾸고 저장.
참고로 `index.js` 파일 내용을 잘 보면, DB에 값을 저장하는 동작을 하고 있으니 한번 리뷰하는 것도 나쁘지 않아 보인다.

### 4단계. 구현 테스트

- 제일 윗부분에 "Select a test event" - 테스트 이벤트 생성 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/31.png)
- 이벤트 이름 : `TestRequestEvent`

```json
{
    "path": "/ride",
    "httpMethod": "POST",
    "headers": {
        "Accept": "*/*",
        "Authorization": "eyJraWQiOiJLTzRVMWZs",
        "content-type": "application/json; charset=UTF-8"
    },
    "queryStringParameters": null,
    "pathParameters": null,
    "requestContext": {
        "authorizer": {
            "claims": {
                "cognito:username": "the_username"
            }
        }
    },
    "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
}
```

- 생성
- 테스트 버튼 클릭
- 실행 결과 로그 세부정보가 아래와 같은지 확인
```json
{
    "statusCode": 201,
    "body": "{\"RideId\":\"SvLnijIAtg6inAFUBRT+Fg==\",\"Unicorn\":{\"Name\":\"Rocinante\",\"Color\":\"Yellow\",\"Gender\":\"Female\"},\"Eta\":\"30 seconds\"}",
    "headers": {
        "Access-Control-Allow-Origin": "*"
    }
}
```

거의 다 왔다. 이제 작성된 Lambda를 호출하는 API를 구현하고 연결시켜주면 된다.

------------

이전 글 : [AWS Severless 서버리스 Webapp 개발 따라하기 2](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-2/)
다음 글 : [AWS Severless 서버리스 Webapp 개발 따라하기 4](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-4/)

------------