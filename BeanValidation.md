## Bean Validation
* 스프링의 기본적인 validation 로직은 필드에 특정 어노테이션을 적용하여 필드가 갖는 제약 조건을 정의하는 구조이다

## 장점
* 컨트롤러 진입 전 inteceptor 단계에서 검증을 함으로써 검증 시점과 검증 방법을 일관성 있게 가져갈 수 있다

* 일관성있는 ErrorResponse이다 오류가 발생하면  ConstraintViolationException이 발생하기 때문에 일관성있는 오류 응답을 할 수 있다

* 사용 예시

```java
@Getter
public signUpReqDto {
    @Email
    private String email;
    @Notblank(message = "이름은 공백이거나 null값을 허용하지 않습니다")
    private String name;
}
```

* custom validator 을 통한 오류처리

```java
// 어노테이션 추가
@Documented
@Constraint(validatedBy = CustomValidator.class)
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface CustomRequestValid {
    String message() default "유효하지 않은 요청";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}

// validator
@Component
@AllArgsConstructor
@Transactional(readOnly = true)
public class CustomValidator implements ConstraintValidator<CustomRequestValid, CustomDto>{

    @Override
    public boolean isValid(CustomDto customDto, ConstraintValidatorContext context) {
        if(customDto.getId() =! null ) {
        context.disableDefaultConstraintViolation();
        // 검증 실패한 항목들에 대해 모두 violation을 추가할 수 있으며 이를 exception handler에서 처리가 가능하다.
        context.buildConstraintViolationWithTemplate(errorMessage)
                .addPropertyNode(firstNode)
                .addConstraintViolation();
        }
    }
}

//Dto
@Data
@CartItemRequestValid
public class CustomDto {
    private String id;
}

// controller
@RestController
@RequestMapping
public class controller {
    @GetMapping // 유효하지 않은 검증이 일어나면 handler에 등록된 오류를 통해 해결이 가능하다
    public ResponseEntity<> findById(@Reqeustbody @Valid CustomDto customDto) {

    }
}
```