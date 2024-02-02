# Spring Actuator

Actuator란 HTTP나 JMX를 통해 운영 중인 어플리케이션을 모니터링 및 관리하는 것이다

어플리케이션 감사, 상태, 메트릭 등을 수집하는 것 또한 자동적으로 어플리케이션에 적용할 수 있다

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```
를 통해 dependency를 추가할 수 있다

## Endpoint

actuator는 애플리케이션을 감시하고 상호작용할 수 있는 엔드포인트를 제공하는데, 기본적으로 제공되는 엔드포인트 뿐만 아니라 자신의 것도 추가할 수 있다.
각각의 여러 엔드포인트들을 활성화 또는 비활성화 할 수 있고, HTTP와 JMX를 통해 노출시킬 수 있다. HTTP를 사용하여 노출시키는 경우에는 ```/actuator``` 접두사를 통해 URL에 매핑할 수 있다

URL의 종류에는 여러 가지가 있지만 그중 몇가지만 알아보자면
* health : 어플리케이션의 상태 정보를 보여준다
* loggers : 애플리케이션의 logger의 설정을 수정하고, 보여준다
* metrics : 애플리케이션의 현재 metric 정보를 보여준다

애플리케이션이 웹 애플리케이션이라면 추가적인 엔드포인트들을 사용할 수 있다

* heapdump : JVM의 heap 메모리를 dump한 파일을 반환한다 HPROF 포맷의 파일이 반환된다
* logfile : 로그 파일의 내용을 반환한다. HTTP 의 ```Range```헤더를 통해 로그 파일의 내용을 검색할 수 있다
* prometheus : 프로메테우스 서버에서 스크랩 가능한 형식으로 메트릭을 노출한다 ```micrometer-registry-prometheus```의존성을 필요로 한다.

엔드포인트들을 활성화 시키는 방법

```yaml
management:
    endpoint:
        <endpoint>: 
            enabled: true
```

엔드포인트들에는 민감한 정보들이 포함되어 있을 수 있으므로 노출 여부를 결정할 수 있다

HTTP를 통해 엔드포인트를 노출하는 경우
```yaml
management:
    endpints:
        web:
            exposure:
                include: 노출할 엔드포인트들을 ,로 구분하여 나열한다. *를 적어 모든 엔드포인트를 노출할 수 있다.
```

