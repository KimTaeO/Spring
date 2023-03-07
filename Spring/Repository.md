## Repository(리포지토리, 저장소)

* Entity에 의해 생성된 DB에 접근하는 메서드 들을 사용하기 위한 인터페이스이다

* 위에서 엔티티를 생성함으로써 데이터베이스 구조를 만든다면 여기에 어떤 값을 넣거나, 넣어진 값을 조회하는 등 CRUD를 해야 쓸모가 있는데, 이것을 어떻게 할지 정의해주는 계층이다

### JPARepository

* Spring FrameWork에 있는 데이터 검색을 할 수 있게 해주는 구조

* 미리 검색 메소드를 정의해 두는 것으로 사용하고, 메소드를 호출하는 것만으로도 데이터 검색이 가능하게 한다