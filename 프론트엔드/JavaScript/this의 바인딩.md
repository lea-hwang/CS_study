### this의 바인딩

💡 먼저 **this**란? this는 Object를 참조하는 keyword이다! (ex. 파이썬의 `self`)

```javascript
function foo() {
  const a = 10;
  console.log(this.a);
}

foo(); // ??
```

해당 코드에서 실행 결과값이 10일것 같지만, 자바스크립트에서 this가 참조하는 것은 **함수가 호출되는 방식에 따라 결정**되는데, 이를 ***this binding***이라고 한다!



❗ EC(Execution Context)가 생성될 때마다 this의 바인딩이 일어나며 우선순위 순으로 나열해보면 다음과 같다.

1. **new 바인딩** : `new`를 사용했을 때 해당 객체로 바인딩된다.
   - 자바스크립트의 `new` 키워드는 함수를 호출할 때 앞에 `new` 키워드를 사용하는 것으로 객체를 초기화할 때 사용, 이때 사용되는 함수를 생성자 함수라고 한다.
   - 생성자 함수에서는 this 키워드를 해당 생성자를 이용해 생성할 객체에 대한 참조로 사용한다.
   - 예제 코드에서 Foo 함수가 new 키워드와 함께 호출되는 순간 새로운 객체가 생성되고, **새로 생성된 객체가 this로 바인딩이 된다**. 그리고 생성된 객체의 a라는 프로퍼티에 20이라는 값이 할당되고, 해당 객체는 foo라는 변수에 할당된다.

```javascript
function Foo() {
  this.a = 20;
}

const foo = new Foo();

console.log(foo.a); // 20
```



2. **명시적 바인딩** : `call`, `apply`, `bind` 와 같은 명시적 바인딩을 사용했을 때 인자로 전달된 객체에 바인딩된다.
   - `call`, `apply`의 매개변수로 바인딩할 객체를 넘겨주면서 bar 함수를 실행할 때의 this 컨텍스트를 foo로 직접 바인딩 해주었다.
   - `call`은 매개변수의 목록, `apply`는 배열을 받는다는 차이점이 있다.
   - `bind` 메서드는 매개변수로 전달받은 오브젝트로 this가 바인딩된 함수를 반환한다.

```javascript
const foo = {
  a: 20,
}

function bar() {
  console.log(this.a);
}

bar.call(foo); // 20
bar.apply(foo); // 20

const bound = bar.bind(foo)

bound(); // 20
```



3. **암시적 바인딩** : 객체의 메소드로 호출할 경우 해당 객체에 바인딩된다.
   - this는 해당 함수를 호출한 객체, 즉 컨텍스트 객체에 바인딩된다.

```javascript
const foo = {
  a: 20,
  bar: function () {
    console.log(this.a);
  }
}

foo.bar(); // 20
```



4. 그 외의 경우

```javascript
function foo() {
  const a = 10;
  console.log(this.a);
}

foo(); // undefined
```

- strict mode: `undefined` 로 초기화된다.

```javascript
window.a = 10;

function foo() {
  console.log(this.a);
}

foo(); // 10
```

- 일반: 브라우저라면 `window` 객체에 바인딩 된다.