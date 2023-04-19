# 엄격 모드 (Strict mode)

ECMAScript 5 에서 소개된 JavaScript 의 엄격모드는 JavaScript 의 제한된 버전을 선택하여 암묵적인 "**느슨한 모드**(sloppy mode)" 를 해제하기 위한 방법이다. strict 모드는 자바스크립트가 묵인했던 에러들의 에러 메시지를 발생시킨다. 

1. 기존에는 조용히 무시되던 에러들을 throwing한다.
2. JavaScript 엔진의 최적화 작업을 어렵게 만드는 실수들을 바로잡는다. 가끔씩 엄격 모드의 코드는 비-엄격 모드의 동일한 코드보다 더 빨리 작동하도록 만들어진다.
3. 엄격 모드는 ECMAScript의 차기 버전들에서 정의 될 문법을 금지한다.



> ​	*"엄격하게 문법 검사를 하겠다."*



## 1. strict 모드 선언

스크립트의 시작 혹은 함수의 시작 부분에 "use strict"(또는 'use strict')를 선언하면 strict 모드로 코드를 작성 할 수 있다.

엄격모드는 *전체 스크립트* 또는 *부분 함수*에 적용가능하다. 단, `{}` 괄호로 묶여진 블럭문에는 적용되지 않는다.

> `"use strict"`는 스크립트 최상단이 아닌 *함수 본문 맨 앞에* 올 수도 있다 -> 오직 *해당 함수만 엄격 모드로* 실행된다.

**🚨 "use strict"는 반드시 최상단에 위치시키기**

`"use strict"`는 스크립트 최상단에 있어야 한다! 그렇지 않으면 엄격 모드가 활성화되지 않을 수도 있다.

아래는 엄격 모드가 활성화되지 않는 코드🔽

```javascript
alert("some code");
// 하단에 위치한 "use strict"는 스크립트 상단에 위치하지 않으므로 무시된다.

"use strict";
// 엄격 모드가 활성화되지 않는다.
```

`"use strict"`의 위에는 주석만 사용할 수 있다.

**🚨 `use strict`를 취소할 방법은 없다.**

자바스크립트 엔진을 이전 방식으로 되돌리는 `"no use strict"`같은 지시자는 존재하지 않는다.

일단 엄격 모드가 적용되면 돌이킬 방법은 없다.



### 스크립트에서 strict 선언

```javascript
"use strict";
var v = "Hi!  I'm a strict mode script!";
```



### 함수에서 strict 선언

```javascript
function strict(){
  // Function-level strict mode syntax
  'use strict';
  function nested() { return "And so am I!"; }
  return "Hi!  I'm a strict mode function!  " + nested();
}
function notStrict() { return "I'm not strict."; }
```



### 모듈에서 strict 선언

JavaScript 모듈의 전체 컨텐츠는 엄격 모드 시작을 위한 구문 없이도 자동으로 엄격모드이다.

```javascript
    function strict() {
        // 모듈이기때문에 기본적으로 엄격하다
    }
    export default strict;
```





## **2. use strict 사용으로 발생하는 제약 조건**

### 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다. 

```javascript
"use strict"
x = 1;
console.log(x);	// ReferenceError: x is not defined
```

 

### 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다. 

```javascript
"use strict"

var x = 1;
delete x;	// SyntaxError: Delete of an unqualified identifier in strict mode.

function foo(a) {
	delete a;	// SyntaxError: Delete of an unqualified identifier in strict mode.
}

delete foo;	// SyntaxError: Delete of an unqualified identifier in strict mode.
```

 

### 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다. 

```javascript
"use strict";

// SyntaxError: Duplicate parameter name not allowed in this context
function foo(x, x) {
	return x + x;
}
console.log(foo(1,2));
```

 

### with문의 사용

with문을 사용하면 SyntaxError가 발생한다. 

```javascript
"use strict";

// SyntaxError: Strict mode code may not include a with statement
with({
	x : 1
}) {
	console.log(x);
}
```

####  

### 전역 this 바인딩

this 값이 null 또는 undefined인 경우 **전역 객체**로 변환하지 않는다. 

strict mode가 아닌 경우에 전역 this가 *window 객체로 자동 바인딩*이 된다. 

```javascript
function foo() {
	this.print = function() {
		console.log(this);
	}
}

let f = new foo();
f.print();	// this = foo 객체 

let f2 = f.print;	// foo의 print method
f2();	// print method 실행 : this = window (전역) 객체
```

 하지만, strict 모드에서는 바인딩해주지 않은 this를 전역객체로 변환해주지 않는다.

```javascript
"use strict";

function foo() {
	this.print = function() {
		console.log(this);
	}
}

let f = new foo();
f.print();	// this = foo 객체 

let f2 = f.print;	// foo의 print method
f2();	// print method 실행 : this = undefined (전역 객체로 반환해주지 않는다)
```





## **3. use strict 사용하는지 체크하기**

**전역 객체인 this와 자바스크립트 함수**를 사용해 엄격 모드가 실행 중인지 확인할 수 있다.

**전역 this는 엄격모드에서 사용할 수 없기** 때문에 this가 사용 가능하면(true 반환) strict 모드이고, 그렇지 않으면(false 반환) strict 모드가 아님을 알 수 있다. 

```javascript
function isStrictMode() {
	return !this;
}
```

 

  

## **4. 'use strict'를 사용한다고 더 안정적인 코드가 되지는 않는다.**

'use stict'는 더 엄격하게 코드 사용 규칙을 제한하고, 잠재적으로 오류가 발생할 수 있는 전역 변수 사용을 제한하기 때문에 더 좋은 코드를 작성하는데 도움을 주는 것은 분명하다. 

그렇다고 'use strict' 사용이 극적으로 더 안정적인 코드를 만들어주는 것은 아니다. 

대부분의 자바스크립트 코드에서 가장 크고 빈번하게 문제를 일으키는 것은 **로직의 오류**이기 때문에,

'use strict'가 아니어도 이런 잠재적인 대입 문제는 대부분의 경우 이후의 실행 로직에서 문제를 일으켜 오류를 확인하지 못하고 넘어갈 가능성은 생각보다 낮다. 