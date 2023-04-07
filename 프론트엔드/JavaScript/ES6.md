# ES6

## Destructuring Assignment

: 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식

```javascript
const student = {
    name: 'lea',
    level: 1
};

const { name, level } = student;
console.log(name, level);

// 변수명을 key와 다르게 하고 싶은 경우
const { name: studentName, level: studentLevel} = student;
console.log(studentName, studentLevel);

// 배열도 가능하다
let a, b, c;
[a, b, c=10] = [1, 2];
console.log(c)l // 10

// 함수의 반환 값을 활용해 변수의 값을 받을 수도 있다.
function f() {
  return [1, 2, 3];
}

// 두 번째 반환값을 이런식으로 무시할 수도 있다.
[a, , b] = f();
console.log(a); // 1
console.log(b); // 3

// 변수에 배열의 나머지를 할당 할 수도 있다.
[a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```

[MDN - 구조분해할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)



## Spread Syntax

**전개 구문**을 사용하면 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소 (배열 리터럴의 경우)로 확장하여, 0개 이상의 키-값의 쌍으로 객체로 확장시킬 수 있다.

```javascript
const obj1 = { key: 'key1' };
const obj2 = { key: 'key2'};

const array = [obj1, obje2];
const arrayCopy = [...array]; // 배열 복사
//array와 arrayCopy 동일하게 출력됨

// 객체 리터럴에서의 전개
const obj1 = { foo: 'bar', x: 42 };
const obj2 = { foo: 'baz', y: 13 };

const clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

const mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
// foo의 값이 'baz'가 된다.
```

[MDN - 전개 구문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)



## Default parameters

함수의 매개변수에 기본값(default)을 설정해줄 수 있다. 

```javascript
function printMessage(message = 'default message'){
	console.log(message);
};
printMessage();
```

falsy한 값을 전달한다면 다음과 같이 작동한다.

```javascript
function test(num = 1) {
  console.log(typeof num)
}

test()            // 'number' (num 은 1로 설정됨)
test(undefined)   // 'number' (num 이 역시 1로 설정됨)

// 다른 falsy values로 테스트 하기:
test('')          // 'string' (num 은 ''로 설정됨)
test(null)        // 'object' (num 은 null로 설정됨)
```

[MDN - 기본값 매개변수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Default_parameters)



## Optional Chaining

optional chaining 연산자 (**`?.`**) 는 체인의 각 참조가 유효한지 명시적으로 검증하지 않고, 연결된 객체 체인 내에 깊숙이 위치한 속성 값을 읽을 수 있다.

`?.` 연산자는 `.` 체이닝 연산자와 유사하게 작동하지만, 만약 참조가 [nullish](https://developer.mozilla.org/ko/docs/Glossary/Nullish) ([`null`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/null) 또는 [`undefined`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined))이라면, 에러가 발생하는 것 대신에 표현식의 리턴 값은 `undefined`로 단락된다. 함수 호출에서 사용될 때, 만약 주어진 함수가 존재하지 않는다면, `undefined`를 리턴한다.

```javascript
// optional chaining을 사용하지 않는 경우 obj.first의 값이 null 또는 undefined가 아닌지 검증하고 obj.first.second에 접근할 수 있도록 코드를 짠다. 그렇지 않으면 에러가 발생할 수 있다.
let nestedProp = obj.first && obj.first.second;

// optional chaining 연산자를 사용하게 되면, 명시적으로 테스트하지 않아도 된다.
let nestedProp = obj.first?.second;
```

[MDN - optional chaining](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining)



## Nullish coalescing operator

: **널 병합 연산자 (`??`)** 는 왼쪽 피연산자가 [null](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/null) 또는 [undefined](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자이다.

```javascript
const name = '';
const userName = name ?? 'Guest';
console.log(userName); // ''

const num = 0;
const message = num ?? "undefined";
console.log(message); // 0
```

이전에는 변수에 기본값을 할당하고 싶을 때, 논리 연산자 OR ([`||`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#logical_or_2))을 사용하는 것이 일반적인 패턴이었으나, 이 동작은 만약 `0`, `''` or `NaN`을 유효한 값으로 생각한 경우 예기치 않는 결과를 초래할 수 있다.

```javascript
let count;
let text;
...
count = 0;
text = "";
...
let qty = count || 42;
let message = text || "hi!";
console.log(qty);     // 42 and not 0
console.log(message); // "hi!" and not ""
```

널 병합 연산자는 첫 번째 연산자가 `null` 또는 `undefined`로 평가될 때만, 두 번째 피연산자를 반환함으로써 이러한 위험을 피한다.

[MDN - Nullish coalescing operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)



## Arrows

: Arrows(화살표) 함수는 `=>` 문법을 사용하는 축약형 함수이다.

- 표현식의 결과 값을 반환하는 표현식 본문(expression bodies)뿐만 아니라 상태 블럭 본문(statement block bodies)도 지원
- 일반 함수의 자신을 호출하는 객체를 가리키는 `dynamic this`와 달리 arrows 함수는 코드의 상위 스코프(lexical scope)를 가리키는 `lexical this`를 가리킴

```javascript
var evens = [2, 4, 6, 8,];

// Expression bodies (표현식의 결과가 반환됨)
var odds = evens.map(v => v + 1);   // [3, 5, 7, 9]
var nums = evens.map((v, i) => v + i);  // [2, 5, 8, 11]
var pairs = evens.map(v => ({even: v, odd: v + 1})); // [{even: 2, odd: 3}, ...]

// Statement bodies (블럭 내부를 실행만 함, 반환을 위해선 return을 명시)
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
// 출력결과 : Bob knows John, Brian
var bob = {
  _name: "Bob",
  _friends: ["John, Brian"],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```



## Classes

ES6 클래스는 프로토타입 기반 객체지향 패턴을 더 쉽게 사용할 수 있는 대체재이다. 클래스 패턴 생성을 더 쉽고 단순하게 생성할 수 있어서 사용하기도 편하고 상호운용성도 증가한다.

```javascript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

[MDN - class](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/class)



### Enhanced Object Literals

ES6에서 객체 리터럴은 선언문에서 프로토타입 설정, `foo: foo` 선언을 위한 단축 표기법, 메서드 정의, super 클래스 호출 및 동적 속성명을 지원. 

```javascript
var obj = {
    // __proto__
    __proto__: theProtoObj,

    // ‘handler: handler’의 단축 표기
    handler,

    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },

    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

[MDN Grammar and types: Object literals 353](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Values,_variables,_and_literals#객체_리터럴).

### Template Strings(Template literals)

: 여러 줄로 이뤄진 문자열과 문자 보간기능을 사용가능한 문자열 ``를 사용해 표현함

Tagged template literals는 인젝션 공격 방어 혹은 문자열로 부터 상위 데이터 구조체 재조립 등을 위해 string 생성을 커스터마이징이 가능하게끔 해줌.

```javascript
// Basic literal string creation
`In JavaScript '\n' is a line-feed.`

// Multiline strings
`In JavaScript this is
 not legal.`

// String interpolation
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

[MDN Template literals 254](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)

### Default + Rest + Spread

**Default**: 파라미터에 기본 값 설정.

```javascript
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) // 15
```

**Rest**: 함수에 전달된 인수들의 목록을 배열로 전달. 

```javascript
function f(x, ...y) {
  // y is an Array ["hello", true]
  return x * y.length;
}
f(3, "hello", true) // 6
```

**Spread**: 함수 호출 시 배열을 일련의 인자에 나누어 주입.

```javascript
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) // 6
```

[Default parameters 69](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Default_parameters), [Rest parameters 112](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters), [Spread Operator 244](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_operator)

### Let + Const

**블록 유효 범위를 갖는 새로운 변수 선언 방법**을 지원합니다. 

- `let`: 재할당 가능, 재선언 불가
- `const`: 재할당 및 재선언이 불가

```javascript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

- `var`의 유효 범위: 전체 외부 함수
- `let`, `const`: 변수를 선언한 블록과 그 내부 블록들에서 유효.

```javascript
function varTest() {
    var x = 31;
    if (true) {
        var x = 71;  // same variable!
        console.log(x);  // 71
    }
    console.log(x);  // 71
}

function letTest() {
    let x = 31;
    if (true) {
        let x = 71;  // different variable
        console.log(x);  // 71
    }
    console.log(x);  // 31
}
function varTest() {
    if (true) {
        var x = 71;
        console.log(x);  // 71
    }
    console.log(x);  // 71
}

function varTest() {
    let x = 71;
    if (true) {
        console.log(x);  // 71
    }
    console.log(x);  // 71
}

function varTest() {
    if (true) {
        let x = 71;
        console.log(x);  // 71
    }
    console.log(x);  // Uncaught ReferenceError: x is not defined
}
```

 [let statement 72](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let), [const statement 56](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)

### Iterators + For…Of

반복자는 자신만의 반복을 정의하는 규약이고 이는 `for...of` 를 통해 순회할 수 있다. `[Symbol.iterator]` 라는 이름의 메서드를 정의해야하며 그 메서드는 반드시 `next()` 메서드를 가진 객체를 반환해야 한다.

```javascript
let fibonacci = {
    [Symbol.iterator]() {
        let pre = 0, cur = 1;

        return {
            next() {
                [pre, cur] = [cur, pre + cur];
                return { done: false, value: cur }
            }
        }
    }
}

for (var n of fibonacci) {
    // truncate the sequence at 1000
    if (n > 1000)
        break;
    console.log(n); // 1, 2, 3, 5, 8, ...987
}
```

*value에는 꺼낸 값*이 저장되고 *done에는 반복이 끝났는지를 뜻하는 논리값*이 저장된다.

```javascript
let a = [1, 2, 3];
let iter = a[Symbol.iterator]();
// Symbol.iterator(이터레이터 심벌)는 이터레이터를 반환하는 메서드

console.log(iter.next()); // Object {value: 1, done: false}
console.log(iter.next()); // Object {value: 2, done: false}
console.log(iter.next()); // Object {value: 3, done: false}
console.log(iter.next()); // Object {value: undefined, done: true}
console.log(iter.next()); // Object {value: undefined, done: true}
```

### Generators

반복자를 쉽게 생성해주는 것으로 `function*` 과 `yield` 를 사용한다. 반복자의 하위 타입으로, `next` 와 `throw` 를 포함한다. 또한 ES7의 `await` 과 같이 사용할 수 있다.

Generator는 Iterable이면서 Iterator인 객체의 특별한 종류이다. 이 객체는 일시정지와 재시작 기능을 여러 반환 포인트들을 통해 사용할 수 있다. 이러한 반환 포인트들은 `yield` 키워드를 통해 구현할 수 있으며, 오직 generator 함수에서만 사용할 수 있다. `next`호출시마다 다음 `yield`의 expression이 반환된다.
`yield value`를 사용하면 한가지 값을 반환할 수 있고, `yield* iterable`을 사용하면 해당되는 Iterable의 값들을 순차적으로 반환시킬 수 있다.

Generator의 반복이 끝나는 시점은 3가지 경우인데, generator 함수에서 `return` 사용, 에러 발생 그리고 마지막으로 함수의 끝부분까지 모두 수행된 이후, 이렇게 3가지 경우이다. 그리고 이때 done 프로퍼티가 true가 될 것이다.

이러한 generator함수는 Generator객체를 반환하며, `function*`키워드로 정의할 수 있다. 또는 클래스에서 메서드 이름 앞에 `*`을 붙여 정의할 수도 있다.

```javascript
function* gen(){
  yield* ["a", "b", "c"];
}

var a = gen();

a.next(); // { value: "a", done: false }
a.next(); // { value: "b", done: false }
a.next(); // { value: "c", done: false }
a.next(); // { value: undefined, done: true }
```

```javascript
function* fibonacci() {
    let prev = 0, curr = 1; // [prev, curr] = [0, 1];
    yield prev;
    yield curr;
    while (true) {
        let temp = prev;
        prev = curr;
        curr = temp + curr;
        // [prev, curr] = [curr, prev + curr];
        
        yield curr;
    }
}

for (const n of fibonacci()) {
    if (n > 100) break;
    console.log(n);
}
```

[MDN Iteration protocols 61](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols)

### Map + Set + WeakMap + WeakSet

자주 쓰이는 자료구조로, Weak이 붙은 것은 가비지 컬렉션을 허용하며 `size` 프로퍼티를 가지지 않는다.

```javascript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size // undefined (사용된 곳이 없기 때문)

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
wm.size // undefined (사용된 곳이 없기 때문)
```

MDN [Map 69](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map), [Set 42](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set), [WeakMap 89](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/WeakMap), [WeakSet 28](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

### Symbols

새로 추가된 **원시타입** 으로 유일한 값을 가지며 객체의 접근제어를 가능하게 한다. `description` 매개변수를 이용해 디버깅이 가능하며 `Object.getOwnPropertySymbols` 를 통해 객체의 심볼 프로퍼티들을 볼 수 있다.

```javascript
var map = {};
var a = Symbol('a');

map[a] = 123;
map["b"] = 456;

console.log(map[a]); // 123
console.log(map["b"]); // 456

for (let key in map) {
    console.log(key); // b
}

Object.keys(map); // ["b"]
```

더 자세한 내용은 [ES6 In Depth: 심볼 (Symbol) 93](http://hacks.mozilla.or.kr/2015/09/es6-in-depth-symbols/)를 참고하세요.

### Math + Number + String + Array + Object APIs

core Math 라이브러리, Array 생성 helper, String helper, 복사를 위한 Object.assign 등 많은 라이브러리들이 추가됨

```javascript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

[Number 11](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number), [Math 12](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math), [Array.from 16](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from), [Array.of 12](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/of), [Array.prototype.copyWithin 12](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin), [Object.assign 23](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

### Promises

비동기 작업이 맞이할 미래의 완료/실패와 결과 값을 나타내는 객체이다.

```javascript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

 [MDN Promise 204](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)



참고자료

https://velog.io/@dami/JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%B5%9C%EC%8B%A0-%EB%AC%B8%EB%B2%95-ES6-ES11-%EB%B3%B5%EC%8A%B5

https://www.youtube.com/watch?v=36HrZHzPeuY

https://itstory.tk/entry/JavaScript-ES6-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC

https://ui.toast.com/weekly-pick/ko_20151021