# Block Formatting Context

*웹 페이지의 시각적 CSS 렌더링의 일부로서,*

*블록 박스의 레이아웃이 발생하는 지점과 플로팅 요소의 상호작용 범위를 결정하는 범위*.

요소에 새로운 블록 양식화 문맥(이하 BFC)을 적용하면 **하위 요소를 대상으로 한 독립적인 레이아웃 환경**을 만들수 있고 **다른 주변 요소와도 레이아웃 관계를 형성**할 수 있다.



## 🔎 생성 조건

1. **문서의 루트 요소일 때**

`<html>`은 웹 문서의 시작을 알리는 최상위 요소로, 하위 요소들이 독립적인 레이아웃을 가질 수 있도록 되어 있다.



2. **플로팅(Floating) 되었을 때**

`float` 이 none이 아닌 속성값을 가질 때, 해당 element는 normal flow를 벗어나 떠 있는 부동 요소가 되면 BFC가 생성된다.

> `normal flow` : 내가 요소의 레이아웃을 변경하지 않았을 시 웹페이지 요소가 자기 자신을 스스로 배치하는 방법

예제)

- float 속성을 선언하고 개발자도구를 확인해보면 display가 inline에서 block으로 변경되는 것을 볼 수 있다.

```html
<a href="/">a태그</a>
a {
	float: left;
}
```

![img](https://velog.velcdn.com/images/parkseonup/post/e290f1ae-0473-46f2-9679-bc1fe31ff8cf/image.png)



3. **`position` 속성값이 `absolute`나 `fixed` 일 경우**

요소가 형제와 조상의 크기나 위치에 영향을 주지 않고 normal flow에서 벗어남으로 BFC를 형성하게 된다.



4. **`display: inline-block`**

inline-block은 inline 요소처럼 행동하는 또 다른 block 요소로, BFC를 형성하게 된다.

> `inline-block` : `display` 속성이 `inline-block`으로 지정된 엘리먼트는 기본적으로 `inline` 엘리먼트처럼 전후 줄바꿈 없이 *한 줄에 다른 엘리먼트들과 나란히 배치*되지만, `block` 엘리먼트처럼 *`width`와 `height` 속성 지정 및 `margin`과 `padding` 속성의 상하 간격 지정이 가능*하다. 즉, 내부적으로는 `block` 엘리먼트의 규칙을 따르면서 외부적으로 `inline` 엘리먼트의 규칙을 따른다.



5. **표**

- 칸: `display: table-cell`. HTML 표 칸의 기본값

- 주석: `display: table-caption`. 표 주석의 기본값

- 표 전체: `display: table`

- 행: `display: table-row`

- 본문: `display: table-row-group`

- 헤더: `display: table-header-group`

- 푸터: `display: table-footer-group`

- `display: inline-table` 요소가 암시적으로 생성한 무명 칸

  

6. **`overflow`가 선언된 요소 (visible 제외)**

overflow가 visible 이외의 값을 가진다는 것은 공간에 대한 크기를 정의했다고 볼 수 있기 때문에 BFC를 형성하게 된다.

> `overflow` : 요소의 콘텐츠가 너무 커서 요소의 BFC에 맞출 수 없을 때의 처리법을 지정한다.
>
> https://developer.mozilla.org/ko/docs/Web/CSS/overflow



7. **기타**

- display가 flow-root인 경우

  > `flow-root` : `div` 에 요소상에 `display: flow-root`을 적용하면, 컨테이너 내부의 모든 요소는 해당 컨테이너의 BFC에 참여하게 되며, floats는 같은 요소 밑의 밖으로 돌출하지 않게 된다.
  >
  > `flow-root`는 (마치 <html>의 경우처럼) 본질적으로 새로운 root 요소와 같은 기능하는 어떤 것을 생성한다는 사실을 말해준다. 
  >
  > `overflow`나 `clearfix`와 유사. (overflow: hidden;을 사용하여 영역을 잡을 수 있지만 불안정. 이때, float을 사용한 상위(부모)박스에 .clearfix를 설정하면 float 으로 인하여 영역이 깨지는 현상을 방지하게 된다. 결국 .clearfix는 영역을 잡아주기 위한 css 속성.)
  >
  > https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Flow_Layout/Intro_to_formatting_contexts

- contain가 layout, content, paint일 경우

  > [contain](https://developer.mozilla.org/en-US/docs/Web/CSS/contain)

- flex 아이템이나 grid 아이템의 경우

  

## 🔎 특성

- 마진 상쇄를 막을 수 있다.
- 컨테이너가 페이지의 normal flow에서 벗어나 떠 있고 높이가 없는, 부동 요소인 자식 요소를 포함하도록 확장시켜준다. (float, position: absolute의 경우)
- float된 요소를 감싸는 텍스트를 분리시킬 수 있다.