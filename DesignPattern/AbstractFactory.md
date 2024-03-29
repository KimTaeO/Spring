# 추상 팩토리(Abstract Factory)

연관된 객체들을 묶어 추상화하고, 주어진 구체적인 상황이 발생하면 묶은 객체들을 구현하는 패턴이다

## 추상 팩토리 구조

* AbstractFactory: 최상위 공장 클래스로 제품들을 생성하는 여러 메소드들을 추상화한다

* ConcreteFactory: 서브 공장 클래스로 반환 타입에 맞는 클래스를 반환하도록 메소드를 재정의한다

* AbstractProduct: 각 타입의 제품들을 추상화한 인터페이스

* ConcreateProduct: 각 타입의 제품들의 구현체들 

![클래스 구조도](../images/AbstractFactory.png)

## 추상 팩토리 특징

* 클래스와 연관된 클래스들을 실행시킴과 동시에 구체적인 클래스에 의존하고 싶지 않을 때

* 여러 제품군들 중 하나를 선택해야하고 한번 구성한 제품들을 다른것으로 대체해야 할 때

* 제품에 대한 라이브러리를 제공할 때 구현체가 아닌 인터페이스만 노출시키고 싶을 때

## 추상 팩토리의 장점

* 객체를 생성하는 코드를 메인로직과 분리하여 관리할 수 있다

* 제품군을 쉽게 관리 가능하다

* 단일 책임 원칙과 개방 폐쇄 원칙을 준수한다