---
title: "[스프링] 스프링 핵심 개념 정리 - IoC Container [4/4]"
layout: post
tags: '스프링'
subtitle: '스프링의 핵심 개념인 제어의 역전(IoC), 의존 관계 역전 원칙(DIP), 의존성 주입(DI)을 학습한다.'
---


## 들어가며
---
지난 포스팅에서 Dependency Injection 패턴을 구현하는 방법에 대해 학습하였다. IoC Container(a.k.a. DI Container)는 자동으로 DI를 구현하도록 돕는 프레임워크다. IoC Container는 객체의 생성과 생명주기를 관리하고, 클래스에 의존성을 주입한다.

IoC 컨테이너는 지정된 클래스의 객체를 만들고, 런타임에 생성자, 속성, 메서드를 통해 모든 종속성 객체를 주입하고, 적절한 시간에 이를 모두 처리한다. 
이로 인해 우리는 객체를 생성하거나 직접 관리 할 필요가 없어진다.

모든 컨테이너는 반드시 다음 DI 생명주기를 따르도록 지원해야한다.

- 등록(Register): 컨테이너는 특정 유형이 발견 될 때, 인스턴스화 할 종속성을 알고 있어야 한다. 이 과정을 등록(Registration)이라고 한다. 기본적으로 형식 매핑(type-mapping)을 등록하는 방법이 포함되어 있어야한다.

- 해결(Resolve): IoC 컨테이너를 사용할 경우 객체를 직접 만들 필요가 없다. 겍체를 만드는 것은 컨테이너의 역할이다. 이것을 해결(Resolution)이라고 한다. 컨테이너는 반드시 특정 형식을 해결하기 위한 방법을 포함해야한다. 컨테이너는 지정된 형식의 객체를 생성하고, 필요한 종석성을 주입하고, 객체를 반환한다.

- 처리(Dispose): 컨테이너는 종속 객체의 생명주기를 관리해야한다. 대부분의 IoC 컨테이너는 서로 다른 생명 주기 관리자를 가지고 있고, 관리자는 생명 주기를 관리하고 처리한다.




&nbsp;
## 참고
- [https://www.baeldung.com/](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
