---
title: "스프링부트로 REST API 구현하기[2/5]"
layout: post
---

## 들어가며
지난 포스팅에서 REST API가 무엇인지 알아보았다. 이번 프로젝트로 간단한 연락처 앱을 작성할 예정이다. 최종적으로 HTTP Method를 이용해 연락처를 추가, 변경, 삭제하는 기능을 구현할 예정이다.

이번 포스팅에서는 스프링부트 프로젝트를 생성하는 방법과 데이터베이스에 저장할 연락처 모델을 작성해보도록 하겠다.

---
## 스프링부트 프로젝트 생성하기
[Spring Initializr](https://start.spring.io/) 사이트를 이용하면 쉽고 빠르게 스프링부트 프로젝트를 생성할 수 있다. 프로젝트의 환경은 다음과 같이 설정하면 된다.

 - Project : Maven
 - Language : Java
 - Version : 2.2.5
 - Deapendency : Web, JPA, H2, Lombok

Generate 버튼을 통해 프로젝트를 생성하면 압축 파일(*.zip)이 생성된다. 원하는 폴더에 압축을 풀고, 자신이 사용하는 에디터로 프로젝트를 실행하면 프로젝트 생성이 완료된다. 만약 VS Code를 사용하고 있다면 [vscode-spring-initializr-extension](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-spring-initializr)을 이용하여 프로젝트를 생성할 수 있다.


---
## 모델 생성하기
연락처에는 이름, 전화번호, 그룹, 이메일 등 다양한 데이터가 저장될 수 있다. 하지만 이번 프로젝트의 목표는 상용 프로그램의 제작이 아닌 스프링의 구조를 이해하기 위함이기 때문에 이름과 전화번호만 저장하는 간단한 객체 모델을 정의하도록 하겠다.

**/src/main/java/contacts/Contacts.java**

```java
package contacts;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

import lombok.Data;

@Data
@Entity
class Contacts {
    private @Id @GeneratedValue Long id;
    private String name;
    private String phoneNumber;

    Contacts() {}

    Contacts(String name, String phoneNumber) {
        this.name = name;
        this.phoneNumber = phoneNumber;
    }
}
```
 
---
## 코드 살펴보기
위의 Contacts class 굉장히 짧고 간단하다. 하지만 많은 정보를 가지고 있다. 

### @Data
우리는 프로젝트를 생성하면서 Lombok dependency를 추가하였다. Lombok은 자바에서 모델 객체를 만들 때 생성자, Getter/Setter, toString 등 불필요하게 반복적으로 만들어지는 코드를 Annotation을 통해 줄여주는 라이브러리다.

### @Entity
Database의 Entity를 나타낸다. Database에서 Entity란 저장되고 관리되야하는 데이터의 집합을 의미한다.

@Entity는 JPA Annotation이다. JPA를 이해하기 위해서는 ORM에 대해 알아야한다. ORM은 Object Relational Mapping의 약어로 객체와 데이터베이스의 데이터를 자동으로 매핑하는 것을 말한다. 그렇다면 객체와 데이터베이스의 데이터를 매핑하는 과정이 왜 필요할까? 그 이유는 객체 지향 언어는 클래스를 사용하고 관계형 데이터베이스는 테이블을 사용해, 객체 모델과 관계형 모델 사이에 불일치가 발생하기 때문이다. ORM은 불일치 문제를 자동으로 해결해 프로그래머가 오직 비즈니스 로직에(객체지향 프로그래밍)만 집중할 수 있게 해준다.

JPA는 Java Persistence API의 약어로, 자바 진영의 ORM 기술 표준 인터페이스다. JPA 인터페이스의 구현체는 Hibernate, OpenJPA, EclipseLink 등이 있다.

결과적으로, Entity annotation은 클래스(객체)가 실제 데이터베이스의 테이블과 매핑 될 클래스임을 명시한다.

### @Id & @GeneratedValue
@Id와 @GeneratedValue는 모두 JPA Annotation이다. @Id는 해당 변수가 기본키(Primary Key)임을 나타내고, @GeneratedValue는 기본키의 값의 자동 생성 전략을 명시하는데 사용한다. 예를들어 @GeneratedValue(strategy = GenerationType.IDENTITY) 구문을 통해 기본키 값의 생성을 데이터베이스에 위임할 수 있다. 이외에도 다른 전략을 명시할 수 있따.


## References
https://spring.io/guides/tutorials/rest/

https://www.inflearn.com/course/ORM-JPA-Basic#description
