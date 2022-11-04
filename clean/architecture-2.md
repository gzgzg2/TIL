# Architecture.2

## 📌 Domain Model 개발 절차

### 1. 클래스 속성, 관계 식별 - 데이터에 대한 모델링

- 도메인 영역의 주요 개념(명사)를 식별
- “Applying UML and Patterms” 참조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/08cf4597-119d-4970-b77c-ada94fd2e78d/Untitled.png)

### 2. 도메인 모델에 행위 추가하기 - 행위에 대한 모델링

- 도메인 모델에 행위를 추가함으로써 도메인 모델에 생명력을 부여
- 도메인 모델의 행위를 결정하기 위해 도메인 모델의 책임(Responsibility)과 상호작용(Collaboration)을 식별
- 클래스의 책임
  - 클래스가 아는 것(속성, 관계), 하는 것, 결정 하는 것 등
- 클래스의 상호작용
  - 책임 수행을 위해 호출하는 다른 클래스들

✔️ **책임과 상호작용을 식별하는 절차**

- 요구 사항(유스케이스, 유저스토리, UI 디자인) 분석을 통해 어플리케이션이 처리해야 하는 요구사항 식별
- 도메인 모델의 클라이언트에게 도메인 모델을 노출하기 위한 도메인 모델의 인터페이스 결정
- 해당 인터페이스를 각각의 요구사항을 고려하여 TDD 접근법으로 구현

**✔️ 요구 사항 식별하기**

- 처리해야 할 요구사항 식별/ 어떻게 응답할 지 결정
- UI 디자인, 유스케이스, 유저스토리 등을 분석해서
- 요구 사항은 2가지 부분으로 구성
  - 사용자 행위 (command)
  - 사용자 행위 요청에 대한 어플리케이션의 응답 (query)
- 어플리케이션의 책임은 2가지로 그룹핑
  - 사용자 입력 검증, 값 계산, 데이터베이스 갱신
  - 값 출력

**✔️ TDD로 메소드 구현하기**

- 대상 서비스 메소드에 대해 하나 이상의 테스트케이스를 작성하는 것으로 시작
  - 각 테스트케이스는 서로 다른 상황을 재현하기 위해 다른 인자로 구성된다.
    - 경계 값으로 테스트가 가능하다면 경계값으로 테스트하는 것도 좋은 방법이 될 듯
- Mock 객체를 이용해서
  - service methods → repository methods 순으로 top-down 방식으로 구현
    - stubbing을 활용하면 하위 레이어의 클래스가 구현되지 않아도 상위 레이어의 테스트 코드를 작성할 수 있음
  - 구현을 하다가 발견되는 collaborator를 구현하기 위해 머릿속에서 context-switching이 일어날 필요가 없어서 집중하면서 구현이 가능함

**POJOs In Action**

## 📌 Architecture Use Case

✔️ **Web System에 만연하는 MVC Architecture**

> Web 기반 Accounting 시스템을 설계해야 한다면 여기서 중요하게 생각해야 되는 부분은 Web 시스템이 아니라 Accounting 시스템이다. 하지만 대개의 Web 시스템은 Web Issue에 대해서 고함치고, 비즈니스 의도에 대해서는 거의 언급하지 않는 경우가 대부분이다. SW 아키텍처는 Accounting Issue를 드러내고 Web에 대해서는 거의 언급이 없어야 한다.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f32fd744-bbdb-4a33-8ff1-3e8272a2190d/Untitled.png)

- View/Controller: 강하게 HTML과 연관
- Model: Controller에 강하게 연관
- View/Controller: 강하게 모델에 coupling - 모델 구조에 많은 영향을 미친다.

**✔️ 요구사항 변경에 유연하게 대응하려면**

- Archtiecture 변경 없이 delivery 메커니즘(web 이나 기타 등등)을 변경할 수 있어야 한다.
  - 동일한 시스템을 웹과 콘솔에 deliver 할 수 있어야 함
  - 이 두 시스템의 아키텍처는 동일해야 함

✔️ **Use Case**

- “Object-Oriented Software Engineering - A Use Case Driven Approach”, lavr Jacobson
  - Delivery 문제를 우아한 아키텍쳐로 해결
  - Delivery와 무관한 방식으로 사용자가 시스템과 상호작용하는 방식을 이해하는 것
  - 링크, 버튼, 클릭 등의 용어를 사용하지 않고 표현
  - Delivery 메커니즘을 나타내지 않는 용어를 사용
  - 제이콥슨은 이런 상호작용을 **Use Case** 라고 하였음
  - 어플리케이션 개발은 Delivery와 독립적인 Use Case에 의해 주도되어야 한다.
  - use case가 시스템에서 가장 중요한 것!!
- 하지만 보통 Web System을 개발하는데 다양한 Delivery 메커니즘을 수용할 수 있게 개발해야 하나?
  - Web System만 개발하더라도 Use Case와 Delivery 메커니즘이 분리되어야 요구사항 변경에 대응 가능하다.  Delivery 메커니즘이 변경되더라도 나의 시스템이 안전한 것이 중요함
  - Delivery 메커니즘과 Use Case가 분리되어야 독립적인 배포가 가능하다. 여기서 독립적인 배포란 독립적인 컴파일을 의미한다.

### 📌 Use Case Driven Architecture

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcdc1281-643d-4773-bcf4-8ae4feb49a86/Untitled.png)

- use case driven 시스템의 아키텍처를 보면 delivery 메커니즘이 아닌 use case를 보게됨
  - 독자가 보게 되는 것은 **시스템의 의도**이다.
    - 개발자는 항상 사용자 편의성을 생각하고 개발해야 한다는 것이 갑자기 떠올랐음 ..

✔️ **Use Cases**

- 사용자가 특정 목적을 이루기 위해 시스템과 어떻게 상호작용하는지에 대한 형식적 기술
  - 인수 테스트의 시나리오와 비슷한가?
  - 시나리오는 예외와 기본적인 시나리오 두 가지로 나뉠 수 있음
- 목적: 주문처리 시스템에서 새로운 주문을 생성하는 것
- 시스템으로 들어가는 데이터, 커맨드와 시스템이 응답하는 것만 언급
- Delivery 메커니즘과 무관한 아키텍처를 가지려면 Delivery 메커니즘과 무관한 use case로 시작해야 한다.
- Use case의 응답도 주목
  - oder-id는 완전하게 delivery 메커니즘과 무관
  - human clerk은 order-id를 볼 필요가 없다.
  - 반면 delivery 메커니즘은 팝업을 띄워서 사용자에게 항목을 주문에 추가하라고 요청할 수 있다.
  - delivery 메커니즘에 중립적인 관계를 취할 수 있게 설계?

**✔️ Use Case Algorithm**

- Use case 객체에 비즈니스 로직을 위치시켜야 한다.

**✔️ Partitioning**

- 야콥슨: 아키텍처는 3개의 Fundamental Kinds Of Ojbect들을 갖는다.
  - Business Objects (BO, Entity) - Entities라고 불리는
  - UI Objects (DTO) - Boundaries라고 불리는
  - Use Case Objects (Service?)- Controller라고 야콥슨이 부른, 하지만 MVC와 혼동을 피하기 위해 Interactors 라고 부를 것
- Entities have:
  - Application과 독립적인 비지니스 로직
  - 다른 application에서도 entity들은 사용됨
  - 특정 Application에 특화된 메소드를 갖으면 안됨
  - 도메인이 같으면 무조건 같은 로직을 비지니스 로직이라고 부름
    - 주문을 예시로 들면, 주문에 Order와 Customer는 어디든 포함이 됨.
      - 여기서 Order와 Customer가 Entity가 될 수 있음
      - 주문은 Application마다 방식이 달라질 수 있으므로 Application 로직에 적합함
- Interactors have:
  - Application 종속적인 비지니스 로직
  - 특정 Application에 특화된 메소드들은 Interactor 객체에 구현
  - 같은 도메인인데 어떤 시스템이냐에 따라서 달라지는 부분을 Application 로직이라고 할 수 있음.