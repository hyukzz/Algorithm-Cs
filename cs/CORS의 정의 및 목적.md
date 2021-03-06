## CORS (Cross Origin Resource Sharing)

CORS를 알기 전에 먼저 CORS의 `Cross-Origin` 이 무엇인지 알아보자.

1. 프로토콜
2. 도메인
3. 포트 번호

위의 3가지 중에, 어느 하나라도 다르면 `Cross-Origin` 이라고 한다.

```jsx
프로토콜 → http와 https는 다른 프로토콜이다.
도메인 → hello.com과 hi-hello.com은 다른 도메인이다.
포트 번호 → 3000포트와 8080포트는 다른 포트이다.
```

브라우저에서는 보안적인 이유로 이 `Cross-Origin` HTTP 요청들을 제한하는데,

`Cross-Origin` 요청을 하기 위해서는 서버의 동의를 얻어야 한다.

서버에서 동의할 시 브라우저에서 요청을 허락하고,

서버에서 동의하지 않으면 브라우저에서 요청을 거절한다.

이렇게 동의를 구하고, 요청을 허락하거나 거절하는 메커니즘을

CORS (Cross Origin Recource Sharing) 라고 부른다.

## CORS의 목적

CORS 없이 어느 곳에서든 데이터를 요청하고, 받아갈 수 있다면

우리가 허용하지 않은 다른 사이트에서 우리 사이트를 흉내낼 수 있게 된다.

이렇게 되면, 회원의 개인정보를 탈취하거나 잘못된 정보를 기입해 놓는 등의 공격을 당할 수 있다.

이런 공격으로부터 원천적으로 차단하고, 필요한 경우에 서버와의 Sharing을 통해

요청과 응답을 보내주기 위한 목적을 갖고 있다.

## Node.js에서의 CORS 설정

Node.js의 Express 기반 API서버 환경에서 CORS 설정을 하는법을 알아보자.

Acess-Control-Allow-Origin 헤더를 직관적이고 편리하게 설정해줄 수 있는 cors npm 모듈이 존재한다.

cors를 npm install 해준 뒤, 서버 app.js에서 cors를 require해주고, 변수에 할당해준다.

해당 변수의 옵션값으로 허용할 origin, credential 옵션, 허용할 HTTP method 등을 작성해 준다.

```jsx
// 예시

const cors = require("cors");
const app = express();

app.use(
  cors({
    origin: *, //모든 origin 허용
    credentials: true, // cookie 사용
    methods: ["GET", "POST", "OPTIONS", "PATCH"] // 허용할 HTTP Methods
  })
);
```
