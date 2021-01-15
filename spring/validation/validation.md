#Spring JSR 380 Validation 적용과 테스트 코드 작성

유효성(Validation)을 체크하는 Java bean인 `JSR 380`(또는 `Bean Validation 2.0`)에 대해 간단히 알아보겠다.
Java 8 버전 이상의 버전이 요구되며, Spring boot 2.3 이후부터는 validation 의존성을 추가해야한다.
```
<dependency> 
     <groupId>org.springframework.boot</groupId> 
     <artifactId>spring-boot-starter-validation</artifactId> 
 </dependency>

or

implementation 'org.springframework.boot:spring-boot-starter-validation'
```

---

### 유효성 체크 정의
유효성 체크를 하고 싶은 DTO에 검증 어노테이션와 관련 메시지를 정의한다.

```
import javax.validation.constraints.NotBlank

@Getter
@Setter
@NoArgsConstructor
public class MemberDto {
    ...

    @NotBlank(message = "아이디를 입력하세요.")
    private String loginId;

    @NotBlank(message = "생년월일을 입력하세요.")
    private String birth;

    ...
}
```

아래와 같은 유효성 검사 어노테이션을 사용할 수 있다.
-   **@NotNull** validates that the annotated property value is not null
-   **@AssertTrue** validates that the annotated property value is _true._
-   **@Size** validates that the annotated property value has a size between the attributes and ; can be applied to _String_, _Collection_, _Map_, and array properties.
-   **@Min** validates that the annotated property has a value no smaller than the attribute.
-   **@Max** validates that the annotated property has a value no larger than the attribute.
-   **_@Email_** validates that the annotated property is a valid email address.
-   **_@NotEmpty_** validates that the property is not null or empty; can be applied to _String_, _Collection_, _Map_ or _Array_values.
-   **_@NotBlank_** can be applied only to text values and validates that the property is not null or whitespace.
-   **_@Positive_** and **_@PositiveOrZero_** apply to numeric values and validate that they are strictly positive, or positive including 0.
-   **_@Negative_** and **_@NegativeOrZero_** apply to numeric values and validate that they are strictly negative, or negative including 0.
-   **_@Past_** and **_@PastOrPresent_** validate that a date value is in the past or the past including the present; can be applied to date types including those added in Java 8.
-   **\*@Future** and **@FutureOrPresent\*** validate that a date value is in the future, or in the future including the present.

### 검증

DTO에 validation annotation을 적용했다고 해서 자동으로 검증이 되지는 않는다. 어느 시점에 검증을 수행할 지 알려야한다. Controller에서 해당 DTO로 요청을 받았을 때 검증이 수행되도록 `@Valid` 를 사용하였다.
-   `@Valid`를 통한 검증
      ```
        import javax.validation.Valid;
        
        @PostMapping(value = "/member/join")
            public ResponseWrapper<Long> postMember(@Valid MemberDto member) {
        
                return new ResponseWrapper<>(memberService.save(member));
            }
      ```

-   테스트코드에서 직접 validator 실행
    ```
    import lombok.extern.slf4j.Slf4j;
    import org.junit.Test;
    
    import javax.validation.ConstraintViolation;
    import javax.validation.Validation;
    import javax.validation.Validator;
    import javax.validation.ValidatorFactory;
    
    import java.util.Set;
    
    @Slf4j
    public class MemberDtoTest {
    
        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        Validator validator = factory.getValidator();
    
        @Test
        public void validateMemberDto() {
    
            MemberDto dto = new MemberDto();
            dto.setId(null);
            dto.setBirth("");
    
                    // 검증
            Set<ConstraintViolation<MemberDto>> violations = validator.validate(dto);
    
                    // 검증 결과를 출력
            for (ConstraintViolation<MemberDto> violation : violations) {
                log.error(violation.getMessage());
            }
        }
    }
    ```

-   테스트결과
    ```
    18:36:42.711 [Test worker] ERROR net.huray.da.dto.MemberDtoTest - 생년월일을 입력하세요.
    MemberDtoTest > validateMemberDto STANDARD_OUT
        18:36:42.711 [Test worker] ERROR net.huray.da.dto.MemberDtoTest - 생년월일을 입력하세요.
    18:36:42.711 [Test worker] ERROR net.huray.da.dto.MemberDtoTest - 아이디를 입력하세요.
        18:36:42.711 [Test worker] ERROR net.huray.da.dto.MemberDtoTest - 아이디를 입력하세요.
    ```

---

> 참고 자료

- [Java Bean Validation Basics](https://www.baeldung.com/javax-validation)
- [Validation in Spring Boot](https://www.baeldung.com/spring-boot-bean-validation)

> 추천 자료

- [Spring Validation 공통모듈 만들기](https://jojoldu.tistory.com/129)
- [Custom Error Message Handling for REST API](https://www.baeldung.com/global-error-handler-in-a-spring-rest-api)
- [Exception Handling in Spring MVC](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)

---

이번 포스팅은 `Java Bean Validation Basics` 글을 보고 간단하게 Validation 적용 대상, 적용 시점, 에러 객체가 무엇인지 보기 위해 테스트 코드를 작성하였다. 하지만 Validation 처리에 대한 예외처리를 공통으로 처리하고 싶은 경우 추천 자료를 참고하길 바란다.