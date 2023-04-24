# JSX

"React에서 사용하는" *자바스크립트를 확장*한 문법. UI가 어떻게 생겨야 하는지를 설명함. 

```jsx
const element = <h1>Hello, world!</h1>
```

JSX는 `React element`를 생성한다. 



### JSX에 표현식 포함하기

JSX의 `중괄호` 안에는 모든 `Javascript 표현식`을 넣을 수 있다.

```react
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```



### JSX 속성 정의

`속성`에 `따옴표("")`를 이용해 **문자열 리터럴**을 정의할 수 있다.

```react
const element = <a href="https://www.reactjs.org"> link </a>;
```

`중괄호`를 사용해 속성에 **JavaScript 표현식**을 삽입할 수도 있다.

```react
const element = <img src={user.avatarUrl}></img>;
```

> ❗따옴표(문자열 값에 사용) 또는 중괄호(표현식에 사용) 중 하나만 사용하고, 동일한 어트리뷰트에 두 가지를 동시에 사용하면 안 된다.



### JSX로 자식 정의

태그가 비어있다면 `/>`를 이용해 바로 닫아주어야 하며,

```react
const element = <img src={user.avatarUrl} />;
```

JSX 태그는 자식을 포함할 수 있다.

```react
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

> ❗가독성을 좋게 하기 위해 JSX를 여러 줄로 나누는데, `자동 세미콜론 삽입`을 피하고자 `괄호`로 묶는 것을 권장한다고 한다



### XSS 공격을 방지

모든 항목은 렌더링 되기 전에 문자열로 변환(이스케이프)하기 때문에 XSS 공격을 방지할 수 있다.

```react
const title = response.potentiallyMaliciousInput;
// 이것은 안전합니다.
const element = <h1>{title}</h1>;
```



### 객체를 표현

Babel은 JSX를 `React.createElement()` 호출로 컴파일한다.

```react
// 아래 코드와 동일하다.
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
// Babel이 이렇게 컴파일한다.
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

이러한 객체를 `React 엘리먼트`라고 하며 화면에서 보고 싶은 것을 나타내는 표현이다.