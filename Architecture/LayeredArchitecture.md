## Layered Architecture(계층화 아키텍쳐)
* 코드를 논리적인 부분 혹은 역할에 따라 독립된 모듈로 나누어서 구성하는 패턴이다

* User Interface(Presentation Layer)
    * 해당 시스템을 사용하는 사용자나 클라이언트와 직접적으로 연결되는 부분
* Business Logic(Business Layer)
    * 실제 시스템이 구현해야하는 로직을 담당
    * user Interface 부분에서 읽어들은 요청을 처리
* Data Access(Persistance Layer)
    * 데이터베이스와 관련된 로직을 구현하는 곳
    * 데이터 생성, 읽기, 수정, 삭제를 처리하는 역할을 합니다

## 아키텍쳐의 핵심 요소
* 단방향 의존성
    * 각각의 Layer는 Presentation -> Business -> Persistance -> database순인데, 이것들은 서로 아래에 있는 레이어들에게만 의존해야한다

* sepration of concerns
    * 각 레이어의 역할이 명확하다
> 코드의 확장성, 재사용 가능성, 코드 파악 부분에서 이점을 가져갈 수 있다