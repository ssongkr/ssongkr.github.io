---
title: "JDBC, JPA/ORM, SQL Mapper 이해하기"
layout: post
tags: ['JDBC', 'JPA', 'ORM', 'SQL Mapper', 'Hibernate', 'Mybatis']
subtitle: 'JDBC, JPA, ORM, SQL Mapper, Hibernate, Mybatis에 대해 학습한다'
---

## 들어가며
---
최근 스프링을 공부하면서 JDBC와 MyBatis를 사용하여 데이터베이스를 연동하고있다. 그런데 스프링 프레임워크에 익숙해지는데 집중하다보니, 해당 개념들을 명확하게 숙지하지 못한 상태에서 프로그래밍만 하고 있었다. 이번 포스팅을 통해 JDBC와 JPA/ORM, 그리고 SQL Mapper에 대해서 명확히 정리하고자 한다.


&nbsp;
## JDBC란?
---
JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다. JDBC는 데이터베이스에서 자료를 쿼리하거나 업데이트하는 방법을 제공한다.

### JDBC 아키텍처
![jdbc-architecture](/images/jdbc-architecture.png)

#### Java Application
클라이언트나 서버에서 독립적으로 실행되는 자바 프로그램

#### JDBC API
자바 프로그램을 데이터베이스와 연결하거나 통신할 수 있도록 돕는 클래스와 인터페이스를 제공한다.

#### JDBC Driver Manager
데이터베이스 드라이버들을 관리한다. 올바른 드라이버를 사용하여 각각의 데이터 소스에 접근하도록 한다.

#### JDBC Driver
이 인터페이스는 데이터베이스와의 통신을 처리한다.


&nbsp;
## Persistent와 Persistence Framework
---
JPA와 ORM을 설명하기에 앞서 **영속성**과 **Persistence Framework**가 무엇인지 살펴보자.

#### 영속성(Persistence)
영속성이란 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성이다. 영속성이 없는 데이터는 메모리에 저장되기 때문에 프로그램을 종료하면 데이터가 모두 사라진다. 따라서 해당 데이터를 파일 시스템이나 데이터베이스에 저장하여 영속성을 부여할 수 있다.

#### Persistence Framework
Persistence Framework는 데이터의 저장, 조회, 변경, 삭제를 다루는 클래스 및 설정 파일들의 집합이다. JDBC 프로그래밍의 복잡함이나 번거로움 없이 간단한 작업만으로 데이터베이스와 연동되는 시스템을 빠르게 개발할 수 있으며 안정적인 구동을 보장한다.

Persistence Framework는 크게 ORM(Object Relational Mapping)과 SQL Mapping 두가지로 나뉘어진다.


&nbsp;
## ORM(JPA)의 탄생 배경
---
소규모라도 JDBC나 이용하여 애플리케이션을 만들어 보았다면 알 수 있겠지만, DAO 소스 코드의 작성은 단순 반복되는 작업이어서 상당히 귀찮고 많은 시간을 소모한다.

SQL Mapper인 Mybatis를 사용하면 코드가 보다 간결해지고, SQL을 XML로 따로 관리하여 유지보수가 편해진다. 하지만 Mybatis 역시 SQL 쿼리를 작성하는 과정이 단순 반복이기 때문에 매우 귀찮다. 뿐만 아니라 테이블에 칼럼이 추가되는 등의 변동사항이 있다면 이와 관련된 모든 DAO의 SQL문을 수정해야한다. (DAO와 테이블이 강한 의존성을 갖고 있다)

단순 반복 노동을 해결하기 위해 다양한 툴이 개발 됐었지만, 큰 효과를 거두지 못했다. 그 이유는 자바는 객체 지향 패러다임으로 만들어졌고, 데이터베이스는 관계형 패러다임으로 만들어졌기 때문이다. 탄생 배경부터 다른 패러다임을 두고 만들어졌기 때문에 두 소프트웨어 사이에는 어쩔 수 없는 간극이 있다.


&nbsp;
## ORM(Object Relational Mapping)
---
단순 반복 작업과 패러다임 불일치 문제를 해결하고자 만든 기술이 ORM이다. ORM은 자바의 객체와 데이터베이스의 테이블을 매핑 시켜 준다.*(자바에 국한된 기술은 아니다)*

ORM을 사용하면 SQL의 쿼리를 자바의 메서드로 조작할 수 있다. 예를 들어 MySQL에서 `User` 테이블의 데이터를 조회하려면 `select * from user`라는 쿼리를 실행해야한다. 그러나 ORM을 사용하면 `userInstance.findAll()`라는 메서드를 호출하여 데이터를 조회할 수 있다.

이처럼 ORM을 사용하면 메서드 호출로 쿼리를 수행할 수 있기 때문에 생산성이 높아진다. 하지만 쿼리가 복잡해지면 ORM으로 표현하는데 한계가 있고 성능이 비교적 느리다는 단점이 있다. 


&nbsp;
## JPA(Java Persistent API)
---
JPA는 자바 진영에서 표준 스펙으로 정의된 API 명세서로, ORM을 사용하기 위한 **인터페이스**의 집합이다. 따라서 JPA를 사용하기 위해서는 JPA를 구현한 Hibernate, EclipseLink, DataNucleus 같은 ORM 프레임워크를 사용해야한다.

&nbsp;
### Hibernate
JPA를 구현한 여러 프레임워크가 있지만, Hibernate가 JPA를 주도하고 있기 때문에, Hibernate의 특징을 살펴보도록 하겠다.


### 장점
- **생산성** - Hibernate는 SQL을 직접 사용하지 않고, 메서드 호출만으로 쿼리가 수행된다. SQL 반복 작업을 하지 않으므로 생산성이 높아진다.

- **유지보수성** - Mybatis를 사용하면 테이블의 컬럼이 변경되었을 경우 DAO, SQL 등을 직접 수정해야 한다. 그런데 JPA를 사용하면 이러한 일련의 작업들을 JPA가 대신 해주기 때문에 유지보수성이 높다.

- **벤더 종속성** - JPA는 특정 벤더(MySql, Oracle)에 종속적이지 않다. 설정파일에 어떤 데이터베이스를 사용할지 설정하면 데이터베이스가 변경된다.


### 단점
- **성능** - 메서드 호출을 통해 쿼리를 실행하기 때문에 SQL을 직접 호출하는 것보다 성능이 떨어진다. 지금은 많이 발전하여 좋은 성능을 보여주고 있다.

- **정확성** - 객체간의 매핑(Entity Mapping)이 잘못되거나, JPA를 잘못 사용하여 의도하지 않은 동작을 할 수 있다.

- **러닝커브** - JPA를 잘 사용하기 위해서는 알아야 할 것이 많다. 이러한 복잡성을 해결하고자 최근에는 Spring Data JDBC가 주목을 받고 있다.




&nbsp;
## 출처
---
- https://ko.wikipedia.org/wiki/JDBC
- http://examradar.com/jdbc-connectivity-model-architecture/
- https://victorydntmd.tistory.com/195
- https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html
- https://kok202.tistory.com/84