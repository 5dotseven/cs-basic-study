## **ORM**

---

Object Relational Mapping, 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 프로그래밍 기술

- 객체와 관계형 DB 간의 데이터를 매핑하는 기술
- 개발자는 SQL 언어 없이 객체 지향 언어로 데이터를 관리 가능
- DB 설계와 비즈니스 로직 사이의 간극을 줄여주며 코드의 가독성과 유지 보수성을 높여줌

**MyBatis, JPA**

> SQL Mapper: 개발자가 작성한 SQL 실행 결과를 객체에 매핑
>

**MyBatis**: SQL Mapper 기술을 제공

- 개발자가 Java 메소드 선언문과 SQL문을 작성하면 MyBatis가 자동으로 둘을 연결
- SQL의 직접 제거악 가능하며 복잡한 쿼리, 특정 DB에 최적화된 쿼리 작성에 적합
- 기본적이고 단순한 작업에도 SQL문을 직접 작성해야 하는 단점이 존재, DB에 종속적이기 때문에 DB 수정 시 SQL문을 전부 수정해야 한다.

**JPA**: ORM 기술을 제공

- Java 객체와 DB 엔티티 자체를 매핑하여 접근할 수 있도록 함
- MyBatis와 다르게 더 자동화가 이루어져 SQL문까지 생성해준다는 차이점
- 표준화된 인터페이스로 종속되지 않아 일관된 방식으로 개발이 가능, DBMS와 독립적
- 어노테이션, 즉시 로딩, 지연 로딩, 영속성 전이 등의 스펙과 작성법으로 인해 높은 학습 곡선이 존재하며 복잡한 SQL의 생성이 여렵다.
- DB의 특성에 따라 자동화된 SQL문은 성능 저하로 이어질 가능성이 있다.

출처:

[https://mangkyu.tistory.com/20](https://mangkyu.tistory.com/20)
[https://f-lab.kr/insight/understanding-orm](https://f-lab.kr/insight/understanding-orm)