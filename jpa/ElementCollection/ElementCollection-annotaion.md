@ElementCollection
---
http://redutan.github.io/2018/05/29/ddd-values-on-jpa
@ElementCollection은 Basic Type이나 Embeddable Class에 사용된다.
Embeddable 설명 추가

@ElementCollection
@CollectionTable과 함께 쓰는 경우

설명 잘되어있음 
https://blog.naver.com/PostView.nhn?blogId=qjawnswkd&logNo=222074814530

3. @ElementCollection을 사용하는 경우
  3-0 Entity가 아닌 단순한 형태의 객체 집합을 정의하고 관리하는 방법이다.
  3-1 한 테이블에서 연관된 다른 테이블에 대한 정보를 다룬다. One-To-Many 관계를 다룬다.
    3-1-1 @Embeddable 객체와 관계를 정의하여 사용할 수 있다.
    3-1-2 자바의 primitive와 그들의 wrapper클래스들와 관계를 정의하는데 사용한다.( Integer, Double, ...)
      3-1-2-1 @Entity 를 받는 속성을 정의할 수 없다. 그렇게 하려면 @OneToMany를 사용해야 한다.
    3-1-3 이 말은 이 방식으로는 아래와 같은 형식의 간단한 Collection의 타입만을 사용할 수 있다는 말이다.


private Set<String> courses = new HashSet<String>();
private List<Integer> results = new ArrayList<>();
 
  3-2 보통 관계하는 테이블을 별도의 자바 Entity로 구성하지 않는 경우에 사용한다.
    3-2-1 물론 별도의 Entity를 구성해도 사용이 가능하지만 그럴 경우 Entity에 @Id를 지정해야 하니 귀찮아진다.
  3-3 이 annotation이 설정된 속성은 부모 클래스와 별도로 저장하거나 테이블에서 가져올 수 없다.

    3-3-1 그렇게 하려면 @Entity로 Entity를 생성해야 한다.
  3-4 cascade 옵션을 제공하지 않는다.
  3-5 관계 테이블의 데이터는 무조건 항상 부모와 함께 저장되고 삭제되고 관리된다.
    3-5-1 아래의 경우 course 테이블의 정보는 항상 Student 클래스와 함께 관리되어야 한다는 의미가 된다.