# var vs let vs const



## Var

- ES6의 등장 이전 `var`로 변수를 선언하는 것이 지배적이었다.
- 현재에는 거의 사용하지 않는다.



### Scope of var

- `스코프` : 변수를 사용할 수 있는 위치

- 전역 범위 혹은 함수 범위로 지정

  - 함수 외부에서 선언될 때의 범위는 전역
  - 함수 내에서 선언될 때는 함수 범위

  ```javascript
   var tester = "hey hi";
      
  function newFunction() {
      var hello = "hello";
  }
  console.log(hello); // error: hello is not defined
  ```

  `hello`는 `newFunction()` 함수 밖에서 사용할 수 없기 때문에 에러가 발생



### var 변수는 재선언되고, 업데이트될 수 있다.

- 같은 범위 내에서라면 수정이 가능하며 에러가 발생하지 않는다.



```javascript
var a = 123;
console.log(a); // 123
 
var a = 567;
console.log(a); // 567
```

같은 이름의 변수를 *중복해서 선언해도 정상적으로 동작*하며(error가 발생하지 X), *가장 마지막 값을 저장*하고 있는다.

이처럼 이미 선언했던 변수명을 모르고 또 사용할 경우, **기존에 있던 변수는 전혀 다른 값**을 가지게 됩니다. 그 경우, 그 변수를 사용하는 다양한 **로직들에 치명적인 문제**가 생깁니다.



### var의 호이스팅

- `호이스팅`

  - 변수와 함수 선언이 맨 위로 이동되는 자바스크립트 매커니즘

  - var를 사용해서 변수 선언 했을 때 해당 **변수의 선언부를 Scope 최상단으로** 올리는 것

  - JavaScript의 *변수 생성과 초기화의 작업이 분리되어 진행되기 때문*

  - var : Function-scoped => 코드 전체의 최상단이 아니라 **함수 내부의 최상단**으로 이동한다.

    >  `Function-scoped` : 함수 내부의 영역을 범위로 갖는 것

- 예

  ```javascript
  // 아래처럼 코딩하면
  var a = 123;
  function func() {
    console.log(a); // undefined
    var a = 456;
    console.log(a); // 456
  }
  func();
  ```

  ```javascript
  // var의 호이스팅에 의해 아래와 같이 인식됨
  var a = 123;
  function func() {
    var a;
    console.log(a); // undefined
    a = 456;
    console.log(a); // 456
  }
  func();
  ```

  호이스팅 때문에 a를 참조할 Reference는 있으니 Reference Error는 나지 않고, 대신 `undefined(자료형이 정해지지 않은 상태)`로 값이 초기화된다.



### var의 문제점

1. **변수의 중복 선언이 가능하다.**

   var는 a라는 이름의 변수가 있음에도 불구하고 또 그 아래에서 a라는 이름의 변수를 선언할 수 있다.

   이처럼 이미 선언했던 변수명을 모르고 또 사용할 경우, **기존에 있던 변수는 전혀 다른 값**을 가지게 됩니다. 그 경우, 그 변수를 사용하는 다양한 **로직들에 치명적인 문제**가 생깁니다.

2. **for문에서의 문제점**

   var는 Function-scoped이기 때문에 for문에서 for문의 순회를 위해 i라는 변수를 var로 선언한 경우, 이 변수는 **for문이 종료되어도 접근이 가능**하다.

   만약 for문의 함수 내부가 아닌, **함수 외부에 전역적으로 돌아갈 경우** for문에서 사용한 var 변수들은 **전역 변수**로서 역할을 하므로, **전역 변수가 남발**될 수 있습니다.





## let

### 블록 범위 let

블록은 `{}`로 바인딩된 코드 청크. 하나의 블록은 중괄호 속에서 존재하며, 중괄호 안에 있는 것은 모두 블록 범위이다.

**`let`으로 선언된 변수는 해당 블록 내에서만 사용가능**

```javascript
let greeting = "say Hi";
let times = 4;

if (times > 3) {
    let hello = "say Hello instead";
    console.log(hello);// "say Hello instead"
}
console.log(hello) // hello is not defined
```

중괄호로 감싸진 `hello` 변수가 정의된 **블록 외부에서 `helllo`를 사용했더니 에러가 반환**되는 것을 확인할 수 있다. `let` 변수는 블록 범위이기 때문.



### let은 업데이트될 수 있지만, 재선언은 불가능하다.

```javascript
// 업데이트 가능
let greeting = "say Hi";
greeting = "say Hello instead";
```



```javascript
// 재선언 불가
let greeting = "say Hi";
let greeting = "say Hello instead"; // error: Identifier 'greeting' has already been declared
```

동일한 변수가 다른 범위 내에서 정의된다면, 에러는 더 이상 발생 X



```javascript
let greeting = "say Hi";
if (true) {
    let greeting = "say Hello instead";
    console.log(greeting); // "say Hello instead"
}
console.log(greeting); // "say Hi"
```

오류가 없는 이유 : 서로 다른 범위를 가지므로 서로 다른 변수로 취급되기 때문. 

`let`을 사용하는 경우라면, **변수가 범위 내에서만 존재**하기 때문에 이전에 이미 사용한 적이 있는 변수명에 대해서 더 이상 신경쓰지 않아도 된다.또한, 범위 내에서 동일한 변수를 두 번 이상 선언할 수 없기 때문에 앞서 설명한 `var`의 문제가 발생하지 않는다.



### let의 호이스팅

`var`와 마찬가지로 `let` 선언은 맨 위로 끌어올려진다. `undefined(정의되지 않음)`으로 초기화되는 `var`와 다르게 `let`의 키워드는 초기화되지 않는다. 선언 이전에 `let` 변수를 사용하려고 시도한다면 `Reference Error(참조 오류)`가 발생할 것이다.





## Const

`const`로 선언된 변수는 **일정한 상수 값을 유지**한다. `const` 선언은 `let` 선언과 몇 가지 유사점을 공유한다.



### 블록 범위 const

`let` 선언처럼 `const` 선언도 **선언된 블록 범위 내에서만 접근 가능**.



### const는 업데이트, 재선언 둘다 불가능하다

`const`로 선언된 변수의 값이 **해당 범위 내에서 동일하게 유지됨**을 의미한다. 즉, 업데이트하거나 다시 선언할 수가 없다.

 `const`로 변수를 선언한 경우에는 다음과 같은 두 작업을 수행할 수 없다:

```javascript
// 업데이트 불가
const greeting = "say Hi";
greeting = "say Hello instead";// error: Assignment to constant variable.

// 재선언 불가
const greeting = "say Hi";
const greeting = "say Hello instead";// error: Identifier 'greeting' has already been declared 
```

따라서 모든 `const` 선언은 선언하는 당시에 초기화되어야 힌다.

개체의 경우는 다소 다른 점이 있다. `const` 개체는 업데이트할 수 없지만, **개체의 '속성'은 업데이트할 수 있다**. `const` 객체를 다음과 같이 선언했다면:

```javascript
const greeting = {
    message: "say Hi",
    times: 4
}
```

아래와 같은 작업은 불가능하지만:

```javascript
// 업데이트 불가
greeting = {
    words: "Hello",
    number: "five"
} // error:  Assignment to constant variable.
```

다음의 코드는 가능하다:

```javascript
greeting.message = "say Hello instead";
```

오류를 반환하지 않고 `greeting.message` 값이 업데이트된다.



### const의 호이스팅

`let`과 마찬가지로 `const` 선언도 맨 위로 끌어올려지지만, 초기화되지는 않는다.



### 🪄 세 가지 변수 선언법의 차이점에 대해서 총정리:

- `var` 선언은 **전역 범위 또는 함수 범위**이며, `let`과 `const`는 **블록 범위**이다.
- `var` 변수는 범위 내에서 업데이트 및 재선언할 수 있다. `let` 변수는 업데이트할 수 있지만, 재선언은 할 수 없다. `const` 변수는 업데이트와 재선언 둘 다 불가능하다.
- 세 가지 모두 최상위로 호이스팅된다. 하지만 `var` 변수만 **`undefined(정의되지 않음)`으로 초기화**되고 `let`과 `const` 변수는 **초기화되지 않는다**.
- `var`와 `let`은 **초기화하지 않은 상태에서 선언**할 수 있지만, `const`는 **선언 중에 초기화**해야한다.





참고)

https://www.freecodecamp.org/korean/news/var-let-constyi-caijeomeun/

https://hoondev.tistory.com/101

https://hoondev.tistory.com/101