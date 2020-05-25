---
layout: post
title: "[네트워크] 웹 캐시란?"
tags: ['네트워크', '웹 캐시']
subtitle: '웹 캐시의 개념, 종류, 작동 방식에 대해서 학습한다.'
---


&nbsp;
## 들어가며
---
[HTTP와 REST의 차이](https://ssongkr.github.io/2020/05/21/difference-between-http-and-rest.html)에 대해 포스팅하면서 웹 캐시에 대해서 가볍게 다루었다. 이번 포스팅을 통해서 웹 캐시가 무엇인지, 어떤 종류가 있는지, 그리고 어떻게 작동하는지 알아보고자 한다.


&nbsp;
## 웹 캐시란?
---
웹 캐시(Web cache)는 웹 서버(or origin server)와 클라이언트가 주고받는 요청(Request)을 모니터링하고, HTML 페이지, 이미지 파일과 같은 응답(Response) 메시지의 사본을 저장한다. 그런 다음 동일한 URL에 다른 요청이 오면, 원래 서버에 다시 요청하지 않고 사본을 응답할 수 있다.

웹 캐시가 사용하면 크게 두 가지 이점이 있다.
1. 지연시간(Latency) 감소: 원래 서버가 요청에 응답하는 대신 클라이언트와 가까운 웹 캐시가 요청에 응답할 수 있다. 따라서 요청에 대한 응답 시간이 감소하고, 웹이 더 실시간으로 반응하는 것처럼 보이게 한다.
2. 네트워크 트래픽 감소: 데이터(representations)가 재사용되기 때문에 클라이언트가 사용하는 대역 폭의 양이 줄어든다.

> Representations는 표현이라는 뜻으로 데이터를 표현하는 방식의 종류를 이야기한다. XML, JSON, 이미지 등이 예시가 될 수 있다.


&nbsp;
## 웹 캐시의 종류
---
#### 브라우저 캐시(Browser Caches)
사파리, 크롬, 파이어폭스 등 최신 브라우저의 설정 창을 보면 **캐시** 설정을 볼 수 있다. 브라우저 캐시는 사용자가 본적 있는 데이터(representations)의 일부를 컴퓨터 하드 디스크의 한 부분에 따로 저장한다. 이 캐시는 간단한 규칙에 따라 작동하는데, 일반적으로 캐싱 된 데이터가 최신 데이터인지 확인하여 데이터를 새로 요청할지 여부를 결정한다.

캐시는 사용자가 웹 브라우저 상에서 **뒤로가기 버튼**을 누르거나, 링크를 클릭하여 방금 본 페이지를 볼 때 특히 유용하다. 또한, 사이트 전반에 걸쳐 이미 본 적 있는(캐싱 된) 이미지를 사용할 경우, 브라우저 캐시를 이용하여 이미지를 빠르게 제공한다.

&nbsp;
#### 프록시 캐시(Proxy Caches)
프록시 캐시도 브라우저 캐시와 동일한 원리로 작동한다. 하지만 브라우저 캐시보다 더욱 큰 규모로 작동한다는 점에서 차이가 있다. 프록시 서버는 동일한 방식으로 수천, 수만명의 사용자에게 서비스를 제공한다. 대기업이나 ISP는 보통 방화벽이나 중개 장치에 설정한다.

프록시 캐시는 클라이언트나 서버(Origin server)의 일부가 아니고, 네트워크 외부에 있기 때문에, 요청이 어떻게든 전달되야 한다. 요청을 올바르게 전달하는 한 가지 방법은 브라우저 프록시 설정을 이용하여 사용할 프록시를 수동으로 설정하는 것이다. 또 다른 방법은 가로채기(intercept)를 이용하는 것이다. 인터셉션 프록시(Interception proxy)는 네트워크 상에서 웹 요청이 그들에게 리다이렉트 되도록 한다. 그러면 클라이언트는 별 다른 설정을 하지 않아도 프록시 캐시를 이용할 수 있다.

프록시 캐시는 여러 사용자에게 공유되는 캐시다. 그리고 프록시 캐시는 지연 시간과 네트워크의 트래픽을 줄이는데 매우 유용하다. 그 이유는 인기있는 데이터가 아주 많이 재사용되기 때문이다.

&nbsp;
#### 게이트웨이 캐시(Gateway Caches)
**Reverse proxy caches**, **Surrogate caches**라고 불리기도 하는 게이트웨이 캐시는 프록시 캐시와 마찬가지로 중개자 역할을 한다. 하지만 네트워크 관리자가 대역폭을 줄이기 위해 배포하는 것이 아니라, 웹 마스터가 사이트의 확장성과 안정성, 그리고 성능을 개선하기 위해 자체적으로 배포한다.

요청은 다양한 방법으로 게이트웨이 캐시로 라우팅 될 수 있지만, 일반적으로 클라이언트에게 캐시가 원본 서버처럼 보이도록 로드 밸런서(Load banancer)가 사용된다.


&nbsp;
## 웹 캐시의 작동 방식
---
모든 캐시는 언제 캐시에서 데이터를 제공할 것인지 결정하는 규칙을 갖고 있다. 이 규칙들 중 일부는 프로토콜(HTTP 1.0/1.1)에서 설정되고, 일부는 캐시 관리자에 의해서 설정된다. (브라우저 캐시 혹은 프록시 캐시 관리자)

가장 일반적인 규칙은 다음과 같다.
1. 응답 헤더(Response header)가 캐시에 저장하지 않도록 지시하면, 캐시에 저장하지 않는다.
2. 인증이나 보안 처리(HTTPs)가 된 요청은 공유 캐시에 의해 캐시되지 않는다. 
3. 다음과 같이 캐시된 데이터는 최신 상태로 간주되고, 클라이언트는 캐시로부터 데이터를 전달받는다.
    - 만료 시간(expiry time)이나 age-controlling header를 가지고 있으며, 이 기한이 여전히 유효한 경우
    - 캐시가 데이터를 최근에 확인했는데 해당 데이터가 오래 전에 수정된 데이터인 경우
4. 오래 된 데이터라면, 캐시는 원본 서버에 사본이 유효한(최신의) 데이터인지 확인 요청을 한다.
5. 특정 상황(예: 네트워크와의 연결이 끊어진 경우) 에서 캐시는 원본 서버를 확인하지 않고 응답을 제공할 수 있다.

응답에 유효성 검사기(ETag or Last-Modified header)가 없고, 최신 정보인지 확인할 수 있는 명시적인 데이터가 없는 경우 캐시할 수 없다고 여겨진다.


&nbsp;
## 마무리하며
---
이번 포스팅을 통해 웹 캐시의 전반에 대해 알아보았다. 다음 포스팅에서는 잠시 언급했던 로드 밸런서에 대한 개념을 자세히 살펴보고, HTTP 캐시 헤더의 구체적인 내용에 대해 알아보도록 하겠다.


&nbsp;
## 참고
---
- [https://www.mnot.net/](https://www.mnot.net/cache_docs/#DEFINITION)
- [https://devcenter.heroku.com/](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers)