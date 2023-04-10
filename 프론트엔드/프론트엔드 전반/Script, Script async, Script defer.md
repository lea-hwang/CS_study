# Script, Script async, Script defer

`<script>`: HTML 파싱이 중단되고 즉시 스크립트가 로드되며 로드된 스크립트가 실행되고 파싱이 재개된다.

`<script async>`: HTML 파싱과 병렬적으로 로드되는데, 스크립트를 실행할 때는 파싱이 중단된다. 구글 애널리틱스와 같이 **다른 스크립트가 의존하지 않는 독자적인 스크립트를 로드**할 때 적합하다.

`<script defer>`: HTML 파싱과 병렬적으로 로드되는데, 파싱이 끝나고 스크립트를 로드한다. 보통 `<body>` 태그 직전에 `<script>`를 삽입하는 것과 동작은 같지만 브라우저 호환성이 다를 수 있으므로 그냥 `<body>` 태그 직전에 삽입하는 것이 좋다.

❗주의할 점은 async와 defer의 경우 `src` 속성이 없으면 적용되지 않는다❗

![img](https://github.com/baeharam/Must-Know-About-Frontend/raw/main/images/html/script.png)