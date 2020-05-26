---
title: "[Java] 싱글톤 패턴 이해하기"
layout: post
---

---
## 싱글톤 패턴이란?
싱글톤 패턴이란 특정 클래스의 인스턴스를 단 하나만 만들 수 있도록 클래스를 정의하고, 해당 인스턴스에만 접근할 수 있도록 하기 위한 디자인 패턴이다.

스레드 풀, 캐시, 사용자 설정 등의 객체는 하나만 있어도 정상적으로 프로그램을 돌릴 수 있다. (혹은 하나만 있어야 한다.) 이런 객체의 인스턴스를 여러개 만들 경우 메모리의 낭비를 초래하거나 에러를 야기할 수 있다.  이러한 문제들을 해결하기 위한 디자인 패턴이 싱글톤 패턴이다.

---
## 싱글톤 패턴 구현

### 1. Lazy instantiation
```java
class Singleton {

    private static Singleton instance // 2
    
    private Singleton() {}; // 1
    
    public static Singleton getInstance() { // 3
        if(uniqueInstance == null) { instance = new Singleton(); }
        return instance;
    }
}

public class SingletonTest {
    public static void main(String[] args) {
        Singleton s = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();
        System.out.println(s == s2); // true
    }
}
```

1. Default 생성자의 접근 지정자를 private로 선언하여 외부에서 추가적인 객체를 생성하지 못하도록 한다.
2. 외부에서 싱글톤 객체를 호출할 수 없도록 접근 지정자를 private로 지정하고, 하나의 싱글톤 객체를 공유하기 위해 정적 변수(static variable)로 선언한다.
3. 외부에서 싱글톤 객체에 접근할 수 있도록 getInstance() 메소드를 정의한다. 그리고 인스턴스를 미리 생성하지 않고 필요한 상황일 때 인스턴스를 생성하는데 이를 **게으른 인스턴스 생성(Lazy instantiation) 기법**이라고 한다. 이 기법은 객체가 자원을 많이 잡아먹는 경우에 유용하게 사용할 수 있다.

하지만 이 구현법을 이용하면 멀티 스레드 프로그램에서는 인스턴스가 여러개 생성될 수 있다는 문제가 있다.

---
### 2. Thread safe + Lazy instantiation
```java
public class Singleton {

    private static Singleton instance;

    private Singleton () {}
        
    // synchronized keyword is added
    public static synchronized Singleton getInstance() {
        if (instance == null) { instance = new ExampleClass();}
        return instance;
    }
}
```

getInstance() 메소드에 synchronized 키워드를 붙였다. 따라서 한 스레드가 메서드의 사용을 끝내기 전까지 다른 스레드는 getInstance() 메서드에 접근할 수 없고, 인스턴스가 두 개 이상 생성되는 문제는 발생하지 않는다.

하지만 이 방법(synchronized)을 이용하면 애플리케이션의 속도가 급격하게 저하되는 문제가 발생한다.

---
### 3. 정적 초기화 시 인스턴스 생성
```java
public class Singleton{
    // 정적 초기화 시 인스턴스 생성
    private static Singleton instance = new Singleton();
    
    private Singleton () {}
    
    public static synchronized Singleton getInstance(){
        return instance;
    }	
    
}
```
이 구현법을 사용하면 클래스가 로딩될 때 JVM가 유일한 인스턴스를 생성한다. 클래스 로드 외에는 인스턴스를 생성할 수 없기 떄문에 인스턴스가 두 개 이상 생성되는 문제는 해결된다. 하지만 인스턴스를 사용하지 않음에도 불구하고, 객체를 생성하기 때문에 만족스럽지 않은 코드다.

---
### 4. Initialization on demand holder idiom
```java
public class Singleton {

    private Singleton() {}

    private static class LazyHolder {
        static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```
JVM에 내장 된 제약 조건으로 클래스 및 객체를 초기화하여 싱글톤을 구현하는 아주 귀여운 방식이다. 내부에 정적으로 선언된 클래스 LazyHolder는 JVM이 LazyHolder를 실행해야한다고 결정하기 전까지 초기화되지 않는다. 

LazyHolder 클래스는 Singleton 클래스의 getInstance() 메서드가 호출될 때에만 실행되고, 이 메서드가 처음 호출되면 JVM은 LazyHolder 클래스를 로드하고 초기화 할 것이다. 그리고 클래스 초기화 단계는 JLS(Java Language Specification)에 의해서 직렬 실행(비동시성)이 보장되므로, 추가적인 동기화를 필요로하지 않는다.

현재 자바에서 싱글톤을 생성시킬 때 가장 많이 사용하는 방법이다.

---
## References

[https://elfinlas.github.io/](https://elfinlas.github.io/)

[http://friday.fun25.co.kr/blog/?p=312](http://friday.fun25.co.kr/blog/?p=312)

[https://medium.com/@sinethneranjana/](https://medium.com/@sinethneranjana/5-ways-to-write-a-singleton-and-why-you-shouldnt-1cf078562376)