---
title: "[CSS] CSS 네이밍 규칙 - BEM"
layout: post
---


---
## 들어가며
현재 React를 기반으로 한 정적 웹사이트 생성기 Gatsby를 이용하여 개인 블로그를 제작하고 있다. 하지만 웹 프론트엔드를 제대로 배운적이 없기 때문에 CSS 요소의 이름을 정하는데 어려움이 있었다. 이번 포스팅을 통해 CSS 요소의 이름을 효율적으로 짓는 방법에 대해 살펴보도록 하겠다.

---
## 하이픈으로 구분 된 문자열
많은 프로그래밍 언어에서 변수의 이름을 지정할 때 Camel Case를 이용한다. 하지만 CSS의 요소 이름은 일반적으로 하이픈을 이용하여 만든다. 이는 CSS의 속성 이름과 일치하여 깔끔하고, 가독성이 좋다는 장점이 있다.

```css
.redBox { ... } // wrong

.red-box { // correct
  font-weight: 10em;
  border: 1px solid red;
}
```

## BEM 네이밍 규칙
BEM은 Block, Element, Modifier를 뜻한다. 그리고 일반적으로 Class의 네이밍을 위해 사용된다.

### Block
Block은 일반적으로 navigation, header, footer 같은 디자인 블록을 나타낸다.

```css
.nav { ... }

.header { ... }

.footer { ... }
```

### Element
Block을 구성하고 있는 요소들을 나타낸다. Block__Element 형태의 이름을 가지고 있다.

```css
.nav__list { ... }

.header__title { ... }

.header__logo { ... }
```

### Modifier
요소를 수정하기 위해 사용한다. 예를들어 헤더의 디자인은 모바일과 PC에서 서로 다를 수 있을 것이다. 

```css
.header--pc { ... }
.header--mobile { ... }
```

BEM을 모두 적용한 예제를 살펴보자. 

```css
.header__title--big { ... }
.header__title--small { ... }
```

웹 프론트엔드에 대한 지식이 많이 없기 때문에 가장 기초적인 부분에 대해서만 다루었다. 나중에 필요하다면 조금 더 심화된 명령어 규칙 및 파일 구조를 나누는 방법을 살펴보도록 하겠다.

## References
[freecodecamp](https://www.freecodecamp.org/news/css-naming-conventions-that-will-save-you-hours-of-debugging-35cea737d849/)

[naver-blog-슘잉애](https://m.blog.naver.com/PostView.nhn?blogId=sum2ne&logNo=221362588553&proxyReferer=https%3A%2F%2Fwww.google.com%2F)