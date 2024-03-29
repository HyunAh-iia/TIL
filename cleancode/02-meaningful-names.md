# 2장 의미 있는 이름

## 책에서 기억하고 싶은 내용
### 의도를 분명히 밝혀라
- 변수나 함수 그리고 클래스의 이름은 아래 질문에 모두 답해야한다.
  - 변수(혹은 함수나 클래스)의 존재 이유는?
  - 수행 기능은? 
  - 사용 방법은?
- 따로 주석이 필요하다면 의도를 분명히 드러내지 못했다는 말임
### 그릇된 정보를 피하라
- 코드에 그릇된 단서를 남겨서는 안된다
- 나름대로 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용해도 안된다.
  - 예를 들어 hp, aix, sco는 적합하지 않다. 유닉스 플랙폼이나 변종을 가리키는 이름이기 때문이다
  - 여러 계정을 그룹으로 묶을 때, 실제 List가 아니라면 accountList라 명명하지 않는다. List는 프로그래머에게 특수한 의미로, 실제 List가 아니라면 프로그래머에게 그릇된 정보를 제공하는 셈이다. (실제로 List더라도 이름에 넣지 않느 것이 바람직한데, 이건 나중에 살펴보겠다.)
- 서로 흡사한 이름을 사용하지 않도록 주의한다.
- 유사한 개념은 유사한 표기법을 사용한다. 이것도 정보다. 일관성이 떨어지는 표기법은 그릇된 정보다.
### 의미 있게 구분하라
- 이름이 달라야 한다면 의미도 달라져야 한다.
- Info나 Data는 a, an, the와 마찬가지로 의미가 불분명한 불용어다. 불용어는 중복이다.
- 안좋은 예
  - getActiveAccount();
  - getActiveAccounts();
  - getActiveAccountlnfo();
- 읽는 사람이 차이를 알도록 이름을 지어라
### 발음하기 쉬운 이름을 사용하라
### 검색하기 쉬운 이름을 사용하라
- 숫자 상수값(1,2,3...)을 변수로 명명하고 검색이 쉽게하자 (검색하기 쉬운 이름이 상수보다 좋다)
- 이름의 길이는 범위 크기에 비례해야 한다(간단한 메서드에서 로컬 변수만 한 문자를 사용)
- 긴 이름이 짧은 이름보다 좋다
### 인코딩을 피하라
- 자바 프로그래머는 변수이름에 타입을 알릴 필요 없다. (옛 컴파일러는 타입을 점검하지 않아서 프로그래머가 타입을 기억할 단서가 필요했으나, 요즘 컴파일러는 타입을 기억하고 강제한다)
- 때로는 인코딩이 필요한 경우도 있다. 인터페이스 클래스와 구현 클래스!
  - 인터페이스 클래스 ex) ShapeFactory
  - 구현 클래스의 이름에 인코딩을 한다. ex) ShapeFactoryImp, CShapeFactory
### 자신의 기억력을 자랑하지 마라
- 독자가 코드를 읽으면서 변수 이름을 자신이 아는 이름으롭 변환해야 한다면 그 변수 이름은 바람직하지 못하다.
- 이는 일반적으로 문제 영역이나 해법 영역에서 사용하지 않는 이름을 선택했기 때문에 생기는 문제다.
### 클래스 이름
- 클래스 이름과 객체 이름은 명사나 명사구가 적합하다.
- Customer, WikiPage, Account, AddressParser 등이 좋은 예다.
- Manager, Processor, Data, Info 등과 같은 단어는 피하고, 동사는 사용하지 않는다.
### 메서드 이름
- 메서드 이름은 동사나 동사구가 적합하다.
- postPayment, deletePage, save 등이 좋은 예다
- 접근제어자, 변경자, 조건자는 javabean 표준에 따라 값 앞에 get, set, is를 붙인다.
- 생성자를 중복정의할 때는 정적 팩토리 메서드를 사용한다. 메서드는 인수를 설명하는 이름을 사용한다.
- `Complex fulcrumPoint =Complex.FromRealNumber(23.0);` 왼쪽 코드가 오른쪽 코드보다 좋다. `Complex fulcrumPoint new Complex(23.0);`
- 생성자 사용을 제한하려면 private으로 선언한다.
### 기발한 이름은 피하라
### 한 개념에 한 단어를 사용하라
- 추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다.
  - 똑같은 메서드를 클래스마다 fetch, retrieve, get으로 제각각 부르면 혼란스럽다.
  - 동일 코드 기반에 controller, manager, driver를 섞어쓰면 혼란스럽다.
- 일관성 있는 어휘를 사용하라
### 말장난을 하지 마라
- 한 단어를 두 가지 목적으로 사용하지 마라. 다른 개념에 같은 단어를 사용한다면 그것은 말장난에 불과하다.
  - 예를 들어, 지금까지 구현한 add 메서드는 모두가 기존 값 두 개를 더하거나 이어서 새로운 값을 만든다고 가정하자. 새로 작성하는 메서드는 집합에 값 하나를 추가한다. 이 메서드를 add라고 불러도 괜찮을까?
  - 새 메서드는 기존 add 메서드와 맥락이 다르다. 그러므로 insert나 append라는 이름이 적당하다. 새 메서드를 add라 부른다면 이는 말장난이다.
- 코드를 최대한 이해하기 쉽게 짜야한다. 집중적인 탐구가 필요한 코드가 아니라 대충 훑어봐도 이해할 코드 작성이 목표다.
### 해법 영역에서 가져온 이름을 사용하라
- 모든 이름을 문제 영역(domain)에서 가져오는 정책은 현명하지 못하다. 같은 개념을 다른 이름으로 이해하던 동료들이 매번 고객에게 의미를 물어야하기 때문이다.
- 기술 개념에는 기술 이름이 가장 적합한 선택이다.
  - VISITOR 패턴에 친숙한 프로그래머라면 AccountVisitor라는 이름을 금방 이해할 것이다.
  - 프로그래머에게 익숙한 기술 개념은 많다.
### 문제 영역(Domain)에서 가져온 이름을 사용하라
- 적절한 프로그래머 용어가 없다면 문제 영역에서 이름을 가져온다
- 해법 영역과 문제 영역을 구분할 줄 알아야한다.
- 문제 영역 개념과 관련이 깊은 코드라면 문제 영역에서 이름을 가져와야한다.
### 의미 있는 맥락을 추가하라
- 클래스， 함수， 이름 공간에 넣어 맥락을 부여한다. 모든 방법이 실패하면 마지막 수단으로 접두어를 붙인다.
  - 예를 들어 , firstName, lastName, street, houseNumber, city, state, zipcode 라는 변수가 있다. 변수를 훑어보면 주소라는 사실을 금방 알아챈다. 하지만 어느 메서드가 state라는 변수 하나만 사용한다면 변수 state가 주소의 일부라는 사실을 알아채기 어려울 수 있다.
  - addr라는 접두어를 추가해 addrFirstName, addrLastName, addrState라 쓰면 맥락이 좀 더 분명해진다. 변수가 좀 더 큰 구조에 속한다는 사실이 분명해진다. 물론 Address라는 클래스를 생성하면 더 좋다. 그러면 변수 가 좀 더 큰 개념에 속한다는 사실이 컴파일러에게도 분명해진다.
- 알고리즘(로직)도 맥락을 제공한다.
### 불필요한 맥락을 없애라
- 일반적으로 짧은 이름이 긴 이름보다 좋다. 단, 의미가 분명한 경우에 한해서다. 이름에 불필요한 맥락을 추가하지 않도록 주의한다.
- accountAddress와 customerAddress는 Address 클래스 인스턴스로는 좋은 이름이나 클래스 이름으로는 적합하지 못하다. Address는 클래스 이름으로 적합하다. 포트 주소， MAC 주소， 웹 주소를 구분해야 한다면 PostalAddress, MAC, URI라는 이름도 괜찮겠다. 그러면 의미가 좀 더 분명해진다.

## 소감
`문제 영역`과 `해법 영역`을 잘 구분하고, 영역별로 적합한 용어를 사용하기 위해 의식적으로 노력해야겠다. 
`맥락`이 다르다면 동일한 단어로 표현할 수 있어도 다른 단어로 그 맥락이 서로 다름을 명시해줘야겠다.

## 궁금한 것
예제 코드가 없이 말로 설명된 경우 다소 추상적으로 느껴지기 때문에 이해가 가지 않는 부분들이 있다. 하지만 이후 사례와 함께 코드가 제공된다고 하니 초반에 원칙을 설명할 때에는 예제가 상상이 가지 않더라도 읽어나갈 생각이다.
