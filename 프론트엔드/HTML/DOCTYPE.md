### DOCTYPE

✅ Document Type의 약자로, **HTML이 어떤 버전으로 작성되었는지 미리 선언하여 웹브라우저가 내용을 올바로 표시할 수 있도록 해주는 것** 이다 = 문서 형식 선언

- `<!DOCTYPE>` 으로 선언하는데 이걸 해주지 않으면 **호환 모드(quirks mode)** 로 동작한다. 호환 모드의 경우 각 브라우저마다 문서를 나타내는 방식이 다르기 때문에 크로스 브라우징 이슈가 훨씬 심해지게 된다.

- HTML 태그는 아니지만, 선언된 페이지의 HTML 버전이 무엇인지를 웹 브라우저에 알려주는 역할을 하는 선언문으로, 대소문자를 구분하지 않는다.
- HTML 4.01에서 DOCTYPE 선언은 SGML을 기반으로 하기 때문에 DTD를 참조해야 한다. 하지만 HTML5는 SGML을 기반으로 하지 않기 때문에 DTD를 참조할 필요가 없다.

❓**DTD(Document Type Definition)**란 ?

✅ 브라우저가 콘텐츠를 정확하게 표현하도록 마크업 언어에 대한 규칙을 명시 = *문서 형식 정의*

즉, HTML 문서가 어떤 문서 형식을 따르는지 DOCTYPE에서 DTD를 지정하는 것이다!

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

