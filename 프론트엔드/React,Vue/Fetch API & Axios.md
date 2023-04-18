# Fetch API & Axios

## Fetch

ES6부터 들어온 Javascript의 내장 라이브러리이다. 내장 라이브러리이기 때문에 상당히 편리하며, JS의 Promise 기반으로 만들어져서 Axios처럼 데이터를다룰 때 어렵지 않다.

> ✅코드 작성시 차이
>
> url를 따로 받는다. body 부분에서 data를 넘겨주기 위해 JSON.stringify 를 별도로 호출해줄 필요가 있다.

```javascript
fetch("https://localhost:3000/user/post", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    id: "asd123",
    description: "hello world",
  }),
}).then((response) => console.log(response));
```

 👍 **Fetch** **장점**

1. 자바스크립트의 내장 라이브러리 이므로 별도로 import 할 필요가 없다.
2. Promise 기반으로 만들어졌기 때문에 데이터 다루기 편리하다.
3. 내장 라이브러리이기 때문에 업데이트에 따른 에러 방지가 가능하다.

👎 **Fetch** **단점**

1. 네트워크 에러 발생 시 response timeout이 없어 기다려야 한다.
2. JSON으로 변환해주는 과정이 필요하다.
3. 상대적으로 axios에 비해 기능이 부족하다.

## Axios

axios는 Node.js와 브라우저를 위한 Promise API를 활용하는 HTTP 통신 라이브러리이다. 비동기로 HTTP 통신을 할 수 있으며 return을 promise 객체로 해주기 때문에 response 데이터를 다루기 쉽다.

> ✅코드 작성시 차이 
>
> url, body가 합쳐진 property를 받으며, response값을 별도로 .json() 해줄 필요가 없다.

```javascript
axios({
  method: 'post',
  url: 'https://localhost:3000/user',
  data: {
    userName: 'Cocoon',
    userId: 'co1234'
  }
}).then((response) => console.log(response));
```

### 👍 **Axios 장점**

1. response timeout (fetch에는 없는 기능) 처리 방법이 존재한다.
2. Promise 기반으로 만들어졌기 때문에 데이터 다루기 편리하다.

### 👎 **Axios 단점**

1. 사용을 위해 모듈 설치 필요하다. (`npm install axios`)



**참고자료**

https://kangaroo-dev.tistory.com/8