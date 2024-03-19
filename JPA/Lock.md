# Lock
JPA에서는 데이터베이스에 대한 동시 접근을 처리하기 위한 동시성 제어 매커니즘을 지원한다

## Lock의 종류
* 낙관적 락(Optimistic Lock)

    대부분의 트랜잭션에서 충돌이 발생하지 않을 것이라고 낙관적으로 가정하는 방법이다 
    버전 관리용 필드에 ```@Version``` 어노테이션을 사용하여 버전을 관리할 수 있으며, 사용할 수 있는 자료형으로는 Int, Long, Short, TimeStamp가 있다
    엔티티가 변경될 때마다 버전이 증가하며, 조회한 시점의 버전과 수정한 시점의 버전이 일치하지 않으면 예외가 발생한다
    > 버전 정보를 비교하는 방식은 이러하다
    ```SQL
    UPDATE table SET 
    content = ?,
    version = ? -- 버전 정보를 +1로 증가
    WHERE id = ?
    AND version = ? -- 버전 정보를 비교
    ```

    * 낙관적 락의 LockModeType
        * NONE
            * 조회한 엔티티를 수정하는 시점까지 변경되거나 삭제되지 않음을 보장한다
        * OPTIMISTIC
            * 조회한 엔티티를 트랜잭션이 끝나는 시점까지 변경되거나 삭제되지 않음을 보장한다
        * OPTIMISTIC_FOR_INCREMENT
            * 엔티티가 물리적으로 변경되지는 않았지만 논리적으로 변경되었을 경우에 version 값을 증가시켜야 하는 경우에 사용한다

* 비관적 락(Pessimistic Lock)
    
    데이터베이스의 락을 사용하여 동시성을 제어하며, ```SELECT ... FOR UPDATE```문을 주로 사용한다

    * 비관적 락의 LockModeType
        * PESSIMISTIC_WRITE
            * ```SELECT ... FOR UPDATE```를 사용하여 데이터베이스에 ```Exclusive Lock```을 건다

        * PESSIMISTIC_READ
            * ```SELECT ... FOR SHARE```를 사용하며, 데이터를 반복적으로 읽기 위하여 사용한다

        * PESSIMISTIC_FOR_INCREMENT
            * 비관적 락 중 버전 정보를 사용한다 하이버네이트 구현체에서는 ```SELECT ... FOR UPDATE NOWAIT```를 지원하는 데이터베이스라면 NOWAIT 옵션을 지원한다