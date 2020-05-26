---
title: "[스프링] 스프링 핵심 개념 정리 - IoC [1/4]"
layout: post
tags: '스프링'
subtitle: '스프링의 핵심 개념인 제어의 역전(IoC: Inversion of Control)을 학습한다.'
---


## 들어가며
---
스프링 프레임워크를 공부하고 있는 학생이라면 IoC, DIP, DI, IoC-Container와 같은 단어를 많이 접해봤을 것이다. 이들을 우리말로 풀이하면 각각 제어의 역전, 의존관계 역전의 원칙, 의존성 주입, IoC 컨테이너이다. 단어의 의미가 매우 추상적이기 때문에 단순 해석만 봐서는 개념을 이해하기 어렵다. 따라서 이번 포스팅을 시작으로 위의 개념들을 정리하고, 코드로 구현해보고자 한다. 최종 목표는 위 개념들을 적용한 코드를 작성하여 강한 연결(tightly coupled) 상태를 가진 앱을 느슨한 연결(loosely coupled) 상태를 가진 앱으로 만드는 것이다. 이번 포스팅에서는 IoC 원칙을 알아보고, 이를 코드에 적용해보도록 하겠다.

![get-loosely-coupled-class](/images/spring-ioc-1.png)

&nbsp;
## 디자인 원칙과 디자인 패턴
---
앞서 언급한 개념들을 이해하기 위해서는 **디자인 원칙(design principle)**과 **디자인 패턴(design pattern)**의 차이를 이해해야한다. 각 용어의 의미는 다음과 같다.

#### 디자인 원칙(Design Principle)
- 높은 품질의 소프트웨어를 개발하기 위한 고수준(high-level)의 `지침(guideline)`이다.
- 단순 지침이므로 구체적인 구현 방식을 명시하지 않는다. 그리고 프로그래밍 언어에 종속되지 않는다.
- 대표적인 예로 SOLID(SRP, OCP, LSP, ISP, DIP)가 있다.

#### 설계 패턴(Design Pattern)
- 객체 지향 프로그래밍에서 발생하는 문제를 해결하기 위한 저수준(low-level)의 `구현 방식(implementation)`을 제안한다.
- 대표적인 예로 싱글톤(singleton) 패턴이 있다.

앞서 언급했던 IoC와 DIP는 디자인 원칙이고, DI는 디자인 패턴이다. 이에 유념하며 포스팅을 읽어보기 바란다.


&nbsp;
## 제어의 역전(IoC: Inversion of Controll)
---
IoC는 **디자인 원칙**이다. 이름에서 알 수 있는 것처럼 IoC는 프로그램의 실행 흐름이나 종속 객체 생성의 제어권을 위임하여(invert), 객체 지향 프로그래밍에서 느슨한 결합(loose coupling)을 가능하게 한다.

IoC를 쉽게 이해하기 위해 운전을 예로 들어보겠다. 만약 회사에 출근하기 위해 자동차를 직접 운전한다면, 자동차의 제어권은 당신에게 있다. 그런데 운전기사를 고용해 운전을 맡긴다면 자동차의 제어권은 운전기사에게 넘어간다. 이것이 제어의 역전(IoC)이다.

그렇다면 IoC 원칙을 준수하여 프로그래밍을 하면 어떤 이점이 있는 것일까? 앞서 언급했듯이 IoC는 클래스 간의 느슨한 결합(loose scoupling)을 가능하게 하여, 서로의 의존성을 줄여준다. 이는 장기적으로 프로그램을 더 쉽게 테스트 할 수 있게 하고, 유지보수하기 쉽게 해주며, 높은 확장성을 갖게 해준다.

&nbsp;
### 프로그램의 실행 흐름 제어
일반적으로 콘솔 어플리케이션은 메인 함수에서 실행된다. 따라서 메인 함수가 프로그램의 실행 흐름을 제어한다. 쉽게 말해 프로그램과 사용자가 상호작용 하는 일련의 순서를 결정한다. 

좀 더 자세히 이해하기 위해 아래의 자바 콘솔 프로그램을 예로 들어보자.

```java
public class Program {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        boolean continueExecution = true;

        while (continueExecution) {
            System.out.println("Enter First Name: ");
            String firstName = sc.nextLine();

            System.out.println("Enget Last Name: ");
            String lastName = sc.nextInt();

            System.out.println("Do you want to save it? Y/N: ");
            String wantToSave = sc.nextLine();
            if(wantToSave.equals("Y")) {
                saveToDB(firstName, lastName);
            }

            System.out.println("Do you want to Exit? Y/N: ");
            String wantToExit = sc.nextLine();
            if(wantToExit.equals("Y")) {
                continueExecution = false;
            }
        }
    }

    public static void saveToDB(String firstName, String lastName) {
        // save first name and last name to database here...
    }
}
```

위 코드에서 `Program` 클래스의 `main()` 함수는 사용자의 이름, 데이터의 저장 여부, 프로그램의 종료 여부를 순서대로 입력받는다. 따라서 위 프로그램의 실행 흐름은 `main()` 함수에 의해 제어된다고 할 수 있다.

위의 콘솔 프로그램을 GUI 기반의 앱으로 만들면 IoC가 쉽게 구현된다. 윈도우 앱 개발을 위한 프레임워크는 `이벤트` 사용하여 프로그램의 흐름을 제어한다. 즉, 프로그램의 제어권이 `main()` 함수에서 `프레임워크`로 역전된 것이다.

![window_application](/images/spring-ioc-2.png)


&nbsp;
### 의존 객체 생성 제어
의존 객체를 생성 할 때도 IoC가 적용될 수 있다. 이를 설명하기에 앞서 의존성(Dependency)이 무엇인지 다음 예제 코드를 통해 살펴보자.

```java
public class A {

    B b;

    public A() {
        b = new B();
    }

    public void task() {
        // do something..
        b.someMethod();
        // do something..
    }
}

public class B {

    public void someMethod() {
        // do something..
    }
}
```

위 코드에서 A-클래스는 `task()` 메서드의 작업을 완료하기 위해서 `b.someMethod()`를 호출한다. 다시말해 A-클래스는 B-클래스의 도움 없이 `task()` 메서드의 작업을 완료할 수 없다. 이를 다른 말로 표현하면 *"A-클래스는 B-클래스에 의존적이다"* 혹은 *"B-클래스는 A-클래스의 의존성(Dependency)이다"*라고 할 수 있다.

객체 지향 디자인 접근 방식에서 클래스들은 기능을 완성하기 위해 서로 상호작용(interact) 해야한다. 위 예제에서 A-클래스는 B-클래스(dependency class)의 객체를 생성하고 생명주기를 관리한다.

IoC 원칙은 의존 객체 생성의 제어권을 다른 클래스로 위임할 것을 제안한다. 즉, 의존 객체 생성의 제어권을 A-클래스에서 다른 클래스로 넘기는(invert) 것이다.

```java
public class A {

    B b;

    public A() {
        b = Factory.getObjectOfB();
    }

    public void task() {
        // do something..
        b.someMethod();
        // do something..
    }
}

public class Factory {

    public static B getObjectOfB() {
        return new B();
    }
}
```

위 코드에서 A-클래스는 B-클래스의 객체를 얻기 위해 Factory-클래스를 사용한다. 따라서 A-클래스는 B-클래스의 객체를 더 이상 생성하지 않고, Factory-클래스를 통해 B-클래스의 객체를 얻는다. 이는 의존 객체 생성의 제어를 A-클래스에서 Factory-클래스로 넘긴 것이다. 

객체 지향 디자인에서는 클래스들이 느슨하게 결합(loosely coupled)되도록 설계 해야 한다. 클래스가 느슨하게 결합되어 있으면 한 클래스의 변화가 다른 클래스에 영향을 주지 않는다. 따라서 느슨한 결합은 앱의 유지 보수성을 높여준다.

느슨한 결합을 n-tier 아키텍처를 통해 조금 더 구체적으로 구체적으로 살펴보자.

![n-tier-architecture](/images/spring-ioc-3.png)

일반적으로 n-tier 아키텍처에서 UI는 데이터를 주고 받기 위해서 서비스 계층과 상호작용 한다. 서비스 계층은 `BusinessLogic` 클래스와 상호작용 하여 데이터에 대한 비즈니스 로직을 처리한다. `BusinessLogic` 클래스는 데이터베이스와 상호작용하는 `DataAccess` 클래스에 의존한다. 이것이 가장 단순한 n-tier 아키텍처이다. 

IoC를 이해하기 위해 `BusinessLogic`과 `DataAccess`에 초점을 맞춰보자.

```java
public class CustomerBusinessLogic {
    
    DataAccess dataAccess;

    public CustomerBusinessLogic() {
        dataAccess = new DataAccess();
    }

    public String getCustomerName(int id) {
        return dataAccess.getCustomerName(id);
    }
}

public class DataAccess {

    public DataAccess() { }

    public String getCustomerName(int id) {
        return "Dummy customer Name" // get it from DB in real app;
    }
}
```

위 예제에서 `CustomerBusinessLogic`은 `DataAccess`에 의존하여 데이터를 읽어온다. 그리고 `DataAccess`를 직접 생성하고 생명주기를 관리하기 때문에, `CustomerBusinessLogic`과 `DataAccess`는 강하게 연결(tightly coupled)된 클래스다.

강한 연결로 발생할 수 있는 문제점은 다음과 같다.

1. `DataAccess`의 변화가 `CustomerBusinessLogic`의 변화로 이어진다. `DataAccess`의 메서드 이름을 변경하거나 추가, 삭제하면 `CustomerBusinessLogic`의 내용도 함께 수정해야한다.

2. 추후에 데이터를 다른 데이터베이스나 웹 서비스를 통해 가져와야 한다면 데이터 접근을 위한 새로운 클래스를 만들어야 한다. 결과적으로 `CustomerBusinessLogic`의 내용도 함께 수정해야한다.

3. `CustomerBusinessLogic`은 *new* 키워드를 사용하여 `DataAccess`의 객체를 생성한다. `DataAccess`의 객체를 생성하는 클래스가 많다면, `DataAccess`의 이름이 변경됐을때 `DataAccess`의 객체를 생성한 모든 클래스를 찾아 코드를 수정해야 할 것이다.

4. `CustomerBusinessLogic`은 `DataAccess`의 객체를 생성한다. `DataAccess`를 Mock 클래스로 대체할 수 없기 때문에, 코드를 독립적으로 테스트 할 수 없다. 

Factory Pattern을 사용하면 IoC 원칙을 구현할 수 있고, 위의 문제를 해결 할 수 있다. 아래의 코드는 Factory Pattern을 구현한 예제다.

```java
public class DataAccessFactory {

    public static DataAccess getDataAccessObj() {
        return new DataAccess();
    }
}

public class CustomerBusinessLogic() {

    public CustomerBusinessLogic() {}

    public String getCustomerName(int id) {
        DataAccess dataAccess = DataAccessFactory.getDataAccessObj();
        return dataAccess.getCustomerName(id);
    }
}
```

이제 `CustomerBusinessLogic`은 `DataAccessFactory`의 `getCustomerDataAccessObj()` 메서드를 사용하여 `DataAccess` 클래스를 얻는다. 이는 의존 클래스의 객체 생성 제어를 `CustomerBusinessLogic`에서 `DataAccessFactory`로 넘긴 것이다.


&nbsp;
## 마무리하며
---
IoC의 원칙의 적용은 느슨한 결합을 구현하기 위한 첫 번째 단계다. 더욱 완전한 느슨한 결합 상태를 구현하려면 DIP, Stategy pattern, DI를 함께 사용해야한다.

다음 포스팅에서는 DIP 원칙을 적용하여 느슨한 결합을 한 단계 발전시켜보도록 하겠다.


&nbsp;
## 참고
---
- [https://www.baeldung.com/](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
- [https://www.tutorialsteacher.com/](https://www.tutorialsteacher.com/ioc/inversion-of-control)
