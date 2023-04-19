# IIFE(Immediately Invoked Function Expression)

- **정의** 되자마자 실행되는 JavaScript 함수
- JavaScript 이벤트 루프에서 호출되거나 호출되는 순간 실행되는 함수

```javascript
(function () {
  // …
})();

(() => {
  // …
})();

(async () => {
  // …
})();
```



### 특징

- 글로벌 JS 범위의 오염을 방지한다.
  - **기존 함수**에서는 **함수 내에서 변수**를 만들면 **전역 개체에서 접근**할 수 있다. **IIFE**에서 변수를 정의하면 **함수 내에서만 직접 접근**할 수 있다.
- `setTimeout()`메서드와 같은 비동기 작업을 수행한다.
- IIFE를 사용하여 개인 변수를 생성할 수도 있다. 실수로 중요한 값을 수정하거나 변경하는 것을 방지해야 하는 경우에 유용.



### Use cases

1. 전역 네임스페이스 오염 방지

   응용 프로그램은 다른 소스 파일의 많은 함수와 전역 변수를 포함할 수 있기 때문에 전역 변수의 수를 제한하는 것이 중요하다. **코드를 다시 재사용하지 않는 경우** 함수 선언이나 함수 표현식을 사용하는 것보다 **IIFE를 사용하는 것이 낫다**.

   

2. 비동기 함수 실행

   IIFE 를 사용하면 최상위 await가 없는 이전 브라우저 및 JavaScript 런타임에서도 `async`사용할 수 있다.

   ```javascript
   const getFileStream = async (url) => {
     // implementation
   };
   
   (async () => {
     const stream = await getFileStream("https://domain.name/path/file.ext");
     for await (const chunk of stream) {
       console.log({ chunk });
     }
   })();
   ```

   

3. 모듈 패턴

   IIFE를 사용하여 private 및 public 변수와 메서드를 생성할 수 있다.

   > `프라이빗 변수` : 변수를 마음대로 사용하는 것을 막음

   ```javascript
   const makeWithdraw = (balance) =>
     ((copyBalance) => {
       let balance = copyBalance; // This variable is private
       const doBadThings = () => {
         console.log("I will do bad things with your money");
       };
       doBadThings();
       return {
         withdraw(amount) {
           if (balance >= amount) {
             balance -= amount;
             return balance;
           }
           return "Insufficient money";
         },
       };
     })(balance);
   
   const firstAccount = makeWithdraw(100); // "I will do bad things with your money"
   console.log(firstAccount.balance); // undefined
   console.log(firstAccount.withdraw(20)); // 80
   console.log(firstAccount.withdraw(30)); // 50
   console.log(firstAccount.doBadThings); // undefined; this method is private
   const secondAccount = makeWithdraw(20); // "I will do bad things with your money"
   console.log(secondAccount.withdraw(30)); // "Insufficient money"
   console.log(secondAccount.withdraw(20)); // 0
   ```

   

4. ES6 이전의 var가 있는 For 루프

   - **ES6** 및 블록 범위 에서 **let** 및 **const** 문이 도입되기 전

     ```javascript
     // Button 0과 Button 1이라는 텍스트가 있는 2개의 버튼을 생성하고 이를 클릭할 때 0과 1을 알리도록 한다고 가정
     for (var i = 0; i < 2; i++) {
       const button = document.createElement("button");
       button.innerText = `Button ${i}`;
       button.onclick = function () {
         console.log(i);
       };
       document.body.appendChild(button);
     }
     console.log(i); // 2
     // 클릭하면 Button 0과 Button 1 모두 2를 알린다(i는 전역이므로)
     ```

   - ES6 이전에 이 문제를 해결하려면 IIFE 패턴을 사용할 수 있다.

     ```javascript
     for (var i = 0; i < 2; i++) {
       const button = document.createElement("button");
       button.innerText = `Button ${i}`;
       button.onclick = (function (copyOfI) {
         return function () {
           console.log(copyOfI);
         };
       })(i);
       document.body.appendChild(button);
     }
     console.log(i); // 2
     // 클릭하면 버튼 0과 1이 0과 1을 알려준다. 변수 i는 전역적으로 정의된다.
     ```

   - **let** 문을 사용시

     ```javascript
     for (let i = 0; i < 2; i++) {
       const button = document.createElement("button");
       button.innerText = `Button ${i}`;
       button.onclick = function () {
         console.log(i);
       };
       document.body.appendChild(button);
     }
     console.log(i); // Uncaught ReferenceError: i is not defined.
     // 클릭하면 이 버튼들은 0과 1을 알려준다.
     ```

     



참고)

https://developer.mozilla.org/en-US/docs/Glossary/IIFE

https://medium.com/sjk5766/iife-immediately-invoked-function-expression-%EC%A0%95%EB%A6%AC-53ab6543b828

https://circleci.com/blog/ci-cd-for-js-iifes/