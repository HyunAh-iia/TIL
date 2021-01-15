# IllegalStateException : Multiple representations of the same entity
1:N 관계의 부모 Entity와 자식 Entity를 동시에 저장할 때 에러가 발생했다.
`error : java.lang.IllegalStateException: Multiple representations of the same entity`

에러 메시지를 토대로 원인을 찾아보니 다음과 같은 Hibernate 이슈글을 발견했다. 원문은 하단 참고문서에 링크해두었다.

> Hibernate throws IllegalStateException when merging entity 'x' if it has references to 2 detached entities 'y1' and 'y2' (obtained from different sessions), and y1 and y2 represent the same persistent entity.

Hibernate는 `multiple representations of the same persistent entity` 인 상황에서 오류를 발생한다.
아래 두 가지 조건이 충족되면 내부적으로 자식 Entity를 복사하나본데, 동일한 Entity에 대한 복사가 감지되면 오류를 발생시키는 것이다. (영속성 시 Entity는 하나의 대표 객체만 가져야하는걸까?)
- 부모 Entity(x)를 merge 할 때 detached되어 있는 자식 Entity(y1, y2)들이 있음
- 이 때 각각 y1, y2 자식 객체는 동일한 persistent entity를 대표함

### 부모 Entity
```
@Getter
@Entity
public class UserSurvey {
   @GeneratedValue(strategy = GenerationType.IDENTITY)
   @Id
   private Long id;

   @Setter
   @OneToMany(mappedBy = "userSurvey", fetch = FetchType.LAZY, cascade={CascadeType.ALL})
   private List<UserSurveyAnswer> answers;

   ...
}
```

### 자식 Entity
자식 Entity는 부모의 Key를 포함한 복합키를 가지고 있다.
```
@Getter
@IdClass(UserSurveyAnswerId.class)
@Entity
public class UserSurveyAnswer {
    @Id
    @ManyToOne(targetEntity = UserSurvey.class)
    @JoinColumn(name = "user_survey_id", referencedColumnName = "id")
    private UserSurvey userSurvey;

    @Id
    @ManyToOne
    @JoinColumn(name = "survey_question_id", referencedColumnName = "id")
    private SurveyQuestion surveyQuestion;

    ...
}
```

### 저장
```
private Long save(...) {
    List<UserSurveyAnswer> questionAnswers = createQuestionAnswers(...);
    userSurvey.setAnswers(questionAnswers);
    userSurveyRepository.save(userSurvey);

    return userSurvey.getId();
}
```

### 해결방법
application.yml 파일에 `entity_copy_observer` 옵션을 `allow`로 변경하였다.
```spring.jpa.properties.hibernate.event.merge.entity_copy_observer=allow```
옵션을 추가하고 테스트를 해보니 이번에는 문제없이 저장이 잘 되었다.

### 진짜 해결방법일까?
이상하다. 다른 oneToMany 관계에서도 부모 Entity를 통해 자식 Entites를 생성했었는데, 오류 없이 저장이 되었다.
지금 케이스와 이전 케이스가 어떻게 다른 것일까?
`entity_copy_observer` 옵션을 주어서 해결하는 게 맞는 방법일까?
default로 `entity_copy_observer` 옵션이 `disallow`되어 있다. 이를 활성화할 경우의 리스크도 존재한다고 하며, default로 막아놓은 이유가 있다.
일단... 현상은 해결되었지만 다른 1:N 관계에서 동일한 방식으로 저장시에 해당 옵션이 없어도 정상 저장 된 걸 보면 올바른 해결방법이 아닌것같다.
출력된 에러메시지 자체에 대한 원인과 해결방법은 `entity_copy_observer` 설정이 맞지만, 음... 일단 내 케이스에 대해 더 비교해볼 필요성이 있다.


---
참고문서
- [hibernate HHH-9106](https://hibernate.atlassian.net/browse/HHH-9106)
- [java.lang.IllegalStateException: Multiple representations of the same entity with @ManyToMany 3 entities](https://stackoverflow.com/questions/26591521/java-lang-illegalstateexception-multiple-representations-of-the-same-entity-wit)