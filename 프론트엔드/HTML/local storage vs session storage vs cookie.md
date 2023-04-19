# local storage vs session storage vs cookie

## ❓ local storage, session storage, cookie 왜 사용할까?

*지속적인 데이터 교환을 위해 사용한다.* 

HTTP는 HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜이다. HTTP는 웹에서 이루어지는 모든 데이터 교환의 기초가 된다. 이 HTTP프로토콜의 특징은 클라이언트가 서버에게 Request를 보내고 서버가 클라이언트에게 Response를 보내면 접속을 종료하며 통신이 끝나면 상태 정보를 유지하지 않는다. 즉, 클라이언트의 로그인 정보나 브라우저에서 입력한 값 등이 페이지를 이동할 때 마다 초기화 된다. 이러한 문제점을 해결하기 위해 데이터 저장에 사용한다.



## 🔎 local storage

클라이언트의 컴퓨터에 데이터를 저장하는 방법. 쿠키와 유사하지만 *일반적으로 키-값 쌍으로 구조화된 방식으로 데이터를 저장*.

![img](https://blog.kakaocdn.net/dn/Gvupq/btqN8ewVfz3/gxIabeCDzA3gk7U58SWpz1/img.png)

- key/value 의 pair로 데이터를 저장한다.
- Javascript/HTML5 을 통해서만 데이터에 접근 가능하다.

- no expiration date. 직접 지울때까지 남아있다.
- 5MB의 저장 공간을 가진다.
- Local Storage의 데이터는 api 호출에 담을 수 없어 서버에 전송이 불가능하다. (= client 에서만 저장 데이터 조회 가능)

- string data로 저장이 제한된다. 따라서 용이하게 사용하려면 직렬화(String화) 가 필요하다.

  > `직렬화` : *데이터 구조나 오브젝트 상태를 동일하거나 다른 컴퓨터 환경에 저장(이를테면 파일이나 메모리 버퍼에서, 또는 네트워크 연결 링크 간 전송)하고, 나중에 재구성할 수 있는 포맷으로 변환하는 과정*. 현재 데이터(structure, object)의 상태를 영속적으로 저장하거나 다른 환경으로 전달(네트워크 통신 등)하기 위해 어떠한 정해진 포맷으로 변환하는 과정.



## 🔎 session storage

로컬 저장소와 유사하지만 *현재 세션에 대한 데이터만 저장하고 브라우저를 닫으면 삭제*된다.

보안이 취약하다는 쿠키의 한계점을 극복하기 위해 사용한다. 쿠키를 기반으로 하여 동작하기는 하지만 사용자 정보를 클라이언트 측이 아닌 **서버 측에서 관리**한다는 점이 다르다. **클라이언트는 서버로부터 서버에서 관리하고 있는 세션 정보를 찾기 위한 세션 ID만 전달받는다**. 세션 정보를 저장하는 장소는 서버 메모리일수도 있지만 다중 서버 환경에서는 외부 저장소를 사용한다.

![img](https://blog.kakaocdn.net/dn/bHdh4T/btqN6UlrjUs/O5HqVubyqClkfAgBrqaUK0/img.png)

- session 기간에만 데이터를 저장한다. 즉, browser(또는 tab)이 꺼진다면 데이터는 소실된다. (보안 측면에서 유리)
- 5-10 MB의 저장 공간을 가진다.
- Session Storage의 데이터는 api 호출에 담을 수 없어 서버에 전송이 불가능하다. (= client 에서만 저장 데이터 조회 가능)
- 같은 주소의 URL의 창을 여러개 열어도 각각의 창은 별도의 Session Storage를 갖는다.

※ 일반적으로 HTTP Reqeust/Response 에서 말하는 Session과는 다른 개념이다.



## 🔎 cookies

`쿠키` : 클라이언트에 대한 정보를 이용자의 PC의 하드디스크에 보관하기 위해서 웹 사이트에서 클라이언트의 웹 브라우저에 전송하는 정보. 일반 텍스트로 *클라이언트 측 컴퓨터에 저장되는 작은 데이터 조각*. 브라우저는 그 데이터 조각들을 저장해 놓았다가, 동일한 서버에 재요청 시 저장된 데이터를 함께 전송한다.

![img](https://blog.kakaocdn.net/dn/5Oq88/btqOcQu3sSm/rOWuipkdkkjXgNZSG0kIA0/img.png)

- expiration date는 각 데이터마다 설정된 기간동안으로 지정된다.

- 4KB 이하의 저장공간을 가진다.

- Server-Side에서 사용되는 데이터를 주로 저장한다.

- 매 api 요청마다 함께 전송된다.(header에 Access-Control-Allow-Credentials를 true로 설정 시)

- HttpOnly flag를 통해 각 Cookie를 client-side에서의 접근으로부터 보호할 수 있다. (document.cookie 로 직접 실험가능)

  > `HttpOnly flag` : Set-Cookie HTTP 응답 헤더에 포함된 *추가 플래그*. 쿠키를 생성할 때 HttpOnly 플래그를 사용하면 클라이언트 측 스크립트가 보호된 쿠키에 액세스하는 위험을 완화하는 데 도움이 된다(브라우저에서 지원하는 경우).



## 🪄 정리

![img](https://blog.kakaocdn.net/dn/bctds1/btqN6Ur75PI/ZGrar9rXQv9EIepjXWfTeK/img.png)