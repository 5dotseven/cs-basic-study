# 디자인패턴 - 어댑터 패턴
어앱터 패턴이란?
> 호환되지 않는 인터페이스를 가진 객체들이 협업할 수 있도록 해주는 구조적 디자인 패턴
## 정의
:어댑터 패턴은 호환되지 않는 인터페이스들을 연결하는 디자인 패턴
> 현실 세계 어뎁터와 같은 의미!!!
> 기존 클래스의 수정 없이 특정 인터페이스를 필요로하는 코드를 사용하게 해줌

<img width="699" alt="스크린샷 2024-07-05 오후 11 55 33" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/77c070a7-df22-4bf0-9ad2-cc358446a98f">

Target: 변화에 대한 요구사항
Adaptee: 기존 코드
Adapter: 변화에 대한 요구사항을 구현한 새로운 코드

Adaptee가 가지고있는 기능을 Adapter가 주입을 받고 operation()을 구현
Client는 인터페이스인 Target을 통해 이를 사용하는 구조

## 어댑터 패턴 이점
- 외부 라이브러리 클래스를 사용하고 싶은데, 클래스의 인터페이스가 다른 코드와 호환이 되지 않을 때
- 여러 자식 클래스를 가지고 있는데 부모 클래스를 수정하기에는 호환성이 문제가 될 때

### 예시1
- 날씨정보를 받아오는 외부라이브러리를 사용하고 싶음 근데 이건 XML
- 우리 프로젝트 프론트앤드 표준은 Json 데이터기준
> 이 때, 외부 라이브러리 데이터를 Json으로 변환 하는 어댑터를 생성하여 이용 할 수 있음

### 예시2
- 이미 구현된 고객사 통합회원관리 API가 존재함
- 새로운 애플리케이션을 구성해야하는데 다시 만들기 번거로워서 통합회원관리 API를 이용하기로함
- 이때 자사 서비스는 익명으로 관리해야하는 요구사하잉 있음
> 이때 고객사 API를 사용하면서 회원 정보 조회 API를 사용할때 암호화하는 어댑터를 만들어 사용

### 코드로 예제
```java
@AllArgsConstructor
@Builder
@Getter
public class KakaoAccount {
    private final String id;
    private final String password;
    private final String name;
    private final String email;
    public static final String KAKAO_SECRET = "KA_SECRET";

    public String getAuthToken() {
        // 인증 절차 생략
        System.out.println("카카오 로그인 성공");
        return id + KAKAO_SECRET + password;
    }
}

@AllArgsConstructor
@Builder
@Getter
public class InflearnAccount {
    private final String email;
    private final String password;
    private final String username;
    public static final String INFLEARN_SECRET = "INF_SECRET";

    public String login() {
        // 인증 절차 생략
        System.out.println("인프런 로그인 성공");
        return email + INFLEARN_SECRET + password;
    }
}
// 타겟 인터페이스
public interface SocialNetworkAuthTarget {
    String getServiceName();
    String getUserName();
    String getSecret();
    String getToken();
}

// 카카오, 인프런 어댑터 구현
@AllArgsConstructor
public class InflearnSocialNetworkAuthAdapter implements SocialNetworkAuthTarget {
    private final InflearnAccount inflearnAccount;

    @Override
    public String getServiceName() {
        return "INFLEARN";
    }

    @Override
    public String getUserName() {
        return inflearnAccount.getUsername();
    }

    @Override
    public String getSecret() {
        return InflearnAccount.INFLEARN_SECRET;
    }

    @Override
    public String getToken() {
        return inflearnAccount.login();
    }
}
//`InflearnAccount` 라는 `Adaptee` 개념의 객체를 주입받아 인증정보를 구성한다.
@AllArgsConstructor
public class KakaoSocialNetworkAuthAdapter implements SocialNetworkAuthTarget {
    private final KakaoAccount kakaoAccount;

    @Override
    public String getServiceName() {
        return "KAKAO";
    }

    @Override
    public String getUserName() {
        return kakaoAccount.getName();
    }

    @Override
    public String getSecret() {
        return KakaoAccount.KAKAO_SECRET;
    }

    @Override
    public String getToken() {
        return kakaoAccount.getAuthToken();
    }
}

public interface SocialNetworkAuthService {
    static void socialLogin(SocialNetworkAuthTarget socialNetworkAuthTarget) {
        System.out.println("소셜 로그인을 시작합니다.");
        System.out.println("이용하는 서비스: " + socialNetworkAuthTarget.getServiceName());
        System.out.println("이름: " + socialNetworkAuthTarget.getUserName());
        System.out.println("토큰: " + socialNetworkAuthTarget.getToken());
    }
}

// - `socialLogin()` 메서드는 `SocialNetworkAuthService` 라는 인터페이스에서 정적 메서드로 지원한다.
// - `SocialNetworkAuthTarget` 을 인자로 받아 로그인을 수행한다.

public class Client {
    public static SocialNetworkAuthTarget getKakaoTarget() {
        KakaoAccount kakaoAccount = KakaoAccount
                .builder()
                .id("kakaoman")
                .password("kakaopassword")
                .email("kakaoman@kakao.com")
                .name("카카오제이크서")
                .build();

        return new KakaoSocialNetworkAuthAdapter(kakaoAccount);
    }

    public static SocialNetworkAuthTarget getInflearnTarget() {
        InflearnAccount inflearnAccount = InflearnAccount
                .builder()
                .email("me@naver.com")
                .password("mypassword")
                .username("인프런제이크서")
                .build();

        return new InflearnSocialNetworkAuthAdapter(inflearnAccount);
    }

    public static void main(String[] args) {
        SocialNetworkAuthService.socialLogin(getKakaoTarget());
        SocialNetworkAuthService.socialLogin(getInflearnTarget());
    }
}
```

## 자바진영에서 사용 예제
- 자브 I/O 라이브러리
<img width="692" alt="스크린샷 2024-07-05 오후 11 56 09" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/6a6265c8-f3ba-499c-b0af-fbb81294231a">
- 스프링 MVC 아키택처에서 HandlerAdapter 인터페이스
<img width="694" alt="스크린샷 2024-07-05 오후 11 56 46" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/7e42e7dc-7220-4248-9378-4651a056019f">c8-f3ba-499c-b0af-fbb81294231a">

출처: [https://jake-seo-dev.tistory.com/379](https://jake-seo-dev.tistory.com/379) [제이크서 위키 블로그:티스토리]
출처: https://yozm.wishket.com/magazine/detail/2077/ [요즘IT]

