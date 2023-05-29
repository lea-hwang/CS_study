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


- 참고하면 좋을 내용(chat GPT)
```
Response timeout는 일반적으로 컴퓨터 네트워크와 관련된 개념으로, 통신 중에 일정 시간 동안 응답을 받지 못할 경우 발생하는 현상을 가리킵니다.

예를 들어, 클라이언트가 서버에 요청을 보내고, 서버는 그 요청에 대한 응답을 반환해야 합니다. 그러나 네트워크 연결이 불안정하거나 서버에 부하가 많아서 응답이 지연되거나 전혀 오지 않을 수 있습니다. 이때 클라이언트는 일정 시간 동안 응답을 기다리다가, 시간이 초과되면 "response timeout" 오류를 받게 됩니다.

response timeout은 네트워크 응용프로그램에서 매우 중요한 개념입니다. 일정 시간 이내에 응답을 받지 못한 경우, 클라이언트는 오류 처리를 위해 다른 조치를 취할 수 있습니다. 예를 들어, 재시도하거나 예외 처리를 수행할 수 있습니다. timeout 값은 응용프로그램에서 설정할 수 있으며, 일반적으로 네트워크 환경과 요청에 따라 조정됩니다.
Fetch API와 Axios는 모두 HTTP 요청을 보내고 응답을 받는 데 사용되는 JavaScript 라이브러리입니다. 그러나 Fetch API는 내장된 브라우저 API이며, Axios는 브라우저 및 Node.js 환경에서 사용할 수 있는 외부 라이브러리입니다. 이들 간에는 response timeout을 처리하는 방식에 차이가 있습니다.

Fetch API는 기본적으로 타임아웃 기능을 제공하지 않습니다. 이는 요청을 보낸 후 응답을 받을 때까지 무기한 대기하게 됩니다. 따라서 Fetch API를 사용할 때는 직접 timeout을 구현해야 합니다. 이를 위해 setTimeout 함수를 사용하여 일정 시간이 지나면 요청을 취소하고 적절한 조치를 취할 수 있습니다. 예를 들면 다른 요청을 시도하거나 에러를 처리하는 등의 방식입니다.

반면에 Axios는 요청에 대한 타임아웃 기능을 내장하고 있습니다. timeout 옵션을 사용하여 응답을 기다리는 최대 시간을 설정할 수 있습니다. 예를 들면 아래와 같이 타임아웃을 설정할 수 있습니다.
이렇게 Axios는 내장된 타임아웃 기능을 제공하여 개발자가 명시적으로 타임아웃을 설정하고 처리할 수 있도록 도와줍니다. 이는 Fetch API에서 직접 구현해야 하는 타임아웃 기능의 번거로움을 줄여줍니다.
```

**참고자료**

https://kangaroo-dev.tistory.com/8
