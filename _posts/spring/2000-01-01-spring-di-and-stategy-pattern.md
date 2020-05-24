---
title: "[스프링] 스프링 핵심 개념 정리 - DI [3/4]"
layout: post
tags: '스프링'
subtitle: '스프링의 핵심 개념인 제어의 역전(IoC), 의존 관계 역전 원칙(DIP), 의존성 주입(DI)을 학습한다.'
---


## 들어가며
---
지난 포스팅에서 DIP와 관련하여 추상화를 클래스를 만들어서 슨하게 결합된 클래스들을 만들었다. 이번 포스팅에서는 의존 객체의 생성을 클래스 외부로 옮기기 위해서 의존성 주입(DI)과 전략 패턴(Stategy pattern)을 함께 구현할 것이다. 이것은 느슨한 결합 상태를 구현하기 위한 세번째 단계이다.

>> Image

## 의존성 주입(DI: Dependency Injection)
---
DI는 IoC를 구현하기 위한 디자인 패턴이다. DI는 의존객체의 생성을 클래스 외부에서 할 수 있도록 하고, 해당 객체를 클래스에 제공한다. DI를 사용하면 의존 객체의 생성과 바인딩을 의존 객체에 의존하는 클래스의 외부로 옮길 수 있다.

DI 패턴은 3가지 클래스 타입을 포함한다.
1. 클라이언트 클래스(Client Class): 클라이언트 클래스(Dependent class)는 서비스 클래스에 의존하는 클래스다.
2. 서비스 클래스(Service Class): 서비스 클래스는(Dependency) 클라이언트에 서비스를 제공하는 클래스다. 
3. 주입 클래스(Injector Class): 주입 클래스는 서비스 클래스 오브젝트를 클라이언트 클래스에 주입한다.

다음 그림은 클래스 간의 관계를 보여준다.

> Image

위 그림에서 볼 수 있는 것처럼 주입 클래스는 서비스 클래스의 객체를 생성하고, 생성된 클래스의 해당 객체를 클라이언트 객체에 주입한다. 이러한 방식으로 DI 패턴은 클라이언트 클래스가 서비스 클래스의 객체를 생성하는 책임을 클라이언트 클래스 밖으로 분산시킨다. 


&nbsp;
## 의존성 주입(DI)의 유형
---
위에서 확인한 것처럼 주입 클래스는 클라이언트(Dependent)에 서비스(Dependency)를 주입한다. 주입 클래스는 생성자, 속성, 메서드를 통해 세 가지 방식으로 의존성을 주입한다. 

1. 생성자 주입(Constructor Injection): Injector는 클라이언트 클래스의 생성자를 통해 서비스를 제공한다.
2. 속성 주입(Property Injection): 속성 주입(aka the Setter Injection)에서 injector는 클라이언트 클래스의 public 속성을 통해 의존성을 제공한다.
3. 메서드 주입(Method Injection): 클라이언트 클래스는 메서드를 선언하여 의존성을 주입하기 위해 선언한 인터페이스를 구현한다. 그리고 injector는 클라이언트 클래스에 의존성을 제공하기 위해 이 인터페이스를 사용한다.

이전 포스팅에서 사용한 예제를 그대로 사용해보겠다. 이전 포스팅 DIP에서 CustomerBusinessLogic클래스 안에 Factory 클래스를 사용했다. CustomerDataAccess 클래스의 객체를 얻기 위해서. 아래와 같이.

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

    ICustomerDataAccess custDataAccess;

    public CustomerBusinessLogic() {
        custDataAccess = DataAccessFactory.getCustomerDataAccessObj();
    }

    public String getCustomerName(int id) {
        retur ncustDataAccess.getCustomerName(id);
    }
}
```

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
https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
