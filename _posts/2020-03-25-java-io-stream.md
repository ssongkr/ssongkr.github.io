---
title: "[Java] 자바의 입출력 스트림 이해하기"
layout: post
---

---
## 스트림이란
자바는 스트림을 이용해서 데이터를 주고받는다. 스트림은 데이터를 출발지점에서 도착지점까지 데이터를 이동시키는 일종의 통로이다. 이 때 데이터의 출발지점, 도착지점을 노드라고 한다. 노드의 종류에는 키보드, 모니터, 메모리, 데이터베이스 등이 있다. 그리고 이 노드에 연결된 스트림을 노드 스트림이라고 한다.

스트림은 단방향으로 데이터를 송수신한다. 그래서 데이터를 받을 때는 입력 스트림을 사용하고, 데이터를 보낼 때는 출력 스트림을 사용한다.

---
## 스트림의 종류

### 1. 바이트 스트림
스트림을 통해서 전송되는 데이터의 단위는 기본적으로 **바이트(byte)**이다. 입력 바이트 스트림의 이름 형식은 **ExampleInputStream**이고, 출력 바이트 스트림의 이름 형식은 **ExampleOutputStream이다.**

### FileInputStream & FileOutputStream
FileInputStream은 파일에서 바이트를 읽어 바이트 스트림으로 변환한다.

FileOutputStream은 바이트 스트림을 바이트 파일로 변환한다.

### ObjectInputStream & ObjectOutputStream
ObjectInputStream과 ObjectOutputStream은 객체를 파일에 읽고 쓸 수 있도록 해주는 바이트 스트림이다. 예를들어 Date 객체를 파일에 저장한다면, 프로그램을 실행 했을 때 해당 객체를 파일에서 읽어서 재사용할 수 있다.

### 직렬화(Serialization)
우리가 객체를 파일에 쓰는 것은 객체를 스트림으로 변환하는 것과 동일하다. 그리고 이것이 바로 **객체 직렬화**다. 객체를 파일에 쓰기 위해서는 해당 객체에 Serializable 인터페이스를 구현해야한다.

---
### 2. 문자 스트림
바이트(byte)는 바이트 단위로 연결하고, 문자(Character)는 문자 단위로 연결하는 것이 원칙이다. 문자는 결국 바이트 두 개가 모여 구성된 데이터라고 할 수 있다. 따라서 두 가지 서로 다른 기준을 호환할 수 있는 방법이 제공되어야 하는데, 이 역할을 담당하는 것이 문자 스트림이다. 문자 스트림은 **ExampleReader** / **ExampleWriter** 형식의 이름을 가지고 있다.

### InputStreamReader & OutputStreamWriter
InputStreamReader는 바이트 스트림을 문자 스트림으로 변환하는 기능을 제공하고, OutputStreamReader는 문자 스트림을 바이트 스트림으로 변환하는 기능을 제공한다. 바이트를 읽어서 지정된 문자 인코딩에 맞는 문자로 변환한다.

### BufferedReader & BufferedWriter
문자의 입출력을 버퍼링하지 않고, 문자 하나하나씩 입출력하면 전송 시간이 오래걸려 비효율적이다. BufferedReader와 BufferedWriter는 문자 스트림을 버퍼링하여 효율적으로 입출력을 처리할 수 있도록 돕는다. 

---
## 출처
[https://postitforhooney.tistory.com/](https://postitforhooney.tistory.com/entry/Java-Java-Stream%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%A2%85%EB%A5%98-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%98%88%EC%A0%9C%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

[https://coding-factory.tistory.com/251](https://coding-factory.tistory.com/251)

[https://hyeonstorage.tistory.com/247](https://hyeonstorage.tistory.com/247)

[http://tcpschool.com/](http://tcpschool.com/cpp/cpp_io_streamBuffer)