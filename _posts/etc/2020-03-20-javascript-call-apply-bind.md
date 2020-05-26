---
title: "[Javascript] Call, Apply, Bind 메소드"
layout: post
---


---
## call, apply method
자바스크립트에서 함수는 선언한 후에 호출해야 실행된다. 자바스크립트도 다른 프로그래밍 언어와 마찬가지로 함수 이름 뒤에 ()를 붙여서 함수를 호출 할 수 있다. 그리고 call, apply 메소드를 이용하여 함수를 호출할 수도 있다. 다음은 자바스크립트에서 함수를 호출하는 실제 코드이다.

```javascript
var add = function (a, b, c) {
  return a + b + c;
}

console.log(add(1, 2, 3)); // 3
console.log(add.call(null, 1, 2, 3)); // 3
console.log(add.apply(null, [1, 2, 3])); // 3
```

위의 3가지 함수 실행 결과는 모두 동일하고, 실제로 모두 동일한 함수 호출이다. call 메소드는 보통 함수처럼 인자를 넣는다. 그리고 apply 메소드는 인자를 배열 하나로 묶어 전달하는 것을 확인할 수 있다.

그렇다면 call과 apply 메소드가 공통적으로 가지고 있는 null 인자의 역할은 무엇일까?

call과 apply의 첫 번째 인자는 함수를 호출한 객체 즉, this를 대체한다. 다음 코드를 살펴보자.

```javascript
var kim = {
  name: 'kim',
  greeting: function() {
    console.log("Hi! I'm " + this.name);
  }
}

var song = {
  name: 'song'
}

kim.greeting(); // Hi! I'm kim
kim.greeting.call(song) // Hi! I'm song
```

kim.greeting() 메소드를 실행하면 메소드 내에 this.name의 this는 자신을 호출한 객체인 kim을 가리킨다. 따라서 Hi! I'm kim이라는 문장이 콘솔에 출력되는 것을 확인할 수 있다.

그런데 kim.greeting.call(song)은 자신을 호출한 객체를 song으로 대체한다. 따라서 this.name의 this는 song을 가리키게 되고, 콘솔에 출력되는 결과는 Hi! I'm song이 된다.

브라우저 환경에서 this는 기본적으로 window를 가리킨다. call, apply, bind에서 첫 번째 인자로 객체를 넣으면 this를 대체할 수 있다.


---
## arguments and call, apply method
자바스크립트의 함수는 arguments라는 숨겨진 속성을 가지고 있다. 그리고 arguments 속성은 함수에 들어온 인자를 배열과 유사한 형식으로 반환한다. 하지만 이는 실제 배열과는 다르다. 따라서 배열의 메소드를 사용할 수 없다.

```javascript
function argTest1() {
  console.log(arguments);
}
argTest1(1, 'string'); // [1, 'string']

function argTest2() {
  console.log(arguments.join());
}
argTest2(1, 'string'); // Uncaught TypeError: arguments.join is not a function

function argTest3() {
  console.log(Array.prototype.join.call(arguments));
}
argTest3(1, 'string'); // 1, string
```

그렇다면 arguments에 배열의 join 함수를 사용하려면 어떻게 해야할까? 이때 call, apply 메소드를 이용하여 배열의 프로토타입에 있는 join 함수를 사용하면 된다.

## bind method
bind 메소드도 앞서 설명한 call, apply와 사용 방법이 유사하다. 다만, bind는 함수가 가리키는 this만 바꾸고 함수를 직접적으로 호출하지 않는다.

```javascript
var kim = {
  name: 'kim',
  greeting: function() {
    console.log("Hi! I'm " + this.name);
  }
}

var song = {
  name: 'song'
}

var greeting2 = kim.greeting.bind(song);
greeting2(); // 'Hi! I'm song'
```
kim.greeting.bind(song)를 실행하면 greeting 함수의 this가 가리키는 객체가 kim에서 song으로 변경된다.
그리고 함수의 반환값을 변수에 저장하여 함수를 실행할 수 있다.

## Reference
[zerocho blog](https://www.zerocho.com/category/JavaScript/post/57433645a48729787807c3fd)