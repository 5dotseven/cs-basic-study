## Node.js

---

**크롬 V8엔진으로 빌드된 javascript 런타임**

- V8은 크롬, 안드로이드 브라우저에 탑재된 C++ 기반 오픈소스 자바스크립트 엔진
    - 기존에 브라우저에 탑재되어 실행해야 했던 javascript 코드를 독립적으로 실행할 수 있게 해줌
- V8 엔진을 기반으로 빌드한 javascript 실행을 위한 프로그램

Javascript를 브라우저 한계점에서 벗어나 다양한 곳에서 사용할 수 있게 해준다.

**Node.js는 이벤트 기반, single thread non-blocking I/O 모델을 사용해 가볍고 효율적**

![img1 daumcdn](https://github.com/user-attachments/assets/1b447518-1da4-42ff-b2dd-a573e1117bb9)

- Non-blocking I/O
    - blocking I/O의 반대, 이전에 수행중인 I/O 작업의 완료를 기다리지 않고 프로세스에서 자원을 바로 사용하는 것
    - libuv 라이브러리에서 구현되며 내부에 이벤트 루프가 위치하며 이곳에서 일부 blocking 작업들이 수행된다.
- 이벤트 기반
    - 이벤트 발생 시 이벤트 리스너에 미리 등록된 콜백 함수로 작업을 처리
    - 이벤트 루프에서 콜백 함수들을 관리하게 된다. 이벤트 루프는 여러 개의 페이즈로 구성되며 각 페이즈마다 큐를 가진다.
    - 노드 프로세스가 종료될 때까지 페이즈들을 순회하며 각 페이즈는 FIFO에 따라 큐의 콜백함수들을 처리

**libuv**

- 비동기 작업에 중요한 라이브러리

    ![image](https://github.com/user-attachments/assets/497fd74a-2c91-432a-b164-248083b18f43)

  *코드 작성 및 실행 시 Node.js에서는 아래와 같은 순서로 처리가 일어난다.*

    1. js로 코드를 작성하고 실행하게 되면 스택에 코드가 쌓임.
    2. 이때 스택에 쌓인 코드를 실행하게 되면 libuv를 호출.
    3. libuv는 비동기 처리를 할지 , 동기 처리를 할지 검사 후, 시스템 API를 이용하거나 쓰레드 풀에 생성된 쓰레드에게 작업을 위임.
    4. 작업이 완료되면 콜백 함수를 테스크 큐에 넘김.
    5. 이벤트 루프는 콜스택에 쌓여있는 함수가 없을 때, 테스크 큐에 대기하고 있던 콜백함수를 콜스택으로 넘김.
    6. 콜스택에 쌓인 콜백 함수가 실행되고, 콜스택에서 제거
- 이벤트루프

    ![image](https://github.com/user-attachments/assets/edb349d4-4362-49c4-bede-714a715d243b)

    - 이벤트루프는 위와 같은 단계로 구성되며 이벤트 루프는 순서대로 호출이 된다.
    - 각 단계는 자신만의 큐를 가지며 이 큐에 사용자가 실행한 작업이 등록된다.
    - times
        - 타이머에 관한 비동기 작업. 예를 들어 setTimeout, setInterval에 등록된 콜백을 관리한다.
    - pending callbacks
        - 이전 이벤트 루프에서 마치지 못한 콜백을 적재하고 처리하는 단계
    - idle, prepare
        - 매틱마다 실행하며 node.js의 내부 관리를 위한 단계이다.
    - poll
        - 거의 모든 콜백이 위치하는 단계, 큐가 비어있을 경우 일정 시간 대기하거나 다음 단계로 넘어가게 된다.
        - DB의 쿼리 결과가 왔을 때 실행하는 콜백, HTTP 요청에 대한 응답이 왔을 때 실행되는 콜백 등
    - check
        - setImmediate()로 등록된 콜백을 관리한다.
    - close
        - close 이벤트에 따른 콜백함수를 관리

**poll에서의 별도 규칙**

poll 큐에 실행할 콜백 함수가 없다면 다음의 규칙에 따라 행동한다.

- check 단계 탐색하여 setImmediate 검색, 있을 경우 check로 이동
- timer 단계를 탐색하고 있을 경우 라운드 로빈 시간이 될때까지 대기한다.
- 대기 중 새로운 콜백 함수가  poll에 등록될 시 즉시 실행한다.

```jsx
setTimeout(() => {
  console.log('timeout');
}, 0);

setImmediate(() => {
  console.log('immediate');
});
```

- 이 경우 보통 가장 위의 setTimeout이 실행될 것이라고 생각하지만, 이벤트루프가 돌고 있는 위치에 따라 setImmediate가 먼저 실행될 수도 있다. → 실행 순서의 장담이 불가능하다.

```jsx
const fs = require('fs');

fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout');
  }, 0);
  setImmediate(() => {
    console.log('immediate');
  });
});
```

- 하지만 이 경우 항상 setImmediate가 먼저 실행된다.
- 입출력과 관련된 콜백 함수는 poll 단계에 있으며 poll은 check이 비어있는지 확인하기 때문에 항상 setTimeout이 먼저 실행된다.