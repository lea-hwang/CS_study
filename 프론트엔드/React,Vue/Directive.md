# Directive

`v-` 접두사가 있는 특수한 속성



### v-text

: 엘리먼트의 텍스트 컨텐츠를 업데이트한다.

```vue
<span v-text="msg"></span>
<!-- 아래와 같음 -->
<span>{{msg}}</span>
```

### v-html

: 엘리먼트의 innerHTML을 업데이트한다.

```vue
<div v-html="html"></div>
```

❗XSS 공격의 대상이 될 가능성이 있기 때문에 유의해야 한다.

### v-show

: 표현식의 thuthy 값을 기반으로 엘리먼트의 가시성을 전환한다.(display css 속성만 전환하기 때문에 항상 렌더링 된다.)

```vue
<h1 v-show="ok">안녕!</h1>
```

### v-if / v-else / v-else-if

표현식이 truthy 한지에 따라 엘리먼트 또는 템플릿 일부를 조건부로 렌더링한다. (조건이 true가 될 때까지 렌더링이 되지 않는다.)

```vue
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  A/B/C 아님
</div>
```

> 일반적으로 `v-if`는 전환 비용이 더 높고, `v-show`는 초기 렌더링 비용이 더 높다. 따라서 매우 자주 전환해야 하는 경우 `v-show`를, 실행 중에 조건이 변경되지 않는 경우 `v-if`를 사용하는 것이 좋다.



### v-for

소스 데이터를 기반으로 엘리먼트 또는 템플릿 블록을 여러 번 렌더링한다.

- 디렉티브는 반복되는 과정의 현재 값에 별칭을 제공하기 위해, 특수 문법인 `alias in expression`(표현식 내 별칭)을 사용해야 한다.

```vue
<div v-for="item in items">
  {{ item.text }}
</div>
```

- 인덱스(객체에서 사용되는 경우 키)의 별칭을 지정할 수도 있다.

  ```vue
  <div v-for="(item, index) in items"></div>
  <div v-for="(value, key) in object"></div>
  <div v-for="(value, name, index) in object"></div>
  ```

- 강제로 엘리먼트를 재정렬하려면, 특수 속성 `key`를 사용하여 순서 지정을 위한 힌트를 제공해야 한다.

  ```vue
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

### v-on

: 엘리먼트에 이벤트 리스너를 연결한다.

```vue
<!-- 메서드 핸들러 -->
<button v-on:click="doThis"></button>

<!-- 동적 이벤트 -->
<button v-on:[event]="doThis"></button>

<!-- 인라인 표현식 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 단축 문법 -->
<button @click="doThis"></button>

<!-- 단축 문법 동적 이벤트 -->
<button @[event]="doThis"></button>

<!-- 전파 중지 -->
<button @click.stop="doThis"></button>

<!-- event.preventDefault() 작동 -->
<button @click.prevent="doThis"></button>

<!-- 표현식 없이 event.preventDefault()만 사용 -->
<form @submit.prevent></form>

<!-- 수식어 이어서 사용 -->
<button @click.stop.prevent="doThis"></button>

<!-- 키 별칭을 수식어로 사용 -->
<input @keyup.enter="onEnter" />

<!-- 클릭 이벤트 단 한 번만 트리거 -->
<button v-on:click.once="doThis"></button>

<!-- 객체 문법 -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

[참고](https://ko.vuejs.org/api/built-in-directives.html#v-on)



### v-bind

: 하나 이상의 속성 또는 컴포넌트 prop을 표현식에 동적으로 바인딩한다.

`:` 또는 `.`(.prop 수식어를 사용할 때)를 사용해서 단축할 수 있다.

```vue
<!-- 속성 바인딩 -->
<img v-bind:src="imageSrc" />

<!-- 동적인 속성명 -->
<button v-bind:[key]="value"></button>

<!-- 단축 문법 -->
<img :src="imageSrc" />

<!-- 단축 문법과 동적 속성명 -->
<button :[key]="value"></button>

<!-- 인라인으로 문자열 결합 -->
<img :src="'/path/to/images/' + fileName" />

<!-- class 바인딩 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]"></div>

<!-- style 바인딩 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- 속성을 객체로 바인딩 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- prop 바인딩. "prop"은 자식 컴포넌트에서 선언되어 있어야 함 -->
<MyComponent :prop="someThing" />

<!-- 자식 컴포넌트와 공유될 부모 props를 전달 -->
<MyComponent v-bind="$props" />
```



### v-model

: 사용자 입력을 받는 폼(form) 엘리먼트 또는 컴포넌트에 양방향 바인딩을 만든다.

- `<input>`, `<select>`, `<textarea>`, 컴포넌로 제한되어 사용된다.

```vue
<input
  :value="text"
  @input="event => text = event.target.value">
// v-model 사용
<input v-model="text">
```



### v-slot

: 이름이 있는 슬롯 또는 props를 받을 것으로 예상되는 범위형 (Scoped) 슬롯을 나타낸다.

`#`을 사용해 단축할 수 있다. 

```vue
<!-- 이름이 있는 슬롯 -->
<BaseLayout>
  <template v-slot:header>
    해더 컨텐츠
  </template>

  <template v-slot:default>
    기본 슬롯 컨텐츠
  </template>

  <template v-slot:footer>
    푸터 컨텐츠
  </template>
</BaseLayout>

<!-- props를 수신할 기본 슬롯 -->
<InfiniteScroll>
  <template v-slot:item="slotProps">
    <div class="item">
      {{ slotProps.item.text }}
    </div>
  </template>
</InfiniteScroll>

<!-- props를 수신할 기본 슬롯, 분해할당을 사용 -->
<Mouse v-slot="{ x, y }">
  마우스 위치: {{ x }}, {{ y }}
</Mouse>
```



### v-pre

이 엘리먼트와 모든 자식 엘리먼트의 컴파일을 생략한다.

❗표현식을 허용하지 않는다.

```vue
<span v-pre>{{ 이곳은 컴파일되지 않습니다. }}</span>
```



### v-once

: 엘리먼트와 컴포넌트를 한 번만 렌더링하고, 향후 업데이트를 생략한다.

❗표현식을 허용하지 않는다.

> 이후 다시 렌더링할 때 엘리먼트/컴포넌트 및 모든 자식들은 정적 컨텐츠로 처리되어 생략된다. 이것은 업데이트 성능을 최적화하는 데 사용할 수 있다.

```vue
<!-- 단일 엘리먼트 -->
<span v-once>절대 바뀌지 않음: {{msg}}</span>
<!-- 자식이 있는 엘리먼트 -->
<div v-once>
  <h1>댓글</h1>
  <p>{{msg}}</p>
</div>
<!-- 컴포넌트 -->
<MyComponent v-once :comment="msg"></MyComponent>
<!-- `v-for` 디렉티브 -->
<ul>
  <li v-for="i in list" v-once>{{i}}</li>
</ul>
```



### v-memo

: 템플릿의 하위 트리를 메모한다.(배열의 모든 값이 마지막 렌더링과 같으면 전체 하위 트리에 대한 업데이트를 생략한다.)

엘리먼트와 컴포넌트 모두에 사용할 수 있다. 

```vue
<div v-memo="[valueA, valueB]">
  ...
</div>
```



### v-cloak

준비될 때까지 컴파일되지 않은 템플릿을 숨기는 데 사용한다.

> DOM 내 템플릿을 사용할 때, "컴파일되지 않은 템플릿이 순간 보이는 현상"이 있을 수 있다. 이러면 사용자는 컴포넌트가 렌더링된 컨텐츠로 대체할 때까지 이중 중괄호 태그를 볼 수 있다. 이를 방지하기 위해 준비되지 않은 템플릿을 숨기기 위해 사용한다.

❗표현식을 허용하지 않는다.

```vue
<div v-cloak>
  {{ message }}
</div>
```

```css
[v-cloak] {
  display: none;
}
```



**참고자료**

https://ko.vuejs.org/api/built-in-directives.html