# Script, Script async, Script defer

<img src="assets/wfL82.png" alt="enter image description here" style="zoom:50%;" />

💡`<script>`

HTML 파싱이 중단되고 즉시 스크립트가 로드되며 로드된 스크립트가 실행되고 파싱이 재개된다.



💡`<script async>`

HTML 파싱과 **병렬**적으로 로드되는데, **스크립트를 실행할 때는 파싱이 중단**된다.(`DOMContentLoaded` 이벤트와 async 스크립트는 서로를 기다리지 않는다.) **다른 스크립트가 의존하지 않는 독자적인 스크립트를 로드**할 때 적합하다.

<img src="https://github.com/lea-hwang/CS_study/blob/master/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C/HTML/assets/images%252Fjiseong%252Fpost%252Fab7693fa-9129-4f7a-a20b-e20fb3307379%252Fimage-1683213343038-1.png?raw=true" alt="img" style="zoom: 50%;" />

다른 스크립트들은 `async` 스크립트를 기다리지 않는다. `async` 스크립트 역시 다른 스크립트들을 기다리지 않는다. 이런 특징 때문에 페이지에 `async` 스크립트가 여러 개 있는 경우, 그 실행 순서가 제각각이 된다. 실행은 다운로드가 끝난 스크립트 순으로 진행된다. 위치상으론 더 짧은 스크립트가 아래이고 긴 스크립트가 위에 있어도 짧은 스크립트가 **먼저 다운로드되었기 때문에 먼저 실행된다**. 이렇게 먼저 로드가 된 스크립트가 먼저 실행되는 것을 ⭐`load-first order`라고 부른다.

```html
<p>...스크립트 앞 콘텐츠...</p>

<script>
  document.addEventListener('DOMContentLoaded', () => alert("DOM이 준비 되었습니다!"));
</script>

<script async src="https://javascript.info/article/script-async-defer/long.js"></script>
<script async src="https://javascript.info/article/script-async-defer/small.js"></script>

<p>...스크립트 뒤 콘텐츠...</p>
```



💡`<script defer>`

HTML 파싱과 **병렬**적으로 로드되는데, **파싱이 끝나고 스크립트를 로드**한다. (그러나 [DOMContentLoaded](https://ko.javascript.info/onload-ondomcontentloaded) 발생 이전에 실행해야 함) 보통 `<body>` 태그 직전에 `<script>`를 삽입하는 것과 동작은 같지만 브라우저 호환성이 다를 수 있으므로 그냥 `<body>` 태그 직전에 삽입하는 것이 좋다.

> `<script />`에 type=module이 있다면 기본적으로 defer로 동작한다.

지연 스크립트는 일반 스크립트와 마찬가지로 HTML에 추가된 순(상대순, 요소순)으로 실행된다. 따라서 길이가 긴 스크립트가 앞에, 길이가 짧은 스크립트가 뒤에 있어도 **짧은 스크립트는 긴 스크립트가 실행될 때까지 기다린다**. 그래서 ⭐짧은 스크립트가 먼저 다운로드되어도 실행은 나중에 된다.

```html
<script defer src="https://javascript.info/article/script-async-defer/long.js"></script>
<script defer src="https://javascript.info/article/script-async-defer/small.js"></script>
```

>❗**스크립트 다운로드가 끝나지 않았어도 페이지는 동작해야 한다.**
>
>`defer`를 사용하게 되면 스크립트가 실행되기 *전* 에 페이지가 화면에 출력된다는 점에 항상 유의해야 한다.
>
>사용자는 그래픽 관련 컴포넌트들이 준비되지 않은 상태에서 화면을 보게 될 수 있다.
>
>따라서 지연 스크립트가 영향을 주는 영역엔 반드시 `로딩 인디케이터`가 있어야 한다. 관련 버튼도 사용 불가(disabled) 처리를 해줘야 한다. 이렇게 해야 사용자에게 현재 어떤 것은 사용할 수 있는지, 어떤 것은 사용할 수 없는지를 알려줄 수 있다.

💡`Dinamic script`: 자바스크립트를 활용해 스크립트를 동적으로 추가한 것을 의미한다. (*)이 실행되었을 때, 즉 외부 스크립트가 문서에 추가 되었을 때 다운로드가 실행된다.

```javascript
let script = document.createElement('script');
script.src = "/article/script-async-defer/long.js";
document.body.append(script); // (*)
```

> `script.async=false` 속성을 추가한 뒤, 문서에 해당 스크립트가 추가된다면, 이 스크립트는 `load-first order`로 실행된다. 

❗주의할 점은 async와 defer의 경우 `src` 속성이 없으면 적용되지 않는다❗



❓async나 defer script들은 어떻게 병렬적으로 처리되는 걸까❓

💡**Preload scanner**

브라우저가 DOM 트리를 만드는 프로세스는 메인 쓰레드를 차지한다. 그렇기 때문에, *프리로드 스캐너* 는 사용 가능한 컨텐츠를 분석하고 CSS나 Javscript, 웹 폰트 같이 우선순위가 높은 자원을 요청한다. 프리로드 스캐너 덕에 구문 분석기가 외부 자원에 대한 참조를 찾아 요청하기까지 기다리지 않아도 된다. 프리로드 스캐너가 자원을 뒤에서 미리 요청하기 때문에 구문 분석기가 요청되는 자원에 다다를 때 쯤이면 이미 그 자원들을 전송받고 있거나 이미 전송받은 후가 된다. 프리로드 스캐너가 제공하는 최적화는 블록킹을 줄여준다.

```html
<link rel="stylesheet" src="styles.css" />
<script src="myscript.js" async></script>
<img src="myimage.jpg" alt="image description" />
<script src="anotherscript.js" async></script>
```

![https://web-dev.imgix.net/image/jL3OLOhcWUQDnR4XjewLBx4e3PC3/6lccoVh4f6IJXA8UBKxH.svg](assets/6lccoVh4f6IJXA8UBKxH.svg+xml)



**참고 자료**

https://developer.mozilla.org/ko/docs/Web/HTML/Element/script

https://ko.javascript.info/onload-ondomcontentloaded

https://ko.javascript.info/script-async-defer

https://stackoverflow.com/questions/10808109/script-tag-async-defer

https://velog.io/@jiseong/HTML-DOMContentLoaded-load-async-defer

https://yceffort.kr/2022/06/preload-scanner

https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work
