## 빌더(Builder) 패턴이란?
* 복잡한 객체의 **생성** 과정과 **표현** 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들수 있도록 하는 패턴이다

## 빌더 패턴을 사용하는 이유
* 개발자가 필요한 데이터만 설정을 할 수 있음

* 가독성을 높이고, 유연한 변경이 가능함
    > 단, 생성할 때 설정자 사용을 자제하여
## 자바로 빌더 패턴 구현
```Java
// Builder를 생성자로 하는 클래스
public final class Car {
    private final String wheel;
    private final String window;
    private final String engine;

    private Car(Builder builder) {
        this.wheel = builder.wheel;
        this.window = builder.window;
        this.engine = builder.engine;
    }
}

// Builder 클래스
public static class Builder {
    private final String wheel;
    private final String window;
    private final String engine;

    public Builder withWheel(String wheel) {
        this.wheel = wheel;
        return this;
    }

    public Builder withWindow(String window) {
        this.window = window;
        return this;
    }

    public Builder withEngine(String engine) {
        this.engine = engine;
        return this;
    }

    public Car build() {
        return new Car(this);
    }
}
```