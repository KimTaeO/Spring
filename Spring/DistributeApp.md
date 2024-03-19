# 애플리케이션 배포

## 실행 가능한 JAR파일을 만들기 위해서는 아래와 같은 명령어를 사용해야 한다

```cli
./gradlew build
```
> 프로젝트 디렉토리로 이동한 뒤 실행해야 한다

## JAR파일 구성 요소

* ### JAR 파일을 읽고 JAR 안에 포함돼 있는 JAR 파일에 있는 클래스를 로딩하기 위한 스프링 부트 커스텀 코드

* ### 애플리케이션 코드

* ### 사용하는 서드파티 라이브러리들