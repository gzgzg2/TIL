# SOLID - SRP

## Responsibility

> 책임이란 한가지 일만 수행하는 것이 아닌, 변경의 원인을 말한다.
>
>
> 변경의 원인이 같은 것은 책임이 동일하다고 볼 수 있다.
>
> 특정 액터의 요구사항을 만족시키기 위한 일련의 함수의 집합
>

### It’s about Users

- SRP는 사용자에 관한 것
  - 어떠한 사용자에 의해 클래스가 변하는가
- 책임
  - SW의 변경을 요청하는 특정 사용자들에 대해 클래스/함수가 갖는 것
  - “변경의 근원”으로 볼 수 있음

### It`s about Roles

- User들을 그들이 수행하는 Role에 따라 나눠야
- User가 특정 Role을 수행할 때 Actor라고 부른다.
  - 책임은 개인이 아니라 액터와 연결

### Two Values of SW

- Primary and Secondary Values
- Secondary value of SW is it’s behavior
  - 현재의 SW가 사용자의 현재 요구사항을 만족하는가?
- Primary Value of SW
  - 지속적으로 변화하는 요구사항을 수용(tolerate, facilitate)하는 것
  - 대부분의 SW는 현재의 요구사항을 잘 만족하지만 변경하긴 어렵다.

### Collision


- Primary Value 저하
  - Policy Actors: business rule의 변경을 필요로 함
  - Architecture Actors: DB Schema 변경을 필요로 함
  - Operations Actors: Business Rule 변경을 원한다.
  - 동일 모듈의 변경. Merge 충돌 Source Reop 충돌

### Fan Out


- Employee는 너무 많은 것을 안다.
  - Business rules
  - DB
  - Report, formatting
- 많은 책임을 갖는다.
- 각각의 책임은 Employee가 다른 클래스들을 사용하도록 한다.

### Collocation is Coupling

- Operations Actor가 새로운 리포트 기능을 필요
  - 새로운 리포트 기능도 Employee 클래스에 추가
  - 기존 책임(Policy, Architecture)에는 변경이 없음에도 새로운 리포트가 추가되어 변경됨
  - 새로운 리포트 기능이 Employee 클래스에 추가되면 이 기능을 필요로 하지 않는 Employee 클래스를 사용하는 모든 클래스들이 다시 컴파일/배포되어야 함
  - 모든 액터들이 영향을 받게 된다.
    - “Collacation of responsibilites couples the actors”

## SRP

- 모듈은 하나/반드시 `하나의 변경 사유`를 가져야 한다.
- One and only one reponsibility
  - 동일한 이유로 변경되어야 하는 것들은 동일 모듈에,
  - 다른 이유로 변경되어야 하는 것들은 다른 모듈에
  - 세 개의 클라이언트가 있다면 각각의 클라이언트를 위해 각각의 클래스를 가지고 있어야 한다.
- 시스템 설계
  - Actor 파악에 주의해야 함
  - Actor들을 serve하는 책임들을 식별
  - 책임을 모듈에 할당
    - 각 모듈이 반드시 하나의 책임을 갖도록 유지하면서
  - 분리의 이유
    - 다른 이유로 인해 변경되고, 다른 때에 변경되기 때문

## Solution


**Inverted Dependencies**

- OOP에서 이런 의존성을 다루는 전략
  - 클래스를 인터페이스와 클래스로 분리
  - 중간에 Employee 인터페이스를 두어 소스코드의 의존성과 런타임 의존성을 분리
  - 다른 구현체가 변경되어도 인터페이스가 변경되지 않으면 클라이언트는 변경되지 않음
- Actor를 클래스에서 Decouple
  - 모든 Actor들이 하나의 인터페이스에 coupled
  - 하나의 클래스에 구현되어도 coupled


**Extract Classes**

- 3개의 책임을 분리하는 방법: 3개의 클래스로 분리
- 결과
  - Actor들은 분리된 3개의 클래스에 의존
  - 3개의 책임에 대한 구현은 분리
  - 하나의 책임의 변경에 다른 책임에 영향 안미침
- 문제점
  - 의존성 전이(EmployeeGateway/ExployeeReporter → Employee)
  - Employee 개념이 3개로 분리


**Extract Classes & Inverted Depedencies**

- 두개의 장점을 합치면 문제점을 최소화 할 수 있다.
- 의존성은 역전시키고 클라이언트마다 각각의 인터페이스를 만들어두면 의존성 전이 문제와 결합 문제를 해결할 수 있다.
- 단점으로는 추출한 인터페이스마다 구현해야 하고 이해하기 복잡해질 수 있는 문제가 있다.


**Facade - 어디에 구현이 있는지 찾기 쉽게**

- Put all 3 function families into a facade class
- Facade delegates to the different implemenations
- 장점
  - 어디에 구현되었는지 찾기 쉽다.
- 단점
  - Actor들은 여전히 Coupled

## Case Study

## Faking It