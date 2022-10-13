## Portable Service Abstraction

* 추상화 계층을 사용하여 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공해주는 것이 서비스 추상화(Service Abstraction)이다 

### PSA의 원리

* Java로 DB와 통신을 구현

1. DB Connection을 맺고 트랜잭션을 시작한다

2. 쿼리를 실행하고 결과에 따라 Commit 또는 Rollback을 하게 된다

3. 마지막으로 DB Connection을 종료한다

    * 위 과정을 java코드로 나타낸 예시
    ```java
    public void method_name() {
        // 1. DB Connection 생성
        // 2. 트랜잭션(Transaction) 시작
        try {
            // 3. DB 쿼리 실행
            // 4. 트랜잭션 커밋
        } catch(Exception e) {
            trhow new IlleagalStateException(e);
            // 5. 트랜잭션 롤백
            throw e;
        } finally {
            // 6. DB Connection 종료
        }
    }
    ```