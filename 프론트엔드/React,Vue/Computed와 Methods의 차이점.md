# Computed와 Methods의 차이점

## 🔎 Computed

- `계산된`
- 어떠한 값을 계산된 값으로 제공해주는 속성
- 계산해야하는 목표 데이터를 정의 하는 ‘선언형’ 프로그래밍 방식
- 저장된 결과(캐싱)를 반환하므로 종속 대상(data 속성에 정의한 반응형 데이터(reactive data) )의 변경이 일어나기 전까지 호출 되지 않는다.

```vue
<script type="text/javascript">
let vm = new Vue({
    el: '#app',
    data: {
       message: 'Hello world'
    },
    computed: {
        reverse: function() {
            console.log('I am computed :: reverse')
            return this.message.split('').reverse().join('')
        },
        now: function() { // 아무곳에도 의존하지 않기때문에 절대로 업데이트 되지 않는다.
            console.log('I am computed :: now')
            return Date.now()
        }
    }
})
// message 값을 변경하자 computed 의 reverse 함수가 호출된다.
// point1. reverse가 message 데이터에 의존적이기 때문에 message 값이 변경되면 업데이트 된다.
// point2. computed 속성은 종속 대상을 따라 캐싱된다. 즉, 종속 대상이 변경될 때만 호출되므로 message가
// 변하지 않으면 reverse 는 계산을 다시하지 않고 캐싱 결과를 반환한다.
vm.message = '안녕 세계!' // message 를 업데이트하자 reverse 가 호출 된다.
</script>
```

reverse 에 구현된 함수를 보면 message 라는 반응형 데이터 사용하여 값을 반환하고 있다. (message 값 에 의존한다)

now에 구현된 함수는 그냥 현재 시간을 반환하고 있다. (종속된 데이터가 없다)

reverse는 리턴값을 캐싱 하고 있어 `message` 값이 바뀔때만 업데이트 된다 (함수가 호출 된다). now는 어떤 값에도 의존하지 않기 때문에 절대로 업데이트 되지 않는다.





## 🔎 Methods

- 일반적으로 함수를 정의할때 사용
- 종속된 값이란 개념없이 렌더링이 일어날 때마다 계속 호출된다.

```vue
<body>
    <div id="app">
      <input type="text" :value="fetchBanana()">
      <input type="text" :value="fetchApple()">
    </div>
</body>
<script type="text/javascript">
let vm = new Vue({
    el: '#app',
    data: {
       banana: '바나나',
       apple: '사과',
    },
    methods: {
        fetchBanana: function() {
            console.log('fetch banana')
            return `${this.banana} 가져왔어요!`
        },
        fetchApple: function() {
            console.log('fetch apple')
            return `${this.apple} 가져왔어요!`
        }
    }
})
vm.banana = '바나나 + 망고' // fetchBanana, fetchApple 모두 호출 되는것 확인!
</script>
```

![콘솔로그](https://sunny921.github.io/assets/post/0827-computed-methods.png)

`vm.banana = '바나나 + 망고'`로 banana 를 업데이트 했는데 콘솔 로그를 보니 fetchApple 까지 호출 된다.

#### Why? 

*data의 변경으로 **화면의 렌더링이 발생**했고, 템플릿 안에서 fetchBanana(), fetchApple() **함수가 재호출**되었기 때문.*

그럼 템플릿 안에서 10개의 함수 호출 구문을 넣었다면? 그 안에서 api 호출이나 무거운 로직을 구현했다면?

바로 이런 차이가 성능 저하의 원인이 될 것이다.

`methods` 는 **값의 캐싱없이 화면 렌더링이 일어날 때마다 의도 하지 않는 함수 호출이 발생할 수 있다.**



👉 *무거운 로직이나 값을 캐싱하여 재사용이 필요하다면 적절히 Computed 속성으로 구현하자.*





참고자료)

https://sso-feeling.tistory.com/676

https://kbcoding.tistory.com/entry/methods-%EC%99%80-computed-%EC%9D%98-%EC%B0%A8%EC%9D%B4

https://velog.io/@reasonz/2022.06.09-Vue-computed%EC%99%80-methods-%EC%B0%A8%EC%9D%B4%EC%A0%90

https://sunny921.github.io/posts/vuejs-computed-method/

