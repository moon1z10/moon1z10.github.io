---
title:  "AWS EC2(Ubuntu)에서 Docker 이미지 생성"
excerpt: "AWS EC2 인스턴스(우분투 리눅스)에서 GitHub에 있는 Spring Boot 서버 코드를 git pull하여 가져오고, Gradle을 사용하여 코드를 컴파일 및 빌드한 다음 Docker 이미지로 만드는 과정"

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - AWS
  - EC2
  - Ubuntu
  - Docker
last_modified_at: 2024-04-01T13:36:00-00:00
---
------------

AWS EC2 인스턴스(우분투 리눅스)에서 GitHub에 있는 Spring Boot 서버 코드를 git pull하여 가져오고, Gradle을 사용하여 코드를 컴파일 및 빌드한 다음 Docker 이미지로 만드는 과정은 아래와 같습니다.

# 1. 필요한 도구 설치

- Git
- Java JDK
- Gradle
- Docker

# 2. Git 설치
```bash
sudo apt update
sudo apt install git
```

# 3. Java JDK 설치
Spring Boot 애플리케이션에 맞는 Java 버전을 설치합니다. 예를 들어, Java 11을 설치하는 방법은 다음과 같습니다.
```bash
sudo apt install openjdk-11-jdk
```

# 4. Gradle 설치
Gradle을 설치합니다. 아래 명령은 SDKMAN을 사용하는 방법으로, 다른 버전의 Gradle을 손쉽게 관리할 수 있게 해줍니다.
```bash
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install gradle
```
이후 gradle -v 명령으로 설치가 잘 되었는지 확인할 수 있습니다.

# 5. Docker 설치
Docker를 설치합니다. 이전 [포스트](2024-04-01-AWS-Ubuntu-Docker-Installation.md)서 제공된 단계를 참조하여 Docker를 설치하십시오.

# 6. GitHub에서 Spring Boot 프로젝트 클론
GitHub에서 프로젝트를 클론합니다.
```bash
git clone https://github.com/yourusername/your-spring-boot-repo.git
cd your-spring-boot-repo
```

# 7. 프로젝트 빌드
Gradle을 사용하여 Spring Boot 애플리케이션을 빌드합니다.
```bash
gradle build
```

# 8. Dockerfile 작성
Spring Boot 애플리케이션을 컨테이너화하기 위한 Dockerfile을 프로젝트 루트 디렉토리에 작성합니다.
```Dockerfile
# Dockerfile
FROM openjdk:11
COPY build/libs/yourapp.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

`yourapp.jar`는 단계 7에서 생성된 jar 파일의 이름으로 교체해야 합니다.

# 9. Docker 이미지 빌드
```bash
docker build -t yourapp:latest .
```
이 커맨드는 현재 디렉토리의 Dockerfile을 사용하여 yourapp:latest라는 태그의 Docker 이미지를 빌드합니다.

# 10. Docker 컨테이너 실행
```bash
docker run -d -p 8080:8080 yourapp:latest
```
이 명령은 8080 포트에서 yourapp:latest 이미지를 사용하여 Docker 컨테이너를 백그라운드에서 실행합니다. 필요에 따라 포트 번호를 조정할 수 있습니다.

------------