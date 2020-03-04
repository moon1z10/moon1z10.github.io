---
title:  "AWS Severless 서버리스 Webapp 개발 따라하기1"
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

# AWS가이드와 함께하는 Severless 서버리스 Webapp 개발 따라하기1
------------
## 모듈 1. 정적 웹 호스팅
> "이 모듈에서는 웹 애플리케이션에 대해 정적 리소스를 호스팅하도록 Amazon Simple Storage Service(S3)를 구성합니다.
HTML, CSS, JavaScript, 이미지 및 기타 파일을 비롯한 모든 정적 웹 콘텐츠는 Amazon S3에 저장됩니다. 그런 다음 최종 사용자는 Amazon S3에서 표시한 퍼블릭 웹 사이트 URL을 사용하여 해당 사이트에 액세스합니다. 사이트를 사용 가능하게 만들기 위해 웹 서버를 실행하거나 다른 서비스를 실행할 필요가 없습니다.
이 모듈의 목적상 제공된 Amazon S3 웹 사이트 엔드포인트 URL을 사용할 것입니다. URL 형식은 http://{your-bucket-name}.s3-website.{region}.amazonaws.com입니다. 대부분의 실제 애플리케이션의 경우 사용자 지정 도메인을 사용하여 사이트를 호스트할 것입니다."

### 1단계. 리전 선택

> "[리전 표](https://aws.amazon.com/ko/about-aws/global-infrastructure/regional-product-services/)를 참조하여 지원되는 서비스를 포함하는 리전을 확인할 수 있습니다. 지원되는 리전 중에서 선택할 수 있습니다."

계정을 가입하고, Region을 Asia Pacific (Seoul)로 바꿔줬다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/3.png)

![](/assets/images/posts/Learn-AWS-Severless-Webapp/4.png)
그런데 S3 서비스는 리전을 선택할 필요가 없다고 나온다.

### 2단계. S3 버킷 생성

> "이 단계에서는 웹 애플리케이션에 대해 모든 정적 애셋(HTML, CSS, JavaScript, 이미지 파일 등)을 호스팅하는 데 사용할 새로운 S3 버킷을 만들겠습니다. 
이 단계에서는 콘솔이나 AWS CLI를 사용하여 Amazon S3 버킷을 만듭니다. 버킷 이름은 전역적으로 고유해야 합니다. wildrydes-firstname-lastname 등과 같은 이름을 사용하는 것이 좋습니다. 버킷 이름이 이미 존재한다는 오류가 발생하면, 사용되지 않는 이름을 찾을 때까지 숫자나 문자를 추가하십시오."

![](/assets/images/posts/Learn-AWS-Severless-Webapp/5.png)

샘플 프로젝트 이므로, 이름을 알아서 정하고, 옵션과 권한은 건드리지 않고 저장한다. 검토 단계에서 '버킷 만들기'를 선택하면 S3 버킷이 생성된다.

![](/assets/images/posts/Learn-AWS-Severless-Webapp/6.png)

### 3단계. 콘텐츠 업로드

- 콘솔 단계별 지침의 a에 있는 "이 링크"를 눌러서 zip파일을 다운로드 받는다.
(웹사이트 구성 정적 파일들)
- 압축을 푼다.
- Management Console에서 S3 서비스로 이동한 다음, 생선한 버킷 이름을 누른다.
- 시작하기 버튼 또는 업로드 버튼을 누른다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/7.png)
- aws-serverless-workshops-master\WebApplication\1_StaticWebHosting\website 폴더내에 있는 파일들을 업로드한다. (website 폴더를 통째로 올리는 것이 아니라, 파일들을 모두 선택해서 업로드 해야 한다.)
![](/assets/images/posts/Learn-AWS-Severless-Webapp/8.png)
![](/assets/images/posts/Learn-AWS-Severless-Webapp/9.png)
![](/assets/images/posts/Learn-AWS-Severless-Webapp/10.png)

### 4단계. 퍼블릭 읽기를 허용하는 버킷 정책 추가

> "버킷 정책을 사용하여 S3 버킷의 콘텐츠를 액세스할 수 있는 사용자를 정의할 수 있습니다. 버킷 정책은 버킷의 객체에 대해 다양한 작업을 실행할 수 있는 주체를 지정하는 JSON 문서입니다.
이 단계에서는 익명 사용자가 해당 사이트를 볼 수 있도록 하는 버킷 정책을 새로운 Amazon S3 버킷에 추가할 것입니다. 기본적으로 AWS 계정에 대해 액세스 권한을 가진 인증된 사용자만 버킷에 액세스할 수 있습니다.
익명 사용자에게 읽기 전용 액세스 권한을 부여할 이 정책 예제를 참조하십시오. 이 예제 정책은 인터넷 사용자가 해당 콘텐츠를 볼 수 있습니다. 버킷 정책을 업데이트하는 가장 쉬운 방법은 콘솔을 사용하는 것입니다. 버킷을 선택하고 권한 탭을 선택한 후 버킷 정책을 선택합니다."

한마디로, 버킷 생성시에 권한을 따로 설정하지 않았다면, 인증된 사용자만 버킷에 있는 애셋에 접근할 수 있다는 이야기다.
하지만, 웹페이지는 그 누구든 접속해서 이미지와 html페이지, javascript등을 요청하여 웹브라우저로 볼 수 있어야 하므로 익명 사용자에게 "읽기" 권한은 부여해야 하는 것이고, 이것은 콘솔에서 쉽게 할 수 있다.

![](/assets/images/posts/Learn-AWS-Severless-Webapp/11.png)

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow", 
            "Principal": "*", 
            "Action": "s3:GetObject", 
            "Resource": "arn:aws:s3:::[YOUR_BUCKET_NAME]/*" 
        } 
    ] 
}
```

참고로 Version의 값에 있는 "2012-10-17"은 절대로 바꾸지 말자.
만약 이 값을 바꾼다면, 아래와 같은 오류를 보게 될 거다.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/12.png)

왜 바꾸면 안되는지는 [JSON Policy Elements: Version](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_version.html) 을 참고하자.

여기서, 버킷정책이 오류와 함께 저장되지 않는다면, 퍼블릭 액세스 차단을 아래와 같이 편집하여 저장하고 다시 시도해보자.
![](/assets/images/posts/Learn-AWS-Severless-Webapp/13.png)

### 5단계. 웹 사이트 호스팅 사용

- 속성 탭 선택
- 정적 웹 사이트 호스팅 선택
- "이 버킷을 사용하여 웹 사이트를 호스팅합니다" 를 선택
- index.html 입력
- 저장

![](/assets/images/posts/Learn-AWS-Severless-Webapp/14.png)

### 6단계. 구현 검증

- 개요 탭으로 이동
- 업로드되어 있는 index.html 파일을 선택
![](/assets/images/posts/Learn-AWS-Severless-Webapp/15.png)
- 객체(파일)의 정보를 보면, 객체 URL이 있다. 이 URL 링크를 선택해서 웹페이지가 잘 뜨는지 확인


이것으로 모듈1의 단계가 끝났다. 다음 모듈2의 단계를 따라가보자.

------------

다음 글 : [AWS Severless 서버리스 Webapp 개발 따라하기 2](https://moon1z10.github.io/blog/Learn-AWS-Severless-Webapp-2/)

------------