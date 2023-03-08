## 클린 아키텍쳐란(Clean Architecture)
* 기존의 Layered Architecture가 가지던 의존성에서 벗어나게 해주는 설계이다
![CleanArchitecture](/images-___pepper-post-afcaa5d2-7653-4ccb-8d91-c8b8881142f6-%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-11-24%20%EC%98%A4%ED%9B%84%206.41.12.png)

* 외부에서 독립적으로 인터페이스를 구현할 수 있도록 한다

* 바깥쪽 원에 해당하는 어떠한 것들도 안쪽의 원에는 영향을 줄 수 없다

* 클린 아키텍쳐에서 바깥으로 갈수록 세부 사항에 해당하고, 안쪽으로 갈수록 중요해진다

* 클린 아키텍쳐에서는 확장 가능하고, 테스트가 가능한 프로그램(TDD에 용이한)을 만드는 것에 용이한 구조를 제공한다

## 클린아키텍쳐로서 중요하게 생각해야하는 것들

* 내부 데이터와 로직들은 안쪽을 향해야 한다

* 요청은 안쪽으로 들어와 바깥쪽으로 나간다
    > 요청 진행 방향이 바뀌는 경우는 없어야 한다

* 인터페이스의 변경이 코드에 변경을 일으키지 않도록 해야 한다