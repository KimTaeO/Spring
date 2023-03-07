## JPQL(Java Persistant Query Langauge)

* JPQL은 엔티티 객체를 조회하는 객체지향 쿼리이다

* 테이블을 대상으로 하는것이 아닌 엔티티 대상을 객체로 한다

* SQL과 비슷한 문법을 가지며, JPQL은 결국 SQL로 변환된다

### JPQL 특징

* 테이블이 아닌 객체를 검색하는 객체지향 쿼리

* SQL을 추상화하였기 때문에 특정 벤더에 종속되지 않음

* JPA는 JPQL을 분석하여 SQL을 생성한 후 DB에서 조회

* JPQL 기본 문법
```java
String jpql = "SELECT column FROM table where column = string";
```

### JPQL과 SQL의 차이점

* 1. 대소문자의 구분
    * 엔티티와 속성은 대소문자를 구분한다
    * 엔티티 이름인 Member, 그리고 Member의 속성 name은 대소문자를 구분해줘야 한다
    * 반면에 SELECT, FROM, AS 같은 JQPL 키워드는 대소문자 구분이 필요없다

* 