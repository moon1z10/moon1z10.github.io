---
title:  "AWS Severless 서버리스 Webapp 개발 따라하기"
excerpt: "AWS Severless 서버리스 Webapp 개발 따라하기 시리즈 포스트 입니다."

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - AWS
last_modified_at: 2020-03-04T15:00:00-00:00
---

# AWS가이드와 함께하는 Severless 서버리스 Webapp 개발 따라하기
------------
자습서 참조 :
https://aws.amazon.com/ko/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/ 접속

사전 준비사항 : 인증 완료 / 사용 가능한 AWS 계정, 텍스트 에디터, 크롬 최신 웹 브라우저

------------

![](/assets/images/posts/Learn-AWS-Severless-Webapp/1.png)

참조 사이트 약 2시간이 걸린다고 나와있다.
하지만 최신으로 항시 업데이트 된 내용이 아니기 때문에 몇 군대에서 시간을 소비했다. 관련해서도 모두 포스팅 할 것이므로 참고하시라.

![](/assets/images/posts/Learn-AWS-Severless-Webapp/2.png)

참고로 서버리스의 구성도는 위와 같다.
매우 간단하다.
정적 파일을 관리하는 S3, 사용자를 관리하는 Cognito, DynamoDB, Lambda, API Gateway.
물론 복사 & 붙여넣기만 하기 때문에 어려움은 전혀 없다.
단지, 추후에 Lambda를 구현하는 방법이나, javascript파일들을 이용해 AWS 서비스와 연동하는 부분은 샘플 파일들이나 공부가 더 많이 필요해보인다.

### [ 목차 ]

#### [AWS Severless 서버리스 Webapp 개발 따라하기 1](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-1/)

#### [AWS Severless 서버리스 Webapp 개발 따라하기 2](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-2/)

#### [AWS Severless 서버리스 Webapp 개발 따라하기 3](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-3/)

#### [AWS Severless 서버리스 Webapp 개발 따라하기 4](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-4/)

------------