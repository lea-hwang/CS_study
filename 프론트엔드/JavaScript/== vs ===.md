# == vs ===

-  '==' 연산자를 이용하여 서로 다른 유형의 두 변수의 [값] 비교, 변수 값을 기반으로 유형을 수정
-  '==='는 엄격한 비교(변수 유형을 고려)를 하는 것으로 알려져 있다 ([값 & 자료형] -> true). 타입을 변환하지 않으므로 타입이 다르면, false를 반환



<img src="https://velog.velcdn.com/post-images%2Ffiloscoder%2F8d6f8a80-fa17-11e9-b483-31f82d28ec79%2FabY0g3L700bwp.webp" alt="img" style="zoom:80%;" />



 **변수를 비교하거나 어떤 비교를 위해 항상 '===' 연산자를 사용 할 것을 권장한다.**

>  가능한 '==' 연산자를 사용하지 않도록 하고, 대신 직접 자료형을 변환하여(casting) 보다 코드 가독성을 높이도록 한다.



```javascript
console.log(null == undefined); // true 
console.log(null === undefined); // false 
```

> null 은 원시값(Primitive Type) 중 하나로, 어떤 값이 의도적으로 비어있음을 표현한다. **undefined 는 값이 지정되지 않은 경우를 의미하지만, null 의 경우에는 해당 변수가 어떤 객체도 가리키고 있지 않다는 것을 의미**(리터럴값)한다.



```javascript
console.log(true == 1); // true 두 피연산자에서 불리언 값이 존재하면, 불리언 값을 1로 변환 후 값을 비교한다.
console.log(true === 1); // false 
```



```javascript
console.log(0 == "0"); // true 
console.log(0 === "0"); // false 
console.log(0 == ""); // true 
console.log(0 === ""); // false 
```



```javascript
console.log(NaN == NaN); // false 
console.log(NaN === NaN); // false 
```

- **NaN** (Not a Number)값은 자기 자신을 포함하여 어떠한 값과도 일치하지 않는다. 즉, === 연산자에 **NaN** 값이 존재하는 경우 항상 **false**이다.

- 정확한 문자열을 비교하기 위해서는 **localeCompare** 메서드를 사용하는 것이 좋다. 문자열은 눈으로 보았을 때, 동일하더라도 인코딩 방식이 다르게 되어있을 수 있기 때문이다.

  > **`localeCompare()`**메서드는 참조 문자열이 정렬 순서에서 주어진 문자열과 앞인지 뒤인지 또는 같은지 나타내는 숫자를 반환한다.



[객체형(Object type)]

```javascript
var a = [1,2,3]; 
var b = [1,2,3]; 
console.log(a == b); // false 
console.log(a === b); // false 
```

배열을 할당할때, 각 변수는 각 메모리의 주소를 참조한다. 

두 변수 a, b의 **값과 데이터 타입이 같지만**, 이와 상관없이 **참조하는 메모리의 주소가 다르기** 때문에 두 a, b는 같지 않다. 



```javascript
var a = [1,2,3]; 
var b = [1,2,3]; 
var c = b; 
console.log(b === c); // true 
console.log(b == c); // ture 
```

**새로운 변수 c에 변수 b를 할당해주면, 변수 c도 b가 참조하는 같은 메모리의 주소를 참조**하게 되어, 두 변수 c, b는 같다. 

이때, c, b의 값과 데이터 타입이 같기 때문에, ==와 ===의 결과값이 동일하다. 



객체도 마찬가지!

```javascript
var x = {}; 
var y = {}; 
var z = y; 
console.log(x == y) // false 
console.log(x === y) // false 
console.log(y === z) // true 
console.log(y == z) // true 
```

