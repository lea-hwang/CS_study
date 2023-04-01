# 모듈(module)

: 프로그램을 구성하는 구성 요소로, 관련된 데이터와 함수를 하나로 묶은 단위

개발하는 애플리케이션의 규모가 커지면 여러 개의 파일로 분리해야 하는데, 이 때 분리된 파일 각각을 모듈이라 부른다. 모듈은 대개 하나 혹은 특정한 목적을 가진 여러 개의 함수를 포함하는 라이브러리로 구성되어 있다.



## 장점

- **유지보수용이**: 기능들이 모듈화가 잘 되어 있는 경우, 의존성을 줄일 수 있기 때문에 기능 개선이나 수정이 용이함
- **네임스페이스화**: 코드의 양이 많아질수록 전역스코프에 존재하는 변수들의 변수명이 겹치는 경우가 발생. 이때 모듈로 분리하면 모듈 만의 네임스페이스를 갖기 때문에 해결 가능.
- **재사용성**: 같은 코드를 반복하지 않고 모듈로 분리시켜서 필요할 때 만다 해결할 수 있음

이러한 *모듈을 필요시에 언제든지 불러올 수 있도록 하는 방법*을 `모듈 시스템`이라 한다.



## 모듈 시스템의 종류

- `AMD` -  가장 오래된 모듈 시스템 중 하나로 require.js 라는 라이브러리를 통해 처음 개발됨
- `CommonJS` - NodeJS 환경을 위해 만들어진 모듈 시스템
- `UMD` - AMD와 CommonJS와 같은 다양한 모듈 시스템을 함께 사용하기 위해 만들어짐
- `ES Module` - `ES6(ES2015)`에 도입된 자바스크립트 모듈 시스템



## ES Module 방식

`ES6(ES2015)`에 도입된 자바스크립트 모듈 시스템. `<script>` 태그에 `type="module"` 속성을 추가해주면 해당 파일은 모듈로서 동작함.

```html
<!DOCTYPE html>
<html>
    <head>
        <script type="module" src="index.js"></script>
    </head>
    <body>
    </body>
</html>
```

모듈을 외부에서 사용할 수 있도록 내보낼 때는 `export`, `export default`와 같은 키워드를 사용하며, 외부에서 모듈을 불러올 때는 `import` 를 사용하여 모듈을 불러올 수 있다.



### 내보내기 & 불러오기

1. named export

   - `named export` 를 사용하여 함수 또는 변수를 내보낼 수 있음 `import {} from 모듈` 형태로 불러올 수 있음. 내부 함수 각각을 export했다면 해당 함수의 명칭을 바꿀 수 없다.

     ```javascript
     // math.js
     export const score = 80;
     export const sum = (a, b) => a+b;
     export const avg = (a, b) => (a+b)/2;
     ```

     ```javascript
     // index.js
     import { score, sum } from "./math.js" // 여러 변수/함수를 
     
     import * as math from "./math.js"    // 별칭 지정
     ```

2. export default

   - `export default`를 사용하여 하나 기본 함수를 내보낼 수 있음. 단 **모듈 당 하나만 가능**. `import 함수명/변수명 from 모듈` 형태로 작성 가능.

     ```javascript
     // math.js
     export const score = 80;
     function a = () => {
         return 100;
     }
     export default a; 
     ```

     ```javascript
     // index.js
     import a from "./math.js";
     import func from "./math.js"; // 다른 이름으로도 지정가능
     ```

     ```javascript
     // funcA.js
     const a = 0;
     const b = 1;
     export default {a, b}; // 이렇게 묶어서 여러개 전달 가능
     ```

     ```javascript
     // index.js
     import {a, b} from "./funcA.js"
     ```

     

## CommonJS 방식

: **NodeJS 환경**에서 자바스크립트 모듈을 사용하기 위해 만들어진 모듈 시스템. 

모듈을 외부에서 사용할 수 있도록 내보낼 때는 `exports`, `module.exports`와 같은 키워드로 사용하며 외부에서 모듈을 불러올 때는 `require`를 사용하여 모듈을 불러올 수 있다.

### 내보내기 & 불러오기

- `exports` 변수의 `속성`으로 내보낼 함수를 설정할 수 있다

  ```javascript
  // math.js
  exports.score = 100;
  exports.sum = function (a, b) {
      return a + b;
  }
  exports.avg = (a, b) => (a + b)/2;
  ```

  ```javascript
  // index.js
  const { score, sum, avg } = require("./math.js"); // 각 함수를 받는 방법
  
  const math = require("./math.js"); // 모듈 통째로 받는 방법
  console.log(math.score)
  ```

  

- `module.exports` 를 사용하여 하나의 객체로 묶어서 내보낼 수 있다.

  ```javascript
  // math.js
  ...
  module.exports = {
      score, sum, avg
  }
  ```

  ```javascript
  // index.js
  const { score, sum, avg } = require("./math.js");
  ```

  

### ES Module for NodeJS

`CommonJS` 모듈 시스템을 채택했던 NodeJS 환경에서 `ES Module` 을 사용하려면 Babel과 같은 트랜스파일러(transpiler)를 사용했어야 했는데, NodeJS 버전 13.2부터 ES 모듈 시스템에 대한 정식 지원이 시작됨에 따라 다른 도구 없이 NodeJS에서 손쉽게 `ES Module`을 사용할 수 있게 되었다.

=> package.json -> type="module" 선언

```json
// package.json
{
    "type": "module"
}
```

이렇게 설정하면 ES Module을 사용할 수 있다.



### AMD(Asynchronous Module Definition)

직독직해하면 비동기적인 모듈 선언을 의미한다. 여러 파일을 비동기적으로 병렬 다운로드가 가능해져 상대적으로 로딩 시간이 단축되는 효과를 가져오게 된다.  `RequireJS` 라는 스크립트가 AMD 스펙을 구현해 두었으며 CommonJS가 노드에서 많이 사용된다면 RequireJS는 브라우저 환경에서 많이 사용된다. `define`, `require` 함수를 통해 구현할 수 있다.

```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <script src="require.js"></script>
  </body>
  </html>
```

require.js를 다운받아서 스크립트에 넣어두거나 cdn을 활용해서 해당 파일을 포함시킨다.

```javascript
// js/exModule.js
define([ // 의존 모듈들을 나열한다
    'js/hello', 
    'js/world'
], function(h,w) { // 콜백함수. 의존 모듈들을 순서대로 매개변수에 담긴다 
  return {
    a: h,
    b: w,
    printA : () => console.log('a'),
  }
});

// example.js
require([ // 불러들을 모듈을 나열한다.
    'js/exModule'
], function (exModule) {
  console.log(exModule.a); // hello
  console.log(exModule.b); // world
  exModule.printA(); // a
});
```

다음과 같이 모듈 exModule을 만들고 `define` 함수를 통해 정의한다. require를 통해 첫번째 인자 'exModule' 모듈이 로드되었을 때 변수 exModule로 그것을 받고 define에 정의해 둔 모듈의 프로퍼티들을 이용할 수 있다.

브라우저 환경의 JavaScript는 파일 스코프가 따로 존재하지 않기 때문에 이 define() 함수로 파일 스코프의 역할을 대신한다. 즉, 일종의 네임스페이스 역할을 하여 모듈에서 사용하는 변수와 전역변수를 분리한다. 

### UMD

AMD와 CommonJS가 서로 호환될 수 있도록 만들어진 모듈 시스템이다.

UMD는 하나로 정해진 코드라기 보다는 디자인 패턴에 더 가깝다. AMD, CommonJS, 그리고 기존처럼 window에 추가하는 방식까지 모든 경우를 커버할 수 있는 모듈을 작성하는 것이다.

AMD는 define을 쓰고, CommonJS는 module.exports를 쓰기 때문에 이 차이를 활용하면 UMD를 만들 수 있다. 모듈을 아래와 같이 선언할 수 있다.

```javascript
// myModule.js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) { // AMD
    define(['jquery', 'zerocho'], factory);
  } else if (typeof module === 'object' && module.exports) { // CommonJS
    module.exports = factory(require('jquery'), require('zerocho'));
  } else { // window
    root.myModule = factory(root.$, root.Z); 
  }
}(this, function($, Z) {
  return {
    a: $,
    b: Z,
  };
});
```

즉시 실행함수 덕분에 factory 부분이 AMD, CommonJS 각자의 콜백 함수 또는 모듈 객체가 된다.

AMD와 CommonJS 둘 다 아닌 경우(window)의 경우는 this가 window이기 때문에 root도 window가 되어, window.myModule에 값이 담기게 된다.

이제 어떠한 환경에서도 myModule.js를 불러오면 모듈로 만들었던 `{ a: $, b: Z }`를 사용할 수 있게 된다.



**참고 자료**

https://www.youtube.com/watch?v=5NXEXkIrkAk

https://gobae.tistory.com/130

https://blog.naver.com/PostView.naver?blogId=seek316&logNo=222247976056

https://www.zerocho.com/category/JavaScript/post/5b67e7847bbbd3001b43fd73