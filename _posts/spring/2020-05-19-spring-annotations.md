---
title: "[스프링] 어노테이션 살펴보기"
layout: post
tags: ['스프링', 'annotation']
subtitle: '스프링의 구조와 설정을 위해서 사용되는 어노테이션에 대해서 학습한다'
---

## 어노테이션이란?
---
- 자바 1.5 버전부터 지원되는 기능으로 일종의 `메타데이터(metadata)`다.
- 어노테이션의 사전적 의미는 주석이다. 주석처럼 코드에 사용할 수 있고, 컴파일 또는 런타임 시에 해석된다.


&nbsp;
## 스프링 어노테이션의 종류
--- 
#### 1. Spring @Bean
- @Bean 어노테이션은 메서드에 적용된다.
- 스프링 컨텍스트(Spring context)에 의해 관리되는 `빈(Bean)`을 반환하는 메서드라는 것을 명시한다.
- 일반적으로 Configuration 클래스의 메서드에 선언된다.

#### 2. Spring @Service
- @Service 어노테이션은 특별화 된 @Component 어노테이션이다.
- 오직 클래스에만 적용될 수 있다.
- 클래스가 서비스를 제공한다는 것을 명시한다.

#### 3. Spring @Component
- @Component 어노테이션은 클래스가 컴포넌트(Component)라는 것을 명시한다.
- 스프링 프레임워크는 의존성 주입(Depondency Injection)할 때, 해당 클래스들을 자동으로 감지한다.

#### 4. Spring @RestController
- @RestController은 @Controller와 @ResponseBody이 함께 처리 되어있는 편리한 어노테이션인다.
- 클래스가 Request Handler라는 것을 명시하기 위해 사용된다.

#### 5. Spring @Controller
- @Controller 어노테이션은 특별화 된 @Component 어노테이션이다.
- RequestMapping 어노테이션을 기반으로 어노테이션이있는 핸들러 메소드와 함께 사용된다.

#### 6. Spring @Repository
- @Repository 어노테이션은 클래스가 저장, 검색, 업데이트, 삭제 조작을 위한 메커니즘을 제공한다는 것을 나타낸다.

#### 7. Spring @Configuration
- @Configuration 어노테이션은 스프링 코어 프레임워크의 일부다.
- 클래스가 @Bean이 정의된 메서드를 가지고 있다는 것을 나타낸다.
- 스프링 컨테이너는 이를 기반으로 애플리케이션에서 사용 될 클래스를 처리하고 스프링 빈(Bean)들을 생성한다.

#### 8. Spring @Value
- @Value 어노테이션은 변수나 메서드의 매개변수에 기본 값을 지정하기 위해 사용된다.
- 스프링 환경 변수와 시스템 변수를 읽을 수 있다.

#### 9. Spring @PropertySource
- @PropertySource 어노테이션은 스프링 환경에 속성(Property) 파일을 제공하기 위해 사용된다.
- @Configuration 클래스와 함께 사용된다.

#### 10.  Spring @PostConstruct and @PreDestroy
- @PostConstruct 어노테이션이 적용된 메서드는 스프링 빈(Bean)이 초기화 된 직후에 실행된다.
- @PreDestroy 어노테이션이 적용된 메서드는 컨텍스트(Context)로부터 빈(Bean)이 제거될 때 호출된다.

### 11. Spring @Async
- @Async 어노테이션은 스프링에서 비동기 메서드를 생성할 수 있도록 한다.


&nbsp;
## 출처
---
- [Spring Core Framework Tutorials](https://www.journaldev.com/2888/spring-tutorial-spring-core-tutorial)
- 스프링 부트로 배우는 자바 웹 개발 (제이펍)
