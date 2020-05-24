---
title: "[스프링] 스프링 핵심 개념 정리 - DIP [2/3]"
layout: post
tags: '스프링'
subtitle: '스프링의 핵심 개념인 제어의 역전(IoC), 의존 관계 역전 원칙(DIP), 의존성 주입(DI)을 학습한다.'
---

## 들어가며
---
지난 포스팅에서 Factory 패턴을 이용하여 IoC 원칙을 구현하는 방법에 대해서 배웠고, 느슨하게 결합된 디자인의 첫번째 단계를 구현했다. 느슨하게 결합을 구현하기 위한 두번째 단계로 DIP 원칙을 어떻게 구현하는지 알아보겠다. 

> Example Image 


&nbsp;
## DIP(Dependency Inversion Principle)이란?
DIP를 우리말로 해석하면 의존관계 역전이다. DIP는 Robert Martin에 의해서 만들어진 SOLID 객체지향 원칙 중 하나다.

DIP 원칙은 다음과 같다.
1. 상위(High-level) 모듈은 하위(Low-level) 모듈에 의존하면 안된다. 두 모듈은 모두 추상화 클래스에 의존해야 한다.
2. 추상화 클래스는 세부사항에 의존하면 안된다. 세부사항이 추상화 클래스에 의존해야한다.

DIP를 이해하기 위해 이전 포스팅에서 만들었던 예제를 살펴보자.

```java
public class CustomerBusinessLogic {
    
    public CustomerBusinessLogic() {}

    public String getCustomerName(int id) {
        DataAccess dataAccess = DataAccessFactory.getDataAccessObj();
        return dataAccess.getCustomerName(id);
    }
}

public class DataAccessFactory {

    public static DataAccess GetDataAccessObj() {
        return new DataAccess();
    }
}

public class DataAccess {
    
    public DataAccess() {}

    public String getCustomerName(int id) {
        return "Dummy Customer Name" // get it from DB in real app
    }
}
```

위의 예제는 IoC 원칙을 달성한 Factory 패턴의 구현 예제다. 하지만 CustomerBusinessLogic 클래스는 DataAccess의 구상 클래스(concrete class)를 이용한다. 따라서 이 클래스는 여전히 강한 결합 상태이다. 

CustomerBusinessLogic과 DataAccess 클래스에 DIP 사용하여 느슨한 결합 상태를 만들어보자.

DIP의 정의에 따라 상위 모듈은 하위 모듈에 의존하면 한된다. 두 모듈은 모두 추상화에 의존해야한다. 먼저 어떤 클래스가 상위 모듈인지 결정해보자. 상위 모듈은 다른 모듈에 의존하는 모듈이다. 위 예제에서 CustomerBusinessLogic 클래스는 DataAccess 클래스에 의존한다. 따라서 CustomerBusinessLogic 클래스가 상위 모듈이고, DataAccess 클래스가 하위 모듈이다. DIP의 첫 번째 원칙에 의해서 CustomerBusinessLogic 클래스는 DataAccess의 구상 클래스에 의존해서는 안된다. 대신 두 클래스는 모두 추상 클래스에 의존해야한다.

DIP의 두번째 규칙은 추상화 클래스가 세부사항에 의존하면 안되고, 세부사항이 추상화 클래스에 의존해야한다는 것이다.

> ### 추상화란?
추상화와 캡슐화는 객체지향 프로그래밍의 중요한 원칙이다. 이에 대한 다양한 정의가 있긴 하지만, 위의 예제를 통해 추상화가 무엇인지 알아보겠다.



&nbsp;
## 출처
---
- [Spring Core Framework Tutorials](https://www.journaldev.com/2888/spring-tutorial-spring-core-tutorial)
- 스프링 부트로 배우는 자바 웹 개발 (제이펍)
