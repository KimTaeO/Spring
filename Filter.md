## 스프링부트의 필터들

* 스프링은 그 자체로 기본 필터들을 갖고 있으며 직접 필터를 생성하는 것도 가능하다.

    * 필터
        * Web Application에서 관리되는 영역이고 Spring Boot FrameWork에서 client로부터 오는 요청/응답에 대해 최초/최종 단계의 위치에 존재한다 이를 통해 요청/응답의 정보 변경 또는 스프링에 의해 데이터 변환 전 순수한 client의 요청/응답 값을 확인이 가능하다

    * 톰캣
        * WAS(Web Application Service)라고 한다
            - 아피치 sw 재단의 application server로서 자사 sublet을 실행하고 JSP가 포함된 웹 페이지를 생성한다

            - 웹 서버와 연동해 실행 가능한 자바 환경을 제공한다

            - 주로 XML 파일을 편집해 설정한다

            - 톰캣의 필터로는 인터셉터가 있는데 권한이 있으면 들어오도록, 없으면 나가도록 걸러 주는 역할을 한다

    * 인터셉터
        * 컨트롤러의 uri에 접근하는 과정에서 무언가를 제어할 필요가 있을 때 사용하고 로그인 권한 관련 처리 등을 인터셉터를 이용해 효율적으로 처리 가능하다
            - 인터셉터 추가 : org.springframework.web.servlet의 HandlerInterceptor 인터페이스를 구현(implements)한다