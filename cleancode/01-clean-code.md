
# 1장 깨끗한 코드
## 책에서 기억하고 싶은 내용
### 들어가면서
#### 이 책의 구성
1. 깨끗한 코드를 작성하는 원칙, 패턴, 실기
2. 여러 사례 연구를 소개 (문제가 있는 코드를 문제가 더 적은 코드로 바꾸는 연습)
3. 결말. 사례 연구를 만들면서 수집한 냄새와 휴리스틱(heuristic)을 열거
  - 사례 연구에서 코드를 분석하고 정리하면서 행위의 모든 이유를 휴리스틱이나 냄새로 정리함
  - 코드를 분석하고 고치며 스스로 느끼는 감정을 분석하고, 그렇게 느끼는 이유와 그렇게 고치는 이유를 찾아내어 책으로 씀
  - 코드를 짜고, 읽고, 정리하는 관점에서 생각하는 방식을 묘사한 지식 기반으로 구축함
  - 사례 연구에서 코드를 변경할 때 마다 휴리스틱 번호를 표기함
#### 학습 방법
- 저자의 사고방식과 선택에 이유를 쫒기 위해서는 사례연구에서의 각 결정과 휴리스틱 관계 파악이 중요함 (둘 사이 관계 파악이 쉽도록 마지막 부록에 교차 참조표가 있음)
- 시간을 들여서 두번째 사례 연구를 검토하고, 모든 결정과 단계를 이해하고, 저자의 생각방식을 이해하려고 애써보자
- 이 책에서 제시하는 원칙과 패턴과 실기와 휴리스틱을 나만의 지식으로 만들기
### 대가들이 말하는 좋은 코드
- 좋은 코드를 사수하는 일은 프로그래머들의 책임이다.
- 기한을 맞추는 유일한 방법은 언제나 코드를 최대한 깨끗하게 유지하는 습관이다.
- 깨끗한 코드와 나쁜 코드를 구분할 줄 안다고 깨끗한 코드를 작성할 줄 아는 것은 아니다.
- 나쁜 코드는 너무 많은 일을 하려 애쓰다가 의도가 뒤섞이고 목적이 흐려진다. 깨끗한 코드는 한 가지에 집중한다. 각 함수와 클래스와 모듈은 주변 상황에 현혹되거나 오염되지 않은 채 한 길만 걷는다.
- 깨끗한 코드는 작성자가 아닌 사람도 읽기 쉽고 고치기 쉽다. (가독성 뿐만 아니라 고치기 쉬워야 한다!)
- 모든 테스트를 통과한다. 단위 테스트 케이스와 인수 테스트 케이스가 존재한다. (아무리 코드가 우아해도, 아무리 가독성이 높아도, 테스트 케이스가 없으면 깨끗하지 않음)
- 깨끗한 코드에는 의미있는 이름이 붙는다.
- 중복이 없다.
- 시스템 내 모든 설계 아이디어를 표현한다.
- 클래스, 메서드, 함수 등을 최대한 줄인다.
- 나는 여러 기능을 수행하는 객체나 메서드도 찾는다. 객체가 여러 기능을 수행한다면 여러 객체로 나눈다. 메서드가 여러 기능을 수행한다띤 메서 드 추출ExtractMeth어 리팩터 링 기법을 적용해 기능을 명확히 기술하는 메서드 하 나와 기능을 실제로 수행하는 매서드 여러 개로 나눈다.
- 중복을 줄이고, 한 기능만 수행하고, 제대로 표현하고, 작게 추상화하라
### 저자의 생각
- 이 책은 나와 내 동료들이 생각하는 바를 상세히 설명한다. 깨끗한 변수 이름, 깨끗한 함수, 깨끗한 클래스를 만드는 방법을 소개한다.
- 이 책은 오브젝토 멘토 진영이 생각하는 깨끗한 코드를 설명한다. 여기서 가르치는 교훈과 기법은 우리 진영이 믿고 실천하는 교리다. 우리가 가르치는 기법을 따른다면 깨끗하고 수준 높은 코드를 작성하리라 감히 장담한다.
- 우리 생각이 절대적으로 '옳다'라는 단정은 금물이다. 우리들 못지않게 경험 많은 집단과 전문가가 존재한다. 마땅히 그들에게서도 배우라고 권한다. 실제로도 이 책에서 주장하는 기법 다수는 논쟁의 여지가 있다.

## 소감
저자의 말처럼 이 책을 단순히 기분 좋은 책으로 여기지 않고, 생각을 쫒으며 그들의 사고방식과 의사결정을 이해하려고 노력해야겠다.
사람마다 `클린코드`를 정의하는 방식이 달라서 그런지, 1장은 추상적이고 모호하게 느껴졌다. 저자와 대가들이 말한 클린코드에 대해 코드와 사례 중심으로 얼른 읽고 싶다!

## 궁금한 것
- 이 책을 읽어가면 좋은 코드가 무엇이고 어떻게 짜야하는지 알 수 있겠지?
