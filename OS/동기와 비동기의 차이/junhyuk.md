# 동기와 비동기

### 동기

동시에 일어난다는 뜻을 가지고 있고 어떤 요청이 들어왔을 때 응답이 한자리에서 동시에 일어난다는 의미

한가지 작업을 하면 결과가 반환 될 때까지 다른 작업이 대기한다.

### 비동기

어떤 요청이 들어오면 한자리에서 동시에 응답이 일어나지 않는다.

여러 요청을 번갈아가며 수행한다.

동기의 장단점

장점 : 설계가 직관적이다

단점 : 리소스 효율이 떨어진다.

비동기 장단점

장점 : 결과가 나오기 전에 다른작업을 함께 수행할 수 있다.

단점 : 스케줄 관리 등 설계가 복잡하다.

비동기는 병렬이 아니다!! 매우 빠른 컨텍스트 스위칭으로 동시에 일어나는 것 처럼 보이는 것

병렬은 진짜 동시에 일어난다.

 블로킹과 논블로킹 : 처리되어야 하는 (하나의) 작업이, 전체적인 작업 '흐름'을 막느냐 안막느냐에 대한 관점
