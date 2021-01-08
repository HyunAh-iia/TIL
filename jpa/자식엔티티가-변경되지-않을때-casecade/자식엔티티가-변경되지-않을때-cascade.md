JPA 자식 엔티티가 변경되지 않을 때 (feat. cascade)
---
`cascade`는 특정 Entity의 영속성 상태가 변경되었을 때 이를 연관된 Entity에도 전파시킬 지 선택하는 옵션이다.

특정 Entity에 `@ElementCollecion`으로 관리되던 하위 컬렉션이 `@Entity`로 변경되었는데 `cascade` 옵션이 함께 설정하지 않았다.
컬렉션은 기본적으로 부모 Entity와 한 쌍으로 움직이기 때문에 `cascade` 옵션이 없어도 부모 Entity와 함께 저장/삭제된다. (cascade 옵션을 ALL로 준 것 처럼 작동함)
하지만 Entity 간의 연관 관계에서는 기본적으로는 아무런 상태도 전이시키지 않기 때문에 연관을 설정할 때 cascade 옵션에 대해 고려해야한다.
`@ElementCollecion`으로 관리할 때에는 부모 Entity를 통해 자식 테이블에 저장이 잘 되었는데, 어느 순간부터 자식 테이블에 데이터가 쌓이지 않아서 헤매다가 영속성 전이 옵션에 대해 알게 되어서 정리한다.
영속성 전이 옵션을 통해 부모 Entity를 변경할 때 자식 Entity도 함께 변경할 수 있다.

[@ElementCollection 설명](../ElementCollection/ElementCollection-annotaion.md)

# Entity 상태
Entity의 상태는 크게 4가지 종류가 있다.
1. `Transient` : 객체를 생성하고, 값을 주어도 JPA나 hibernate가 그 객체에 관해 아무것도 모르는 상태. 즉, 데이터베이스와 매핑된 것이 아무것도 없다.
2. `Persistent` : 저장을 하고나서, JPA가 아는 상태(관리하는 상태)가 된다. 그러나 .save()를 했다고 해서, 이 순간 바로 DB에 이 객체에 대한 데이터가 들어가는 것은 아니다. JPA가 persistent 상태로 관리하고 있다가, 후에 데이터를 저장한다.(1차 캐시, Dirty Checking(변경사항 감지), Write Behind(최대한 늦게, 필요한 시점에 DB에 적용) 등의 기능을 제공한다)
3. `Detached` : JPA가 더이상 관리하지 않는 상태. JPA가 제공해주는 기능들을 사용하고 싶다면, 다시 persistent 상태로 돌아가야한다.
4. `Removed` : JPA가 관리하는 상태이긴 하지만, 실제 commit이 일어날 때, 삭제가 일어난다.

# casecade 옵션
- CascadeType.PERSIST
  - 엔티티를 영속화 할 때 연관된 엔티티도 함께 유지 ([* PERSIST의 예상치 못한 동작](https://joont92.github.io/jpa/CascadeType-PERSIST%EB%A5%BC-%ED%95%A8%EB%B6%80%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%95%88%EB%90%98%EB%8A%94-%EC%9D%B4%EC%9C%A0/))
  - 연관 엔티티가 DB에 이미 저장이 되어있어도 다시 persist하기 때문에 `detached entity passed to persist Exception`이 발생함(이경우에는 CascadeType.MERGE를 사용)
- CascadeType.MERGE
  - 엔티티 상태를 병합 할 때, 연관된 엔티티도 함께 병합
  - 트랜잭션이 종료되고 detach 상태에서 엔티티가 merge()를 수행하게 되면 연관 엔티티의 추가 및 수정사항도 함께 적용됨
- CascadeType.REFRESH
  - 엔티티를 새로 고칠 때, 연관된 엔티티도 새로고침
- CascadeType.REMOVE
  - 엔티티를 삭제할 때, 연관된 엔티도 함께 삭제
- CascadeType.DETACH
  - 부모 엔티티가 detach()를 수행하게 되면, 연관된 엔티티도 detach() 상태가 되어 변경사항이 반영되지 않음
- CascadeType.ALL
  - 모든 Cascade 적용

---

간단하게 예제를 꾸려 살펴보겠다.
- 부모(Parent) Entity
  - 하위 컬렉션과(ChildCollection) 
  - 하위 Entity(ChildEntity)

```java
@Builder
@NoArgsConstructor
@Entity
public class Parent {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ElementCollection
    @CollectionTable(name="child_collection", joinColumns = @JoinColumn(name= "parent_id", referencedColumnName = "id"))
    private Set<ChildCollection> childCollections = new HashSet<ChildCollection>();

    @OneToMany(mappedBy = "parent") //, cascade = CascadeType.ALL)
    private Set<ChildEntity> childEntities = new HashSet<ChildEntity>();
}
```

```
@Embeddable
public class ChildCollection{
    private String name;
    private String description;
}
```

```
@Builder
@Entity
public class ChildEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    private Parent parent;

    private String name;

    private String description;
}
```

각각 컬럼에 데이터를 저장해보겠다.
```
...
    Set<ChildCollection> collections;
    Set<ChildEntity> entities;
    
    Parent parent = Parent.builder()
        .childCollections(collections)
        .childEntities(entities)
        .build();

    reposistory.save(parent);
...
```
- @ElementCollection의 하위 컬렉션은 부모 Entity와 함께 저장된다.
- @Entity 자식 엔티티는 부모 Entity와 함께 저장되지 않는다. => cascade option 설정 필요
---
> 참고링크 
- [A beginner’s guide to JPA and Hibernate Cascade Types](https://vladmihalcea.com/a-beginners-guide-to-jpa-and-hibernate-cascade-types/)
- [stackoverflow - What is the meaning of the CascadeType.ALL for a @ManyToOne JPA association](https://stackoverflow.com/questions/13027214/what-is-the-meaning-of-the-cascadetype-all-for-a-manytoone-jpa-association)
- [JPA 엔티티 상태 & Cascade](https://velog.io/@max9106/JPA%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%83%81%ED%83%9C-Cascade)