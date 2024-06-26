---
title:  "COVID-19 대시보드 홈페이지를 제작했습니다."
excerpt: "코로나19 대시보드 홈페이지를 개설했어요!"

toc: true
toc_sticky: true
toc_label : "페이지 목차"

categories:
  - Development
tags:
  - COVID-19
  - RaspberryPi
last_modified_at: 2020-03-08T12:00:00-00:00
---
------------

# COVID-19

뭔가 홈페이지 같은거를 집에서 만들어 보고 싶은데, 도통 뭐가 좋을지 떠오르지 않다가 COVID-19 관련 정보 사이트르를 만들어 보기로 결정했다.
그렇게 2일에 걸쳐서 만들어 사람들에게 홍보하기 시작했다.

> 개발 사이트 : [링크](http://bluearena.iptime.org)

개발하다가 느낀 몇가지 점들을 아래에 요약해 봤다.

------------

### 1. 데이터의 중요성을 다시 한번 깨닫는다.

생각보다 누적된 데이터를 가져오기가 쉽지가 않다.
질병관리본부 데이터는 항상 최신이기 때문에 과거 데이터를 검색하려면 일일히 뉴스 검색을 통해서 모아야했다.
각 언론사들은 이미 누적된 데이터가 있어서 활용하기도 했지만, 공식 데이터와 비교하는데도 꽤나 많은 시간이 필요로 했다.

### 2. 개발은 가장 간단히 할 수 있는 Bootstrap 을 이용했다.

개발하다보니 Bootstrap이 너무너무 편하다는 것을 뼈저리게 느꼈다.
반응형 웹을 개발하는데 정말 최적화된 놈이 아닌가 싶다.
왜 다들 요즘 Bootstrap을 쓰고 있는지 충분히 이해되는 상황.

### 3. 라즈베리 파이3에 최신 Raspbian을 깔고 서버를 고민했다.

DB까지 구현해서 데이터를 RESTful API로 호출해서 화면에 동적으로 그려줄까?
생각했었는데, 개발 기간이 너무 오래 걸릴 것 같았다.
그래서 어차피 공개되도 무방하기에 데이터를 json형식으로 올려놓고
javascript에서 해당 데이터를 읽어서 보여주면 될 것 같았다.

#### jQuery 이노오오옴!

$.getJSON()을 통해서 json파일을 읽어서 데이터 처리를 할 수 있는데, 이 녀석이 `ajax` 로 비동기 형식이다.
한마디로 내가 생각한 것보다 너무 복잡하게 흘러간다.
덕분에 삽질을 엄청나게하고 몇 시간을 날려먹었다.
프론트 엔드 안한지 많은 시간이 흘렀더니 빠르게 두뇌회전이 안된다.

### 4. 서버는 뭘로 할 것인가?

역시나 가장 유명한 Apache, 그리고 Tomcat. 
아니면 한창 뜨고 있고, 구인시에 많이 필요로 한다는 nginx?
개인적으로는 Apache가 더 많이 써본 경험도 있어서 Apache를 해볼까 하다가,
그래도 nginx를 써보는 경험도 좋을 것 같아서 `nginx` 로 결정했다.

근데 나는 nginx가 뭐 엄청난 만능인 줄 알았는데..그냥 프록시 웹 서버 수준이더라.
물론 깊게 들억가서 서버 세팅하고 분산 넣어주고 하면 더 많은 시간이 필요하겠지만...
...음...아무튼 간단히 세팅을 구글링을 통해 마쳤다.

그리고 다들 서버리스로 개바해서 그런가? 구인할 때 보면 딱히 Web Application Server 개발 경험은 안보고 nginx만 보는 곳도 있던데..
서버리스 때문인가 싶기도 하고..솔직히 잘 모르겠다.
nginx가 뭐 얼마나 대단한 물건인가 싶기도 하고...Amazon S3를 쓰고 보니 굳이 프록시 서버가 필요한건가? 싶기도 하고..
기술이 너무 많고 발전 속도가 빨라 따라가기 버겁다..ㅠㅠ

### 5. RESTful API 포기 이야기

당장 데이터를 긁어와서 화면에 보여줘야하는데, 빠른시간 내에 개발히기에는 내가 디비를 구성하고, 이것에 맞춘 API와 서비스를 구성하는 것은 +2~3일이 더 필요할 것 같았다.
물론 못할 것은 뭐겠냐만...이미 이런 시간 투자하기도 너무나 아깝다.
COVID-19 가 생각보다 진정될 기미가 보이지 않고, 일도 하고 개발도 하려면 시간이 턱없이 부족하다.
밤 사이에 개발히기에는 혼자서는 너무 많은 시간이 걸린다.

나중에 리팩토링하면 모를까...과감하게 포기하기로 한다.

### 6. 라즈베리 파이3의 이상한 점

vscode를 이용해서 개발하고 있는데, ssh로 접속해서 코드를 수정하고 저장하는데 파일 내용이 다 사라지게 저장이 됐다.
이게 뭐지? 해서 직접 ssh로 접속해서 보면 잘만 저장이 된다.

이상하게 여겨서... `df -h` 명령어로 확인해보니까..
`/dev/root` 가 100%(7GB) 풀이더라.
음???이해가 안됐다...
분명 Raspbian이 한 4기가 정도 했던 것 같은데, 말이 되나?
그래서 `sudo apt-get autoclean` 하니까..그나마 4% 공간이 확보됐다.
괜히 풀로 깔았나보다. 걍 CLI 용으로 400MB 정도 하는 걸로 설치할 걸...

조금 후회됐다.

아무튼 지금은 공간 확보 후에 vscode를 통해서 원격으로 접속후 코드 수정, 저장이 잘 되고 있다.

### 7. 세상은 넓고 정보는 많다.

솔직히 나도 COVID-19 대시보드 홈페이지를 만들생각하지 않았다면,
이렇게 좋은 정보들이 흩어져 있다는 사실을 깨닫지 못했을 거다.

실시간 업데이트, 데이터 시각화(뭐 단순히 Chart.js를 이용해서 보여주는 수준이겠지만) 등. 세상은 넓고 실력자들은 많다.

그 중에서도 눈에 띄는 사이트가 있었으니, 바로 KBS의 세계 확산을 지도상에서 시간의 흐름대로 볼 수 있게 해주는 것과 확산 관계망이였다.
이 정도로 잘 만들어놨는데 왜 나는 전혀 모르고 있었을까?
솔직히 나도 똑같이 만들어 볼 수 있는 있겠지만(카피캣), 굳이 그럴 필요가 있나 싶어서 링크만 달아놨다.
개인적으로 KBS 방송 보도는 마음에 들지 않았지만 이런거는 칭찬해야겠다.

다른 언론사에서 만들었는데 좋아보이는 것은 링크를 달아서 홍보도 해줄 생각이다.
칭찬 받을건 칭찬 해줘야지!


> 마무리말 : COVID-19의 확산이 국내에서 얼른 진정돠기를 바라며, 집 밖으로 좀 나가고 싶다. 모니터의 세상 말고 Real world로!

------------