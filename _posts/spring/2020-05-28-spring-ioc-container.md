---
title: "[스프링] 스프링 핵심 개념 정리 - IoC Container [4/4]"
layout: post
tags: '스프링'
subtitle: '스프링의 핵심 개념 IoC Container를 학습한다.'
---


## 들어가며
---
지난 포스팅에서 DI(Dependency Injection)을 구현하였다. 느슨하게 결합된 상태를 얻기 위한 마지막 과정인 IoC Container(a.k.a. DI Container)에 대해서 알아보도록 하겠다.

![getting-loosely-coupled-class](/images/spring-ioc-container-1.png)

&nbsp;
## IoC Container란?
IoC Container는 자체적으로 의존성을 주입하도록 돕는 프레임워크로 런타임에 생성자, 속성, 메서드를 통해 지정 된 객체에 의존성을 주입한다. 또한 객체의 생성 직접 생성하며 해당 객체의 생명 주기를 관리한다. 이로 인해 우리는 객체를 직접 생성하거나 관리 할 필요가 없어진다.

모든 컨테이너는 반드시 다음 DI 생명주기를 따르도록 지원해야한다.

- 등록(Register): 컨테이너는 특정 타입이 발견 될 때, 인스턴스화 할 의존 객체를 알고 있어야 한다. 이 과정을 등록(Registration)이라고 한다. 기본적으로 타입 매핑(type-mapping)을 등록하는 방법이 포함되어 있어야한다.

- 해결(Resolve): IoC 컨테이너를 사용할 경우 우리는 객체를 직접 만들 필요가 없다. 객체를 만드는 것은 컨테이너의 역할이다. 이것을 해결(Resolution)이라고 한다.

- 처리(Dispose): 컨테이너는 의존 객체의 생명주기를 관리해야한다. 대부분의 IoC 컨테이너는 서로 다른 생명 주기 관리자를 가지고 있고, 관리자는 생명 주기를 관리하고 처리한다.


&nbsp;
## 참고
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
