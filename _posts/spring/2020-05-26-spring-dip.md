---
title: "[스프링] 스프링 핵심 개념 정리 - DIP [2/4]"
layout: post
tags: '스프링'
subtitle: '스프링의 핵심 개념인 제어의 역전(IoC), 의존 관계 역전 원칙(DIP), 의존성 주입(DI)을 학습한다.'
---

## 들어가며
---
지난 포스팅에서 Factory Pattern을 이용하여 IoC 원칙을 구현해, 느슨한 결합의 첫번째 단계를 완성하였다. 하지만 이는 아직 느슨한 결합을 완벽하게 구현한 것이 아니다. 이번 포스팅에서는 느슨한 결합을 구현하기 위한 두번째 단계로 DIP 원칙을 어떻게 구현할 수 있는지 알아보겠다.

![get-loosely-coupled-class](/images/spring-dip-1.png)

&nbsp;
## DIP(Dependency Inversion Principle)이란?
DIP를 해석하면 의존관계의 역전이다. `Robert Martin`에 의해서 만들어진 객체지향 원칙으로 그 내용은 다음과 같다.

1. 상위(High-level) 모듈은 하위(Low-level) 모듈에 의존하면 안된다. 두 모듈은 모두 **추상적 개념(abstraction)**에 의존해야 한다.
2. 세부구현(details)은 추상적 개념(abstraction)에 의존해야 한다.

DIP를 더 명확하게 이해하기 위해 이전 포스팅에서 사용한 예제를 다시 살펴보자.

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

위의 예제는 Factory Pattern을 구현하여 IoC 원칙을 달성하였다. 하지만 `CustomerBusinessLogic`은 `DataAccess`의 **구상 클래스(concrete class)**를 이용하기 때문에, 여전히 강한 결합 상태다.

> 구상 클래스(concrete class)는 객체를 생성할 수 있는 클래스를 의미한다. 즉, 인터페이스나 추상 클래스가 아닌 클래스를 의미한다.

그렇다면 위의 코드가 DIP 관점에서 어떤 문제가 있는지 살펴보자.

***상위 모듈은 하위 모듈에 의존하면 안 된다. 두 모듈은 모두 추상적 개념에 의존 해야 한다.***

위 예제에서 어떤 클래스가 상위 모듈이고, 어떤 클래스가 하위 모듈일까? 상위 모듈은 다른 모듈에 의존하는 모듈이다. 위 예제에서 `CustomerBusinessLogic`은 Customer 데이터를 얻기 위해 `DataAccess`에 의존한다. 따라서 `CustomerBusinessLogic`은 상위 모듈이고, `DataAccess`는 하위 모듈이다. 위 예제에서 `CustomerBusinessLogic`(상위 모듈)은 `DataAccess`(하위 모듈)에 의존하고 있기 때문에, 강하게 결합 된 클래스다.

***세부구현(details)은 추상적 개념(abstraction)에 의존해야 한다.*** 

추상적 개념과 캡슐화는 객체지향 프로그래밍에서 매우 중요한 원칙이다. 추상적 개념은 **non-concrete 클래스**로, 프로그래밍 관점에서 객체를 생성 할 수 없는 클래스를 말한다. `CustomerBusinessLogic`과 `DataAccess`는 객체를 생성 할 수 있기 때문에 **concrete 클래스**다. DIP 원칙에 의해 `CustomerBusinessLogic`과 `DataAccess`은 모두 추상적 개념에 의존 해야 한다. 쉽게 말해 두 클래스는 모두 인터페이스나 추상 클래스에 의존 해야 한다.

DIP의 원칙을 지키기 위해 먼저 아래 코드와 같이 인터페이스를 추가한다.

```java
public interface ICustomerDataAccess {

    String getCustomerName(int id);
}
```

그리고 CustomerDataAccess가 `ICustomerDataAccess`를 구현하도록 만든다.

```java
public class CustomerDataAccess implements ICustomerDataAccess {
    
    public CustomerDataAccess() {}

    public String getCustomerName(int id) {
        return "Dummy Customer Name";
    }
}
```

그리고 `DataAccessFactory`가 `ICustomerDataAccess` 객체(non-concrete class)를 반환하도록 코드를 수정한다.

```java
public class DataAccessFactory {

    public static ICustomerDataAccess getCustomerDataAccessObj() {
        return new CustomerDataAccess();
    }
}
```

마지막으로 `CustomerBusinessLogic`이 `DataAccess`(concrete class) 대신 `ICustomerDataAccess` (non-concrete class)를 사용하도록 코드를 수정한다.

```java
public class CustomerBusinessLogic {

    ICustomerBusinessLogic custDataAccess;

    public CustomerBusinessLogic() {
        custDataAccess = DataAccessFactory.getCustomerDataAccessObj();
    }

    public String getCustomerName(int id) {
        return custDataAccess.getCustomerName(id);
    }
}
```

이제 상위 모듈과 하위 모듈이 모두 추상적 개념(인터페이스)에 의존한다. 또한, 세부 구현(CustomerDataAccess)가 추상적 개념(ICustomerDataAccess)에 의존한다.

따라서 위 코드는 DIP를 구현한 것이라 할 수 있다. DIP를 구현하면서 얻을 수 있는 장점은 `CustomerBusinessLogic`과 `CustomerDataAccess`가 느슨하게 결합됐다는 것이다. 즉, 우리는 `CustomerBusinessLogic`의 내용을 변경하지 않아도 `ICustomerDataAccess`의 구현 클래스들을 변경하면서 사용할 수 있다.

하지만 `CustomerBusinessLogic`은 `ICustomerDataAccess`의 레퍼런스를 얻기 위해서 `Factory` 클래스를 이용하고 있기 때문에, 이는 완전히 느슨한 결합이 아니다. 이 문제는 DI(Dependency Injection)의 구현을 통해 해결할 수 있다. DI의 개념과 실제 적용은 다음 포스팅에서 살펴보도록 하겠다.


&nbsp;
## 출처
---
- [Spring Core Framework Tutorials](https://www.journaldev.com/2888/spring-tutorial-spring-core-tutorial)
- 스프링 부트로 배우는 자바 웹 개발 (제이펍)
- [https://www.baeldung.com/](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
