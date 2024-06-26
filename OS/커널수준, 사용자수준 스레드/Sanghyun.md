### **사용자 수준 스레드와 커널 수준 스레드**

---

**사용자 수준 스레드**

- 사용자가 직접 생성하고 관리하는 스레드
- 유저 스레드가 실행되기 위해서는 커널 스레드와의 연결이 필수
- 커널의 프로세스만 인식할 뿐 내부의 스레드는 알지 못한다
    - 프로세스 A의 1, 2 스레드 중, 스레드 1이 IO 상태가 되면 스레드 2를 할당하는 것이 아닌 다른 프로세스의 실행을 우선시한다.
- 컨텍스트 스위칭이 없어 스레드 교체로 인한 오버헤드 발생이 없다.

**커널 수준 스레드**

- 커널 레벨에서 생성되는 스레드, 커널이 직접 관리
- IO 작업을 위한 컨텍스트 스위칭이 빈번하게 일어날 경우 성능에 영향을 준다.
- 특정 스레드가 동작할 수 없어도 다른 스레드들은 독립적으로 작업이 가능