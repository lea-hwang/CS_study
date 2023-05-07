# re-rendering 발생 조건

## React

React는 **컴포넌트의 상태**가 변경될 때마다 렌더링을 예약한다. (렌더링 **예약**은 이 작업이 즉시 수행되지 않는다는 것을 의미한다. )

**상태**를 변경한다는 것은 `setState` 함수(React hooks에서는 `useState`)를 실행할 때, React 트리거가 업데이트된다는 것을 의미한다. 이는 컴포넌트의 render 함수가 호출된다는 것을 의미할 뿐만 아니라, **props 변경 여부와 관계없이 모든 하위 컴포넌트들이 리렌더링 된다는 것을 의미한다.** (대체 방법 => `React.memo()`)

## Vue

**반응성(Reactivity)**: Vue는 JavaScript 상태(State) 변경을 추적하고, 변경이 발생하면 DOM을 효율적으로 업데이트하는 것을 자동으로 수행한다

### option API

```vue
<script>
export default {
  props: {
    title: String,
    likes: Number
  },
  data() {
    return {
      count: 0
    }
  },
}
</script>

<template>
  <button @click="increment">숫자 세기: {{ count }}</button>
</template>
```

- 옵션 API에서는 `data` 옵션을 사용하여 컴포넌트의 반응형 상태를 선언한다. 이러한 반응 상태를 변경하면 DOM이 자동으로 업데이트한다.

- 부모 컴포넌트가 업데이트될 때마다 자식 컴포넌트의 모든 props가 최신 값으로 업데이트 된다.

### Composition API

```vue
<script setup>
  import { ref, reactive } from 'vue'

  // props
  const props = defineProps(['foo'])
  // 반응적인 상태의 속성
  const count = ref(0)
  // 객체를 반응형으로 만들기
  const state = reactive({ count: 0 })

</script>

<template>
  <button @click="increment">숫자 세기: {{ count }}</button>
</template>
```

- [`reactive()`](https://v3-docs.vuejs-korea.org/api/reactivity-core.html#reactive) 함수를 사용하여 객체, 배열, Map, Set과 같은 컬렉션 유형을 반응형으로 만들 수 있다.

- 어떠한 유형의 데이터라도 반응형으로 재정의할 수 있는 [`ref()`](https://v3-docs.vuejs-korea.org/api/reactivity-core.html#ref) 함수를 제공한다

- 부모 컴포넌트가 업데이트될 때마다 자식 컴포넌트의 모든 props가 최신 값으로 업데이트 된다. 



**참고 자료**

https://velog.io/@surim014/react-rerender
