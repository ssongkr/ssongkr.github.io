---
title: "스프링부트로 REST API 구현하기[1/5]"
layout: post
---

---
## REST API란?
REST API를 구현하기에 앞서 REST가 무엇인지 알아보자. REST는 Representational State Transfer의 약어로서 2000년도에 로이 필딩(Roy Fielding)의 박사학위 논문에서 최초로 소개되었다. 로이 필딩은 웹이 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 REST를 발표했다. REST의 기본 원칙을 성실히 지킨 서비스 디자인을 RESTful이라고 한다.

---
## REST API의 구성

### 1. 자원(Resource)
일반적으로 REST에서 자원은 URL을 말한다. 
```java
/todos
/todos/today
```

---
### 2. 행위(Verb)
Http method(GET, POST, PUT, DELETE etc..)를 나타낸다.
```java
GET /todos/today
```

---
### 3. 표현(Representations)
요청한 자원의 현재 상태를 표현하는 하나의 방식이다. XML, HTML, JSON 등이 있다.
```json
{
    "todo": "study english",
    "deadline": "2020-03-11"
}
``` 

다음 포스팅에서는 본격적으로 프로젝트를 생성하고, 소스코드를 작성하도록 하겠다.


## References
https://velog.io/@wlsdud2194/HTTP-REST-API-%EB%9E%80
https://poiemaweb.com/js-rest-api
https://symfonycasts.com/screencast/rest/rest