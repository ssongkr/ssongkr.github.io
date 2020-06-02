---
layout: post
title: "SPA(Single Page Application)란?"
tags: 'SPA'
subtitle: '스프링의 핵심 개념 IoC Container를 학습한다.'
---

## 들어가며
--- 
웹 프로그래밍을 시작하기 전 React Native를 이용하여 앱 개발을 먼저 시작하였다. 이후 웹에 대해 관심을 가지면서 SPA가 무엇인지 찾아봤었지만, 웹에 대한 경험이 없어서 이에 대해 잘 이해를 하지 못했다. 이번 포스팅을 통해 SPA가 무엇이고, 언제 사용해야하고, 어떠한 장단점이 있는지 알아보도록 하겠다. MPA와 비교했을때.


&nbsp;
## SPA가 무엇이고, 왜 SPA를 사용해야하는가?
---
SPA(Single Page App)는 페이지를 새로 로딩할 필요가 없는 앱이다. 브라우저에서 사용하고 작동하는 동안. Facebook, Google Maps, Gmail, Twitter, Google Drive, GitHub와 같은 앱을 생각해보자. 이 모든 예시는 SPA다. 

올바르게 구성된 SPA의 한가지 큰 장점 중 하나는 사용자 경험(UX)다. 여기서 사용자는 페이지가 다시 로드 될 때까지 기다리지 않고도 앱 본연의 환경을 즐길 수 있다. 너는 같은 페이지에 있을 것이다. 자바스크립트 언어에 의해서 powered 되는.

더 나가기 앞서 자주 보게 될 3가지 약어가 있다.
- SPA: single-page application(위에 언급한 내용과 상동)
- MPA: multi-page application(링크를 누를 때 페이지를 로딩하는 전통적인 앱)
- PWA: progressive web application(웹 사이트: 자바스크립트나 해당 프레임 워크를 사용하여 빌드되고 앱처럼 작동할 수 있는 웹 사이트)


&nbsp;
## SPA의 장점: 왜 SPA를 사용하는가?
SPA의 가장 큰 장점은 속도다. SPA가 필요로하는 대부분의 자원(HTML, CSS, Scripts)는 앱 실행 시 로드된다. 그리고 페이지를 사용하는 동안 다시 로드될 필요가 없다. 변하는 유일한 것은 데이터다. 서버로 전송하고, 전송되는 데이터. 결과적으로 앱은 매우 사용자의 쿼리에 반응적이다. 그리고 클라이언트와 서버간 통신을 항상 기다릴 필요도 없다.

https://i.ibb.co/jbXcbXY/quotes-speed-impact.png




https://huspi.com/blog-open/definitive-guide-to-spa-why-do-we-need-single-page-applications