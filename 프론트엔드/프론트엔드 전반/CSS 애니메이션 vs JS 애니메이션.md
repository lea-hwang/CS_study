# CSS 애니메이션 vs JS 애니메이션

많은 웹사이트에서 애니메이션 효과를 부여할 때 **CSS의 `transition/animation` 속성**을 사용할 수 있고

**JavaScript의 `setInterval()/requestAnimationFrame()`** 을 사용할 수 있다.

각각을 사용할 때의 특징이 다르고 장단점이 다르다.



## 🔎 CSS 애니메이션

**간단**하게 처리하는 애니메이션의 경우 CSS로 처리한다.

`transform` 의 `translate`(CSS) 를 사용해서 구현할 수 있는 애니메이션을 JavaScript의 `style.top` 과 `style.left` 속성을 변화시키게 되면

브라우저 렌더링 과정에서 layout 이나 paint 단계를 거쳐야 할 경우가 생길 수 있기 때문에 성능 개선에 효율적이지 않을 수 있다.

> `translate()` : 가로 및/또는 세로 방향으로 재배치
>
> https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translate



### CSS 애니메이션 특징

- 외부 라이브러리를 필요로 하지 않는다.

- 어떤 요소가 애니메이션을 가져야 한다는 **직관적인 표현**이 가능하다.

- 미디어쿼리를 사용해서 **반응형**으로 애니메이션을 구현 할 수 있다.

- 메인 스레드가 아닌 별도의 컴포지터 스레드(Compositor Thread)에서 그려지기 때문에 메인 쓰레드에서 작업하는 **JS보다 효율적**이다.

  > `메인 스레드(Main Thread)` : JS 실행, DOM 엘리먼트 렌더링 담당. 주어진 작업이 끝나기전 다른작업이 모두 블락된다. 
  >
  > `컴포지터 스레드(Compositor Thread)` : GPU를 이용해 렌더링된 엘리먼트를 화면에 그리는 역할. 특정부분이 블락되면 해당 영역을 빈 부분으로 대체한 후 다른영역을 진행한다.



## 🔎 JavaScript 애니메이션

CSS로 처리하기에는 훨씬 **복잡하고 무거운 애니메이션 작업**들을 효율적이고, 세밀하게 다루기 위해 사용합니다.

바닐라 자바스크립트로 구현할 경우 계속 요소의 위치를 재계산하기 때문에 비효율적이고, 사람들의 눈에 가장 부드러운 60fps가 유지되지 않는다. 

이 때문에 `RAF(RequestAnimationFrame)` 가 등장! 동일한 구현 방식으로 60fps를 달성시킬 수 있다.

> `RAF(RequestAnimationFrame)` : 렌더링 되기전에 수행되어야 하는 콜백을 지정하는 Web API

또한 외부 라이브러리나  Web Animations API(지원브라우저가 적다)를 통해 성능 좋은 애니메이션을 구현 할 수 있다.



### JavaScript 애니메이션 특징

- 애니메이션을 **세밀하게 제어**해야 하는 경우 JS를 사용한다.

- **크로스 브라우징** 측면에서 JS 애니메이션을 사용하는 것이 유리하다.(브라우저 호환성이 좋다.)

  > `크로스 브라우징` : 웹 페이지 제작 시에 모든 브라우저에서 깨지지 않고 의도한 대로 올바르게(호환성) 나오게 하는 작업. HTML, CSS, Javascript 작성 시 W3C의 웹 규격에 맞는 코딩을 함으로써 어느 브라우저, 기기에서 사이트가 의도된 대로 보여지고 작동되는 기법.

- GPU를 통한 하드웨어 가속을 제어할 수 있다. CSS 애니메이션의 경우 특정 속성에 의한 GPU가속이 됨으로서 저사양의 컴퓨팅인 경우에 성능 하락을 발생시킬 수 있으나 이를 막을수 있다.


참고하면 좋을 자료
https://flaviocopes.com/requestanimationframe/
