---
title: "[스프링] 스프링 핵심 개념 정리 - DI [3/4]"
layout: post
tags: ['스프링', 'DI', 'Dependency Injection', 'Stategy Pattern']
subtitle: '스프링의 핵심 개념인 의존성 주입(DI: Dependency Injection)을 학습한다.'
---


## 들어가며
---
지난 포스팅에서 느슨한 결합(Loosely coupled) 상태를 구현하기 위해서 DIP 원칙을 적용하였다. 이번 포스팅에서는 의존 객체의 생성을 클래스 외부로 옮기기 위해서 의존성 주입(DI)과 전략 패턴(Stategy Pattern)을 

구현 할 것이다. 이는 느슨한 결합 상태를 만들기 위한 세 번째 단계다.

![getting-loosely-coupled-class](/images/spring-di-and-stategy-pattern-1.png)

## 의존성 주입(DI: Dependency Injection)이란?
---
DI는 IoC를 구현하기 위한 디자인 패턴이다. DI는 의존 객체의 생성을 클래스 외부에서 할 수 있도록 하고, 해당 객체를 클래스에 제공한다. DI를 사용하면 의존 객체의 생성과 바인딩을 의존(Dependent) 클래스의 외부로 옮길 수 있다.

DI는 세 가지 클래스 타입을 포함한다.
1. 클라이언트 클래스(Client Class): 클라이언트 클래스(Dependent class)는 서비스 클래스에 의존하는 클래스다.
2. 서비스 클래스(Service Class): 서비스 클래스는(Dependency) 클라이언트에 서비스를 제공하는 클래스다. 
3. 주입 클래스(Injector Class): 주입 클래스는 서비스 클래스 오브젝트를 클라이언트 클래스에 주입한다.

![dependency injection](/images/spring-di-and-stategy-pattern-2.png)

위 그림은 세 클래스의 관계를 보여준다. Injector는 서비스의 객체를 생성하고, 클라이언트(서비스 객체를 이용)에 생성한 객체를 주입한다. 이처럼 DI는 서비스 객체의 생성을 클라이언트 외부로 옮겨 의존성을 낮춘다.


&nbsp;
## 의존성 주입(DI)의 유형
---
주입 클래스는 클라이언트에 서비스를 주입한다. 그리고 의존성의 주입은 생성자, 속성, 메서드 세 가지 방식을 이용하여 이루어진다.

1. 생성자 주입(Constructor Injection): Injector는 클라이언트의 생성자를 통해 서비스를 제공한다.

2. 속성 주입(Property Injection): 속성 주입(aka the Setter Injection)에서 injector는 클라이언트 클래스의 public 속성을 통해 의존성을 제공한다.

3. 메서드 주입(Method Injection): 클라이언트 클래스는 메서드를 선언하여 의존성을 주입하기 위해 선언한 인터페이스를 구현한다. 그리고 injector는 클라이언트 클래스에 의존성을 제공하기 위해 이 인터페이스를 사용한다.


아래의 코드는 `CustomerBusinessLogic`에서 `CustomerDataAccess`의 객체를 얻기 위해 `Factory`를 사용한 예제다.

```java
public interface ICustomerDataAccess {

    String getCustomerName(int id);
}

public class CustomerDataAccess implements ICustomerDataAccess {

    public CustomerDataAccess() {}

    public String getCustomerName(int id) {
        return "Dummy Customer Name";
    }
}

public class DataAccessFactory {

    public static ICustomerDateAccess getCustomerDataAccessObj() {
        return new CustomerDataAccess();
    }
}

public class CustomerBusinessLogic {

    ICustomerDataAccess dataAccess;

    public CustomerBusinessLogic() {
        dataAccess = DataAccessFactory.getCustomerDataAccessObj();
    }

    public String getCustomerName(int id) {
        return dataAccess.getCustomerName(id);
    }
}
```

위 코드는 `CustomerBusinessLogic` 내에서 `DataAccessFactory`를 사용했다. 이로 인해서 발생할 수 있는 문제는 다음과 같다.
위 예제 코드의 문제점은 CustomerBusinessLogic 클래스 안에서 DataAccessFactory 클래스를 사용했다는 것이다. ICustomerDataAccess 클래스의 또 다른 구현이 있다고 가정하자. 그리고 우리는 CustomerBusinessLogic 안의 새로운 클래스를 사용하기를 원한다고 가정하자. 그러면 우리는 CustomerBusinessLogic 클래스 안에 있는 소스코드를 수정해야한다. DI 패턴은 이 문제를 의존 객체를 생성자, 속성, 인터페이스를 통해 주입해서 해결한다.

다음 그림은 위 예제의 DI 패턴 구현이다.

> Image

위의 그림처럼 CustomerService 클래스는 injector 클래스가 되었다. 서비스 클래스의 객체를 설정하는 클라이언트 클래스 서비스에. 생성자, 속성, 메서드를 통해서. 느슨한 결합을 구현하기 위해. 각각의 옵션을 조금 더 살펴보자.

### Constructor Injection 
---
앞서 언급한 것처럼 의존성을 생성자를 통해 주입할 떄 이를 생성자 주입이라고 한다.

다음 예제를 생성자를 사용한 DI를 이용해 구현했다고 생각해보자.

> ### Example: Constructor Injection
```java
public class CustomerBusinessLogic {

    ICustomerDataAccess dataAccess;

    public CustomerBusinessLogic(ICustomerDataAccess custDataAccess) {
        dataAccess = custDataAccess;
    }

    public CustomerBusinessLogic() {
        dataAccess = new CustomerDataAccess();
    }

    public String ProcessCustomerData(int id) {
        return dataAccess.getCustomerName();
    }
}

public interface ICustomerDataAccess {
    
    String getCustomerData(int id);
}

public class CustomerDataAccess implements ICustomerDataAccess {
    
    public CustomerDataAccess() { }

    public String getCustomerId(int id) {
        return "Dummy Customer Name";
    }
}
```

위 예제에서 CustomerBusinessLogic은 매개변수의 타입으로 ICustomerDataAccess을 가진다. 이를 호출하는 클래스는 반드시 ICustomerDataAccess 객체를 주입해야한다.

> ### Example: Inject Dependency
```java
public class CustomerService {

    CustomerBusinessLogic customerBL;

    public customerService() {
        customerBL = new CustomerBusinessLogic(new CustomerDataAccess());
    }

    public String getCustomerName(int id) {
        return customerBL.getCustomerName(id);
    }
}
```

위 예제에서 볼 수 있는 것처럼, `CustomerService` 클래스는 CustomerDataAccess 생성하고 CustomerBusinessLogic 클래스에 주입한다. 따라서, `CustomerBusinessLogic` 클래스는 CustomerDataAccess 객체를 만들 필요가 없다. `CustomerServiceClass`는 적절한 DataAccess 클래스를 CustomerBusinessLogic 클래스에 생성하고 설정한다. 이러한 방식으로 `CustomerBusinessLogic`과 `CustomerDataAccess` 클래스는 더욱 느슨하게 결합된 클래스가 되었다.


### Property Injection


&nbsp;
## 참고
- [https://www.baeldung.com/](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
