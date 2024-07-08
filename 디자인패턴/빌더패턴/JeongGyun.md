# 디자인패턴 - 빌더패턴
```java
@NoArgsConstructor
@AllArgsConstructor
public class User {

    private String name;
    private int age;
    private int height;
    private int iq;

}
```

## 빌더 패턴의 장점
1. 필요한 데이터만 설정할 수 있음
2. 유연성을 확보 할 수 있음
3. 가독성을 높일 수 있음
4. 변경 가능성을 최소화할 수 있음 -> **중요**

### 필요한 데이터만 설정할 수 있음
만약, User 객체를 생성하는데 age라는 파라미터는 필요하지 않는 상황
생성자를 사용한다면 -> age 가 없는 생성자를 새로 만들어줘여함
정적메소드를 사용한다면 -> age에 더미값을 넣어줘야함

```java
// 더미값을 넣어주는 방법
User user = new User("정균", 0, 174, 150)

// 2. 생성자 또는 정적 메소드를 추가하는 방법
@NoArgsConstructor 
@AllArgsConstructor 
public class User { 
    private String name;
    private int age;
    private int height;
    private int iq;

    public User (String name, int height, int iq) {
        this.name = name;
        this.height = height;
        this.iq = iq;
    }
    
    public static User of(String name, int height, int iq) {
        return new User(name, 0, 174, 150);
    }
    
```

> 요구 사항이 바뀐다면 계속 변하게 되는 상황이 발생 하지만 빌더로 생성한다면 동적으로 처리할 수 있음

```java
User user = User.builder()
             .name("정균")
             .height(174)
             .iq(150)
             .build();
```

### 유연성을 확보할 수 있음
만약, 새로운 변수(파라미터)가 생긴다면 -> 생성자를 수정해야함
하지만 빌더 패턴으로 한다면 -> 보다 쉽게 가능
```java
User user = User.builder()
             .name("정균")
             .height(174)
             .whight(86)
             .iq(150)
             .build();
```

### 가독성을 높일 수 있음
```java
// 파악 불가능
User user = new User("정균", 0, 174, 150)
// 직관적으로 파악가능
User user = User.builder()
             .name("정균")
             .height(174)
             .iq(150)
             .build();
```

### 변경 가능성을 최소화할 수 있음
Setter를 사용해서 편하게 수정할 수 있음 하지만 이건 변경을 열어두는거라 협업하는 개발자들끼리 소토잉 되지않는다면 불필요하게 변경될 가능성이 높아짐 -> 유지보수하기 힘들어지며 -> setter부분을 찾는거 또한 어려워 질 수 있음
따라서 변경 가능성을 최소화 하는것이 가장 좋은 방법
```java
@Builder
@Getter
@AllArgsConstructor
public class User {

    private String name;
    private int age;
    private int height;
    private int iq;

}
```

> Setter는 열지말고 getter만 열어주자!!


객체를 생성할때 대부분의 경우에는 빌더 패턴을 적용하는것이 좋음
하지만 이 2가지 경우라면 조금 고려를해서 사용하자!!
- 객체의 생성을 라이브러리로 위임하는 경우
- 변수의 개수가 2개 이하이며, 변경 가능성이 없는 경우
