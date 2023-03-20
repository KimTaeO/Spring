## Fake(페이크)
* 테스트 더블의 한 종류로써, 실제 동작하는 구현을 가지고 있으나, 프로덕션에는 사용되기 적합하지 않은 객체이다
* 복잡한 로직이나 객체 내부에서 필요로 하는 다른 외부 객체의 동작을 단순화하여 구현한 객체이다
> 동작은 하지만 실제 객체처럼 정교하지는 않다

* 예시
```java
public interface UserRepository extends JpaRepository<User, Long> {
}

public interface FakeUserRepository implements UserRepository {
    List<User> users = new ArrayList<>();

    @Override
       Member save(Member member) {
           members.add(member);
           return member;
       }
       
       @Override
       Optional<Member> findByName(String name) {
              return members.stream()
                      .filter(m -> m.isName(name))
                      .findFirst();
       }
}
```
> 이를 통해 비용이 큰 요청을 직접 수행하지 않고도 단위 테스트를 수행할 수 있다