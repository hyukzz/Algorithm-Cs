## 콜백 함수

자바스크립트는 비동기 처리를 위한 패턴으로 콜백 함수라는 것을 사용한다.

하지만, 전통적인 콜백 함수는 ‘콜백 지옥’ 으로 인해서 가독성도 나쁘고,

비동기 처리 중 발생한 에러를 처리하기가 곤란하며, 여러 개의 비동기 처리를

한번에 처리하기도 어렵다.

![](https://velog.velcdn.com/images/richard/post/95bfeeee-e5a7-4d3b-b986-85027780b2ae/image.png)
(콜백 함수의 콜백 지옥)

## 콜백 지옥

콜백 지옥이란, 비동기 로직을 처리할 때

콜백 함수를 계속해서 사용하게 될 때 발생하게 되는 문제이다.

```
// XMLHttpsRequest를 이용한 GET 요청 비동기 함수
const get = url => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      callback(JSON.parse(xhr.response));
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};


// id가 1인 post를 획득
get(`${url}/posts/1`, ({ userId }) => {
  console.log(userId); // 1
  // userId를 이용해 userInfo를 획득
  get(`${url}/users/${userId}`, (userInfo) => {
    console.log(userInfo); // {id: 1, name: "nam hun", age: 30, ...}
  }
});
```

위 예제에서 GET요청을 통해 서버로부터 받은 응답을 이용해 또 다시 GET 요청을 한다.

이게 반복되면, 콜백 지옥이 발생한다.

전형적인 콜백 지옥의 예제를 보자.

```
jsx
get('/step1', (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        get(`/step5/${d}`, (e) => {
          get(`/step6/${e}`, (f) => {
            get(`/step7/${f}`, (g) => {
              get(`/step8/${g}`, (h) => {
                get(`/step9/${h}`, (i) => {
                  get(`/step10/${i}`, (j) => {
                    console.log(j);
                  });
                });
              });
            });
          });
        });
      });
    });
  });
});
```

## 프로미스

```jsx
// 프로미스 생성
const promise = new Promise(resolve, reject) => {
  if (//비동기 처리 성공) {
    resolve('result');
  } else { // 비동기 처리 실패
    reject('err');
}
```

기본적인 프로미스 포맷이다.

Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행하고,

비동기 처리가 성공하면 인수로 전달받은 resolve함수를 호출,

비동기 처리가 실패하면 reject 함수를 호출한다.

이 때 비동기 처리가 성공한 상태를 fulfilled, 실패한 상태를 rejected라고 한다.

fulfilled, rejected 상태에 따라 후속 과정을 진행해야 하는데,

then, catch, finally 메소드를 이용해 진행한다.

- then

then 메소드는 2개의 콜백 함수를 인수로 전달받는다.

- 첫번째 콜백 함수는 프로미스가 fullfilled (성공) 상태가 되면 호출된다.
  비동기 처리 결과를 인수로 전달받는다.
- 두번째 콜백 함수는 프로미스가 rejected (실패) 상태가 되면 호출된다.
  프로미스의 에러를 인수로 전달받는다.

즉, 첫번째 콜백 함수는 비동기 처리가 성공했을 때 호출,

두번째 콜백 함수는 비동기 처리가 실패했을 때 호출되는 콜백 함수이다.

```jsx
new Promise(resolve => resolve("fulfilled")).then(
	v => console.log(v),
	e => console.error(e)
); // fulfilled
```

- catch

catch 메소드는 then과 동일하게 동작한다.

프로미스가 rejected 상태인 경우에만 호출된다.

```jsx
new Promise((_, reject) => reject(new Error("rejected"))).catch(e =>
	console.log(e)
); // Error: rejected`
```

## 콜백 함수와 프로미스의 차이

- 콜백 함수는 비동기 처리의 결과를 콜백 함수 내부에서만 처리가 가능하고,

콜백 함수 밖에서는 결과의 값을 알 수 없다.

(콜백 함수 내부에서 외부의 변수 값을 바꿀 수도 없다)

- 프로미스는 비동기 처리의 결과가 프로미스 객체에 저장되므로 결과의 값을 알 수 있다.
