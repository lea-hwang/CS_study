# data- 속성

HTML5 특정 요소와 연관되어 있지만 확정된 의미는 갖지 않는 데이터에 대한 확장 가능성을 염두에 두고 디자인되었다. `data-*` 속성은 표준이 아닌 속성이나 추가적인 DOM 속성, `Node.setUserData()`과 같은 다른 조작을 하지 않고도, 의미론적 표준 HTML 요소에 추가 정보를 저장할 수 있도록 해준다.

HTML의 데이터셋 속성은 **커스텀 사용자 속성을 DOM 요소에 저장하는데 표준화된 방법을 제공**한다. 한마디로 자바스크립트에서 변수를 사용하듯이, 일종의 **html의 변수 역할**이라고 말할 수 있다.

## HTML 문법

문법은 간단하다. 어느 엘리멘트에나 `data-`로 시작하는 속성은 무엇이든 사용할 수 있다. 화면에 안 보이게 글이나 추가 정보를 엘리멘트에 담아 놓을 수 있다. 

`data` 사용법:

```
<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars">
...
</article>
```



## 문제점

보여야 하고 접근 가능해야하는 내용은 데이터 속성에 저장하지 않아야 한다! 접근 보조 기술이 접근할 수 없기 때문. 또한 검색 크롤러가 데이터 속성의 값을 찾지 못할 것이다.

고려해야할 주요한 문제는 인터넷 익스플로러의 지원과 성능이다. 인터넷 익스플로러11+ 은 표준을 지원하지만, 이전 버전들은 [`dataset`을 지원하지 않습니다](http://caniuse.com/#feat=dataset). IE 10 이하를 지원하기 위해서는 대신 [`getAttribute()`](https://developer.mozilla.org/ko/docs/Web/API/Element/getAttribute)를 통해 데이터 속성을 접근해야 한다. 또한, JS 데이터 저장소에 저장하는 것과 비교해서 [데이터 속성 읽기의 성능](http://jsperf.com/data-dataset)은 저조하다.

하지만 이 때문에, 커스텀 요소와 관련된 메타 데이터를 위해서는 훌륭한 해결책이다.



참고

https://developer.mozilla.org/ko/docs/Learn/HTML/Howto/Use_data_attributes