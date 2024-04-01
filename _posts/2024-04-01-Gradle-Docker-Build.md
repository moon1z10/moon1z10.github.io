---
title:  "AWS EC2(Ubuntu)에 Docker 설치하는 방법"
excerpt: "AWS EC2(Ubuntu)에 Docker 설치하는 방법 설명"

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

# Docker 설치 가이드 (Ubuntu Linux on AWS EC2)

AWS EC2 인스턴스에서 Ubuntu Linux를 사용하여 Docker를 설치하는 단계별 가이드입니다.

## 1. 패키지 인덱스 업데이트

터미널을 열고 다음 명령어를 실행하여 Ubuntu의 패키지 리스트를 최신 상태로 업데이트합니다.

```bash
sudo apt-get update
```

## 2. 패키지 인덱스 업데이트

`apt`가 HTTPS를 통해 저장소에서 패키지를 다운로드할 수 있도록 필요한 패키지들을 설치합니다.

```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

## 3. Docker의 공식 GPG 키 추가

Docker 저장소의 공식 GPG 키를 시스템에 추가합니다.

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

## 4. Docker 저장소 추가

시스템의 `apt` 소스 리스트에 Docker의 공식 저장소를 추가합니다.

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

## 5. 패키지 인덱스 업데이트

새로운 저장소를 추가한 후에는 다시 한번 패키지 인덱스를 업데이트합니다.

```bash
sudo apt-get update
```

## 6. Docker CE 설치

이제 Docker CE(커뮤니티 에디션) 및 CLI 도구를 설치할 수 있습니다.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## 7. Docker 서비스 시작 및 자동 시작 설정

Docker 서비스를 시작하고, 시스템 부팅 시 자동으로 시작되도록 설정합니다.

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## 8. Docker 그룹에 사용자 추가 (선택 사항)

sudo 없이 Docker 명령을 실행하려면, 현재 사용자를 docker 그룹에 추가합니다.

```bash
sudo usermod -aG docker ${USER}
```

## 설치 확인

Docker가 성공적으로 설치되었는지 확인하기 위해, 설치된 Docker의 버전을 출력합니다.

```bash
docker --version
```

Docker가 정상적으로 설치되었다면, 해당 명령은 설치된 Docker의 버전 정보를 출력할 것입니다.


------------