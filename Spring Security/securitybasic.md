## Spring Security란?
* Spring 기반의 애플리케이션의 인증과 인가, 권한 등을 담당하는 스프링 하위 프레임워크이다

* ```인증```과 ```권한```에 대한 부분을 Filter 흐름에 따라 처리함

    * Filter는 Dispatcher Servlet으로 가기 전에 적용되므로 가장 먼저 URL요청을 받는다
        > Dispatcher Servlet이란 HTTP Protocol로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러라고 간단히 설명이 가능하다

    * 추가로 Interceptor는 Dispatcher와 Controller 사이에 위치하는 점에서 Filter와는 적용 시기의 차이가 존재한다

* Spring Security의 아키텍쳐는 아래와 같다
![security architecture](../images/spring%20security%20architecture.png)

* Spring Security는 Authentication 절차를 거친 후에 Authorization 절차를 진행하게 되며 Autorization 절차에서 해당 resource에 대한 접근 권한이 있는지 확인을 하게 된다 이러한 절차를 수행하기 위해서 Spring Security에서는 Principal을 아이디로 Credential을 비밀번호로 사용하는 ```Credential 기반의 Authentication 방식을 사용한다```

    * Principal(접근 주체) : 보호받는 Resource에 접근하는 대상

    * Credential(비밀번호) : Resource에 접근하는 대상의 비밀번호

## Spring Security의 모듈

* SecurityContextHolder
    * 보안 주체의 세부 정보를 포함하여 응용프로그램의 현재 보안 컨텍스트에 대한 세부 정보가 저장된다

    * SecurityContext
        * Authentication을 보관하는 역할을 하며, Authentication 객체를 SecurityContext를 통해 꺼내올 수 있다

    * Authentication
        * 현재 접근하는 주체의 정보와 권한을 담는 인터페이스이다

    * SecurityContextHolder를 통해 SecurityContext에 접근하고, SecurityContext를 통해 Authentication에 접근할 수 있다

* UserNamePasswordAuthenticationToken
    * Authentication을 implements한 AbstractAuthenticationToken의 하위 클래스로, User의 ID가 Principal 역할을 하고, Password가 Credential의 역할을 한다

    * UserNamePasswordAuthenticationToken의 생성자에는 두 가지 종류가 존재하는데 각각 아래 코드의 주석에 대한 역할을 수행한다

    ```java
    public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
    private final Object principal;
    private Object credentials;
    
        // 인증 완료 전의 객체 생성
        public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
            super(null);
            this.principal = principal;
            this.credentials = credentials;
            setAuthenticated(false);
        }
        
        // 인증 완료 후의 객체 생성
        public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
                Collection<? extends GrantedAuthority> authorities) {
            super(authorities);
            this.principal = principal;
            this.credentials = credentials;
            super.setAuthenticated(true); // must use super, as we override
        }
    }

    public abstract class AbstractAuthenticationToken implements Authentication, CredentialsContainer {}
    ```

* AuthenticationProvider
    * 실제 인증에 대한 부분을 처리하며, 인증 전의 Authentication 객체를 받아서 인증이 완료된 객체를 반환하는 역할을 한다

    * AuthenticationProvider 인터페이스를 구현하여 Custom한 AuthenticationProvider를 작성하여 AuthenticationManager에 등록하면 된다

* AuthenticationManager
    * 인증에 대한 부분은 여기에서 처리하게 되는데, 실질적으로는 AuthenticationManager에 등록된 AuthenticationProvider에 의해 처리된다

    * 인증이 성공하면 2번째 생성자를 이용해 인증이 성공한 ```(isAuthenticated=true)```객체를 생성하여 SecurityContext에 저장한다

    * 인증 상태를 유지하기 위해 세션에 보관하며, 인증이 실패한 경우에는 AuthenticationException을 발생시킨다

    * ProviderManager
        * Authentication을 implements 하며, AuthenticationProvider를 List로 가지고 있으며, ProviderManager는 for문을 통해 모든 Provider를 조회하면서, authenticate 처리를 한다

* UserDetails
    * 인증에 성공하여 생성된 UserDetail 객체는 Authentication객체를 구현한 UserNamePasswordToken을 생성하기 위해서 사용된다

* UserDetailsService
    * UserDetails 객체를 반환하는 단 하나의 메서드를 가지고 있다

    * 일반적으로 구현된 클래스 내부에 UserRepository를 주입받아 DB와 연결하여 처리한다

* Password Encoder
    * ```AuthenticationManagerBuilder.userDetailsService().passwordEncoder()``` 를 통해 패스워드 암호화에 사용될 passwordEncoder 구현체를 지정할 수 있다

* GrantedAuthority
    * 현재 사용자(principal)가 가지고 있는 권한을 의미한다

    * ROLE_ADMIN, ROLE_USER와 같이 ROLE_*의 형태로 사용하며, 보통 'roles'이라고 한다 GrantedAuthority 객체는 UserDetailsService에 의해 불러올 수 있고, 특정 자원에 대한 권한이 있는지를 검사하여 접근 허용 여부를 결정한다