# Vue2 vs Vue3

- Vue 3는 Vue 2에 비해 더 빠르고, 더 작고, 유지 관리가 더 쉽다.
- Vue 3는 Vue 2의 재작성 버전이며 몇 가지 새롭고 중요한 변경 사항과 기능이 함께 제공된다.
- vue2와 vue3의 기본 구문은 매우 유사하다.

Vue2에서는 코드가 커짐에 따라 구성 요소의 가독성이 떨어지고 유지 관리가 어려워진다. 

Vue3에서 가장 중요한 추가 기능은 이 문제를 해결하기 위해 도입된 **Composition API **이다. 

Composition API로 컴포넌트 코드는  **Options API** 기반의 여러 컴포넌트 옵션에서 Vue2의 분산 코드 세그먼트와 달리 논리적 문제를 기반으로 구성될 수 있다 .



##  ✨ Vue3의 새로운 변화

Vue3에 도입된 몇 가지 주요 새로운 변경 사항

1. Composition API
2. Vue life cycle의 변화
3. Teleport
4. Fragment
5. 초기화 코드(Initialization code)



## 🔎 Composition API

*코드 재사용성을 높이기 위한 Composition API* 

Vue3로 올라가면서 `Composition API`가 core로 적용되었다. 기존과 달리 setup 함수 안에서 커스텀 하게 코드를 작성하도록 바뀌었다. Vue2는 Vue에서 제공하는 API로 구분 지어 코드를 작성했다.

#### Vue2

```vue
...
export default {
 props: {
  user: {
   type: String,
    required: true
  }
 },
 data() {
  return {
   state: {
    type: 'compositon'
   }
  }
 },
 created() {
  console.log(this.user)
 }
}
```

논리에 관계없이 다양한 코드 세그먼트가 여러 구성 요소 옵션에 흩어져 있다. 컴포넌트가 커진 경우 코드를 읽고 이해하기 어렵게 만든다.

#### Vue3

```vue
...
export default {
 props: {
  user: {
   type: String,
   required: true
  }
 },
 setup(props) {
  const state = reactive({
   type: 'compositon'
  })
  console.log(props.user)
  return {
   state
  } 
 }
}
```

setup 내에서 lifecycle methods, computed properties, state data 등을 가질 수 있으므로 논리적 문제로 코드 세그먼트를 그룹화할 수 있다.



## 🔎 Vue life cycle의 변화

life cycle hook는 Vue2 및 Vue3에서 유사하게 작동하지만 Composition API를 사용하면 이러한 수명 주기 후크에 대한 액세스가 다음과 같이 변경되었다.

![img](https://miro.medium.com/v2/resize:fit:645/1*ebI7VwCyxFPksX_ZULJ5Rw.png)

**beforeCreate** 및 **created** life cycle hook는 **setup()이** 이를 대체하고 정확한 기능을 수행하므로 필요하지 않다 . 그리고 이러한 라이프 사이클 후크를 사용하기 전에 가져오는 것이 중요하다.

예를 들어 **onBeforeMount는** 다음과 같이 작성된다.

```vue
import { onBeforeMount } from 'vue' 
export default { 
  setup() { 
    // mounted 
    onBeforeMount (() => { 
      // 구현 
    }) 
  } 
}
```

즉, 모든 라이프 사이클 후크와 그 내용은 **setup()** 안에 있어야 한다 .



## 🔎 Teleport

알림 창이나 경고 창, 모달을 그릴 때 렌더링 하는 DOM이 아닌 **특정 DOM을 연결하여 호출하도록** 바뀌었다. 

Vue2는 모달을 관리하는 컴포넌트를 생성해서 각각 페이지단 뷰에 연결해 줘야만 했다. 위치에 따라 하나하나 관리를 해줘야 하는 영역이라 번거로웠다. 

이제는 특정 DOM으로 연결함으로써 위 문제를 해결할 수 있다. 

Vue2에서 이 문제를 피하려면 [`portal-vue`](https://github.com/LinusBorg/portal-vue) 라이브러리를 이용해야만 한다. (리액트의 Portal 같은 기능)

#### Vue2

```
<template>
 <div id="page-wrap">
  <div id="page-teleport">
   <div>modal</div>// 모달 창 마크업 및 스타일링
  </div>
 </div>
</template>
...
```

#### Vue3

```
<!DOCTYPE html>
<html lang="ko">
 <body>
  <div id="app"></div>
  <div id="teleport"></div>
 </body>
</html>
...
<teleport to="#teleport">
 <div>modal</div>// 모달 창 마크업 및 스타일링
</teleport>
```





## 🔎 Fragment

 Vue 템플릿 최상단을 하나의 태그가 묶지 않아도 된다. 

실제 DOM 트리에서 렌더링 되지 않는 `Fragment` 컴포넌트가 추가되어 여러 태그를 같은 선상에 배치할 수 있도록 변경되었다. 

Vue2는 템플릿에 하나의 DOM만 사용이 가능해서 불필요한 태그를 하나 더 삽입하였는데, 더 이상 그럴 필요가 없다.

#### Vue2

```
<template>
 <div id="wrap">
  <header>...</header>
  <main>...</main>
  <footer>...</footer>
 </div>
</template>
...
```

#### Vue3

```
<template>
 <header>...</header>
 <main>...</main>
 <footer>...</footer>
</template>
...
```



## 🔎 초기화 코드

Vue3에서는 앱을 초기화하기 위해 **createApp 메서드가 도입되었다.** 이 메서드는 Vue 앱의 새 인스턴스를 반환한다. 각 인스턴스는 다른 인스턴스에 영향을 주지 않고 자체 기능을 가질 수 있다.

```vue
const app1 = createApp({})
const app2 = createApp({})
app1.directive('focus', {
    inserted: el => el.focus()
})
app2.mixin({
    /* ... */
})
```

동일한 애플리케이션에서 여러 앱을 만드는 것은 일반적이지 않지만 **프로젝트 규모가 커질 때 유용**할 수 있습니다. 

Vue3를 사용하면 **각 앱을 독립적인 개체로 구성**할 수 있다. 여러 인스턴스 간에 일부 기능을 공유하는 것도 가능하다.





참고자료)

https://www.albiorixtech.com/blog/vue2-vs-vue3-with-top-differences/

https://moz1e.tistory.com/540

https://medium.com/emblatech/vue2-to-vue3-whats-changed-5572514da20d

https://saramin.github.io/2021-12-15-vue2-vs-vue3/