# RESTful API를 위한 6가지 원칙과 네이밍
더 예쁘게 정리된 [블로그 포스팅](https://prohannah.tistory.com/156) 보러가기 :)

## REST와 RESTful
REST는 `REpresentational State Transfer` 의 약어로, 클라이언트과 서버가 데이터를 주고 받는 방식에 대한 아키텍처 스타일이다. REST에는 여섯 가지의 기본 원칙이 있고, 이 가이드를 준수한 인터페이스는 `RESTful`하다고 표현한다.
> 참고문서 : [What is REST](https://restfulapi.net/)
1. **Client–server** 구조 – 클라이언트와 서버는 서로 독립적이어야 하며, 클라이언트는 오직 URIs 리소스만 알아야한다. 클라이언트와 서버의 인터페이스가 변경되지 않는 한, 이 둘은 독립적으로 개발되거나 대체될 수 있게 유지해야한다. (관심사의 명확한 분리)
2. **Stateless(무상태성)** – 클라이언트의 모든 요청에는 해당 요청을 이해할 수 있는 모든 정보가 포함되어야한다. 서버는 HTTP 요청에 대한 어떤 것도 저장하지 않는다. 컨텍스트를 유지해야하는 세션, 인증과 인가에 대한 정보 또한 클라이언트에만 보관되며, 각 요청 시 해당 정보를 모두 포함하여 서버에 요청한다. 
3. **Cacheable** – 서버는 Response cache-control 헤더에 해당 요청이 캐싱이 가능한 지에 대한 여부를 제공해야 한다. 이를 제공한다면 클라이언트는 Response를 캐싱하여 서버와 클라이언트 간의 상호작용을 줄이고, 성능과 서버 가용성을 늘릴 수 있다.
4. **Uniform interface(일관된 인터페이스)** – 보편적인 소프트웨어 엔지니어링 원칙을 `component interface`에 적용하면, 전체적인 시스템 아키텍처는 단순화되고 각 상호작용에 대한 가시성이 개선된다. 이름 그대로 일관된 인터페이스를 얻기 위해 REST에서는 4가지 인터페이스 규칙을 정의하였다.
    - identification of resources : 요청 시 개별 자원을 식별할 수 있어야함
    - manipulation of resources through representations : 어떤 자원에 대헤 작업하기 위한 적절한 표현과 메타데이터를 충분히 갖추고 있다면, 서버는 해당 자원을 변경, 삭제할 수 있는 정보를 가지고 있단 의미
    - self-descriptive messages : 자신을 어떻게 처리해야하는지 정보를 포함해야함
    - hypermedia as the engine of application state : 단순히 결과 뿐만이 아니라 결과에 대한 정보를 포함해야 함
5. **Layered system(다중 계층)** – REST는 다중 계층 구조를 가질 수 있도록 허용한다. (예를 들어 API 서버와 DB서버 그리고 인증 서버를 따로 둘 수 있도록). 그리고 각 레이어는 자기와 통신하는 컴포넌트 외 레이어에 대해서는 정보를 얻을 수 없다. 클라이언트는 REST 서버와만 상호작용할 뿐, REST가 상호작용하는 레이어나 그 외 중간 레이어들, end server에 직접적으로 요청할 수 없으며 이들의 상호작용 또한 볼 수 없다.
6. **Code on demand (optional)** – 서버가 클라이언트에서 실행시킬 수 있는 로직을 전송하여 클라이언트의 기능을 확장시킬 수 있다. 이를 통해 클라이언트가 사전에 구현해야하는 기능의 수를 줄여 간소화시킬 수 있다.

## RESTful API Naming
- URI(Uniform Resource Identifier)
  - Uniform: 리소스 식별하는 통일된 방식
  - Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
  - Identifier: 다른 항목과 구분하는데 필요한 정보
- URI는 Resource(or Representation)만 식별함
- ex) 미네랄을 캐라 -> 미네랄이 리소스, 회원을 등록하고 수정하는 기능 -> 회원이라는 개념 자체가 리소스
- 최근에는 Resource 대신 Representation 라는 표현을 사용하기 시작함
- 하지만 아직 Resource라는 표현을 쓰는 비중이 많으니 이 글에서는 Resource와 Representation를 함께 표기하겠다.

> 참고문서 : [REST Resource Naming Guide](https://restfulapi.net/resource-naming/)

### 1. 리소스를 표현하기 위해 명사를 사용하라
RESTful URI이 가르키는 resource(or representation)는 수행되는 행위가 아니라 객체다. 
이 resource (or representation)를 네 가지 범주로 나누어보겠다. 
항상 리소스가 어느 범주에 해당하는 지 확인하고, 그 범주에 맞는 네이밍 컨벤션을 일관되게 사용하라.

**문서(Document)**
- 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
- 단수 사용 (/device-management, /user-management)
```
http://api.example.com/device-management/managed-devices/{device-id}
http://api.example.com/user-management/users/{id}
http://api.example.com/user-management/users/admin
```


**컬렉션(Collection)**
- 서버가 관리하는 리소스 디렉터리
- 서버가 리소스의 URI를 생성하고 관리
- 복수를 사용하라
- POST 기반 등록
- 예) 회원 관리 API
- 복수 사용 (/users)
    ```
    http://api.example.com/user-management/users
    http://api.example.com/user-management/users/{id}
    ```

**스토어(Store)**
- 클라이언트가 관리하는 자원 저장소
- 클라이언트가 리소스의 URI를 알고 관리
- PUT 기반 등록
- 예) 정적 컨텐츠 관리, 원격 파일 관리
- 복수 사용 (/files)
    ```
    http://api.example.com/files
    http://api.example.com/files/new_file.txt
    ```

**컨트롤 URI 혹은 컨트롤러(Controller)**
- 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
- 동사를 직접 사용
- 예) GET, POST만 사용할 수 있는 HTTP FORM의 경우 컨트롤 URI(동사로 된 리소스 경로)를 사용 ex) /members/delete, /members/new 등
- 동사 사용 (/checkout, /play 등)
    ```
    http://api.example.com/cart-management/users/{id}/cart/checkout
    http://api.example.com/song-management/users/{id}/playlist/play
    ```

### 2. 일관성이 핵심이다
일관된 resource (or representation) naming conventions과 URI 형식을 사용하면 모호함이 최소화되고 가독성과 지속성이 극대화된다. 일관성을 위해 다음과 같은 디자인 힌트를 구현할 수 있다.
- 계층 관계 표현을 위해  `/`를 사용하라
    ```
    http://api.example.com/device-management
    http://api.example.com/device-management/managed-devices
    http://api.example.com/device-management/managed-devices/{id}
    http://api.example.com/device-management/managed-devices/{id}/scripts
    http://api.example.com/device-management/managed-devices/{id}/scripts/{id}
    ```
- 마지막 문자로 `/` 를 사용하지 마라
    ```
    http://api.example.com/device-management/managed-devices/ /* X */
    http://api.example.com/device-management/managed-devices 	/* O */
    ```
- 가독성을 위해 `-`(하이픈)을 사용하라
    ```
    http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
    http://api.example.com/inventory-management/managedEntities/{id}/installScriptLocation  //Less readable
    ```
- `_` (언더스코어)는 사용하지 마라  
    일부 브라우저나 화면에서는 `_` (언더스코어)는 가려질 수 있기 때문에 혼란을 피하기 위해 `-` (하이픈)를 사용한다
    ```
    http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
    http://api.example.com/inventory_management/managed_entities/{id}/install_script_location  //More error prone
    ```
- 소문자를 사용하라  
    Schem과 HOST에만 대소문자 구별이 없고, 이 외에는 대소문자가 구별된다.
    아래 3개의 URL는 대소문자 구별이 없다면 모두 동일한 URL지만, 1번과 2번은 동일하고 3번은 같지 않다.
    ```
    http://api.example.org/my-folder/my-doc  //1
    HTTP://API.EXAMPLE.ORG/my-folder/my-doc  //2
    http://api.example.org/My-Folder/my-doc  //3
    ```

- 파일 확장자를 사용하지 마라  
    가독성에 좋지 않고 어떤 장점도 있지 않다.
    ```
    http://api.example.com/device-management/managed-devices.xml  /*Do not use it*/
    http://api.example.com/device-management/managed-devices 	/*This is correct URI*/
    ```

### 3. CRUD 함수명을 사용하지 마라
URI는 어떤 동작이 수행되는 지 가르키는 게 아니라, 리소스를 가르키는 것이다. 
리소스에 대한 작업은 HTTP Method를 이용하도록 한다.
```
HTTP GET http://api.example.com/device-management/managed-devices  //Get all devices
HTTP POST http://api.example.com/device-management/managed-devices  //Create new Device

HTTP GET http://api.example.com/device-management/managed-devices/{id}  //Get device for given Id
HTTP PUT http://api.example.com/device-management/managed-devices/{id}  //Update device for given Id
HTTP DELETE http://api.example.com/device-management/managed-devices/{id}  //Delete device for given Id
```

### 4. 필터를 위해 쿼리 파라미터를 사용해라
Resource(or Reperesentation)에 대한 정렬, 필터링, 페이징은 신규 API를 생성하지 않고 쿼리 파라미터를 활용해라.
```
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices?region=USA
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ&sort=installation-date
```