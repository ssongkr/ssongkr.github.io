---
title: "[스프링] 스프링 핵심 개념 정리 - DI [3/4]"
layout: post
tags: ['스프링', 'DI', 'Strategy Pattern']
subtitle: '스프링의 핵심 개념인 의존성 주입(DI: Dependency Injection)을 학습한다.'
---


## 들어가며
---
지난 포스팅에서 느슨한 결합(Loosely coupled) 상태를 구현하기 위해서 DIP 원칙을 적용하였다. 이번 포스팅에서는 의존 객체의 생성을 클래스 외부로 옮기기 위해서 의존성 주입(DI)과 전략 패턴(Strategy Pattern)을 구현 할 것이다. 이는 느슨한 결합 상태를 만들기 위한 세 번째 단계다.

![getting-loosely-coupled-class](/images/spring-di-and-strategy-pattern-1.png)


## 의존성 주입(DI: Dependency Injection)이란?
---
DI는 IoC를 구현하기 위한 디자인 패턴이다. DI는 의존 객체의 생성을 클래스 외부에서 할 수 있도록 하고, 해당 객체를 클래스에 제공한다. DI를 사용하면 의존 객체의 생성과 바인딩을 의존(Dependent) 클래스의 외부로 옮길 수 있다.

DI는 세 가지 클래스 타입을 포함한다.
1. 클라이언트 클래스(Client Class): 클라이언트 클래스(Dependent class)는 서비스 클래스에 의존하는 클래스다.
2. 서비스 클래스(Service Class): 서비스 클래스는(Dependency) 클라이언트에 서비스를 제공하는 클래스다. 
3. 주입 클래스(Injector Class): 주입 클래스는 서비스 클래스 오브젝트를 클라이언트 클래스에 주입한다.

![dependency injection](/images/spring-di-and-strategy-pattern-2.png)

위 그림은 세 클래스의 관계를 보여준다. Injector는 서비스의 객체를 생성하고, 클라이언트(서비스 객체를 이용)에 생성한 객체를 주입한다. 이처럼 DI는 서비스 객체의 생성을 클라이언트 외부로 옮겨 의존성을 낮춘다.


&nbsp;
## 의존성 주입(DI)의 유형
---
주입 클래스는 클라이언트에 서비스를 주입한다. 그리고 의존성의 주입은 생성자, 속성, 메서드 세 가지 방식을 이용하여 이루어진다.

**1. 생성자 주입(Constructor Injection)**: Injector는 클라이언트의 생성자를 통해 서비스를 제공한다.

**2. 속성 주입(Property Injection)**: 속성 주입(aka the Setter Injection)에서 injector는 클라이언트 클래스의 public 속성을 통해 의존성을 제공한다.

**3. 메서드 주입(Method Injection)**: 클라이언트 클래스는 메서드를 선언하여 의존성을 주입하기 위해 선언한 인터페이스를 구현한다. 그리고 injector는 클라이언트 클래스에 의존성을 제공하기 위해 이 인터페이스를 사용한다.


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

위 코드는 `CustomerBusinessLogic` 내에서 `DataAccessFactory`를 사용했다. 이로 인해서 발생할 수 있는 문제는 다음과 같다. `ICustomerDataAccess` 인터페이스를 구현한 새로운 클래스가 있고, `CustomerBusinessLogic`에서 새로운 클래스를 사용하려고 한다고 가정해보자. 그러면 `CustomerBusinessLogic`의 코드를 수정해야한다. DI를 구현하면 이 문제를 해결할 수 있다.


&nbsp;
### 1. 생성자 주입(Constructor Injection)
---
의존성을 생성자를 통해 주입하는 것을 생성자 주입이라고 한다. 다음 예제는 생성자를 이용해 DI를 구현한 예제다.

```java
public class CustomerBusinessLogic {

    ICustomerDataAccess dataAccess;

    public CustomerBusinessLogic(ICustomerDataAccess custDataAccess) {
        dataAccess = custDataAccess;
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

위 예제에서 `CustomerBusinessLogic`의 생성자는 `ICustomerDataAccess`를 매개변수로 가진다. 따라서 `CustomerBusinessLogic`을 생성하기 위해서는 반드시 `ICustomerDataAccess` 객체를 주입 해야 한다.

아래의 예제는 `CustomerService`를 통해 `CustomerDataAccess` 객체를 생성하여 `CustomerBusinessLogic`에 주입한다. 따라서 `CustomerBusinessLogic`은 `CustomerDataAccess`의 객체를 더 이상 만들지 않는다. 대신 `CustomerService`가 적절한 `ICustomerDataAccess`를 구현한 클래스를 생성하고 주입한다. 결과적으로 `CustomerBusinessLogic`과 `CustomerDataAccess`는 더욱 느슨하게 결합 된 클래스가 됐다.

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

&nbsp;
### 2. 속성 주입(Property Injection)
---
속성 주입은 의존성을 `public` 속성을 이용하여 주입한다. 아래의 예제를 살펴보자. 

```java
public class CustomerBusinessLogic {

    public ICustomerDataAccess dataAccess;
    
    public CustomerBusinessLogic() {}

    public String getCustomerName(int id) {
        return dataAccess.getCustomerName(id);
    }
}

public class CustomerService {

    CustomerBusinessLogic customerBL;

    public CustomerService() {
        customerBL = new CustomerBusinessLogic();
        customerBL.dataAccess = new CustomerDataAccess();
    }

    public String getCustomerName(int id) {
        return customerBL.getCustomerName(id);
    }
}

```
위 코드에서 볼 수 있는 것처럼 `CustomerBusinessLogic`은 public 속성 `DataAccess`를 포함한다. 그리고 속성을 통해 `ICustomerDataAccess`의 구현 클래스를 인스턴스로 설정 할 수 있다.


&nbsp;
### 3. 메서드 주입(Method Injection)
메서드 주입은 의존성을 메서드를 이용하여 주입한다. 메서드는 클래스 메서드일 수도 있고, 인터페이스 메서드일 수도 있다.

아래의 코드는 인터페이스 메서드를 이용하여 메서드 주입을 구현한 것이다.

```java
interface IDataAccessDependency {

    void setDependency(ICustomerDataAccess customerDataAccess);
}

public class CustomerBusinessLogic implements IDataAccessDependency {

    ICustomerDataAccess dataAccess;

    public CustomerBusinessLogic() {}

    public String getCustomerName(int id) {
        return dataAccess.getCustomerName(id);
    }

    @Override
    public void setDependency(ICustomerDataAccess customerDataAccess) {
        dataAccess = customerDataAccess;
    }
}

public class CustomerService {

    CustomerBusinessLogic customerBL;

    public CustomerService {
        customerBL = new CustomerBusinessLogic();
        customerBL.setDependency(new CustomerDataAccess());
    }

    public String getCustomerName(int id) {
        return customerBL.getCustomerName(id);
    }
}
```

위 예제에서 `CustomerBusinessLogic`은 `IDataAccessDependency` 인터페이스를 구현한다. 따라서 `Injector(CustomerService)`는 클라이언트 클래스에 `CustomerDataAccess`(dependent class)를 `setDependency()` 메서드를 이용하여 주입할 것이다. 

&nbsp;
## 마무리하며
---
지금까지 느슨하게 결합 된 클래스를 구현하기 위해 몇 가지 원칙과 패턴을 사용했다. 좀 더 전문적인 프로젝트에는 많은 종속 클래스가 있고, 이러한 패턴을 구현하는데 많은 시간이 걸린다. IoC 컨테이너는 이 문제를 해결한다. 다음 포스팅에서는 IoC 컨테이너에 대해서 알아보도록 하겠다.


&nbsp;
## 참고
- [https://www.baeldung.com/](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
