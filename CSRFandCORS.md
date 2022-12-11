## CORS(Cross-Origin Resource Sharing)
* 한국어로 직역하면 교차 출처 리소스 공유이다
    > 교차 출처란 "다른 출처"를 의미하는 것이다

* 출처는 Protocol과 Host를 나타내며 서버를 찾아가기 위한 기본적인 것들을 합쳐놓은 주소이다

### SOP(Same-Origin Policy)
* 다른 출처로의 리소스 요청을 제한하는 것과 관련된 두가지 정책이 존재하며 각각 CORS, SOP이다

    * SOP는 RFC 6454에서 처음 등장한 보안 정책이며, 같은 출처에서만 리소스를 공유 할 수 있다는 규칙을 가진 정책이다
        > 하지만 웹이라는 오픈스페이스 환경에서 다른 출처에 있는 리소스를 가져와서 사용하는 것은 굉장히 흔한 일이기 때문에 몇 가지 예외 조항을 두고 조항에 해당하는 리소스 요청은 출처가 다르더라도 허용하도록 했는데 그 중 하나가 CORS정책을 지킨 리소스 요청이다.

* SOP 정책을 웹 서버상에는 지키기 어려우니 CORS 정책을 도입하여 다른 출처들간의 리소스들을 교환하는 것이다

* CORS 동작방식 3가지

    * Preflight Request
    * Simple Request
    * Credentialed Request

* CORS를 해결 할 수 있는 방법

    * Access-Control-Allow-Origin 세팅하기