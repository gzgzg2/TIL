# Architecture.1

## What is Architecture

- 시스템 개발에 기반을 제공하는 변경 불가한 초기 결정사항의 집합
  - Java
  - Eclipse
  - Spring, Tomcat, Hibernate:
  - MySQL, MVC
- 건축의 아키텍처가 해머, 못 등이 아니듯 위에 정의된 것들은 도구일 뿐
- 툴, Building Material 등이 아니라 사용법(Usage)에 대한 것
- 결정 미루기, 핵심 추상화 !!

## What is Use Case

- 시스템이 무엇을 하는지 나타내는 가장 좋은 방법
- 객체지향 분석 설계할 때 actor(시스템 사용자의 행동)를 먼저 찾고 사용자의 useCase에 맞춰 설계하는 방법

## Architecture Exposes Usage

- 아키텍처는 사용법에 대해서 설명
  - 어떤 기술을 사용했는지가 아키텍처가 아님
  - 시스템의 재료를 설명하는 것이 아니라 시스템이 무엇을 하는지 설명해야 함
- MVC 구조만 있는 웹시스템
  - Use case를 숨기고 Delivering 메커니즘만 노출
  - 중요한 것은 Delivering 메커니즘이 아니라 Use case
  - Use case와 Delivering 메커니즘에 decoupled 되어야 함
  - UI, DB, F/W Tools 등에 대한 결정들이 Use case와는 완전히 decoupled 되어야 함
- Use case should stand alone

## Deferring Decisions

- 좋은 아키텍처는 FW, WAS, UI 등과 같은 stuff들에 대한 결정을 연기하는 것을 허용
  - 이것이 좋은 아키텍처의 주요한 목적 중 하나
  - 미래를 알 수 없기 때문에 기술에 대한 결정을 미뤄도 문제가 되지 않아야 함
  - 기술을 먼저 결정하고 나면 아키텍처 설계의 폭이 좁아진다.
  - 기술을 결정하지 않고도 애플리케이션 로직을 작성할 수 있다.
    - 그러려면 인프라와 애플리케이션 계층이 독립적으로 분리되어야 할듯
    - 애플리케이션 로직이 완성될 때 결정하려고 했던 기술보다 더 단순한 해결책을 발견하게 될 수도 있다.

## Central Abstaction

- 많은 아키텍트는 DB를 핵심 추상화라고 생각하여 DB가 준비되기 전에는 어떠한 생각, 작업도 하지 않는다.
  - 실제로는 행동이 제일 중요?
  - 데이터로부터 아키텍처를 고민하게 된다면 절차지향적인 코드가 작성된다.
    - 절차지향적인 코드는 빠른 개발이 가능하지만 확장에 무리가 있다.
- 런타임 의존성은 DB가 맞지만 코드는 아니다.
- Fitness 프로젝트의 코어 추상화 → 위키 페이지
  - DB를 연기할 수 있는 Detail로 간주
  - 좋은 아키텍처는 Tool, FW로 구성되는 것이 아니다.
- 어떻게 하면 연기할 수 있나?
  - 연기하고 싶은 것에서 구조를 decouple, irrelevant 하게 설계
- 어떻게 하면 Tool, FWs, DB에서 decouple 하나?
  - 아키텍처 측면에서 SW 환경이 아닌 Use Case에 집중
- 컨트롤러나 서비스로부터 로직을 시작하게 되면 커질 수밖에 없다. 프레임워크의 힘이 아닌 개발자가 직접 new 할 수 있는 객체가 많은 비지니스 로직을 가져야 테스트 하기 수월해진다.

## Conclusion

- 아키텍처를 Use Case에 집중
  - 기술에 대한 연기는 선택을 최대한 오래 열어 둘 수 있음.
  - 필요에 따라 결정을 변경할 수 있는 것
  - 프로젝트 진행 중에 충분한 정보가 생김에 따라 undo에 대한 비용 없이 여러번 변경할 수 있다. (기술적인 것들이 정해지지 않았기 때문에 변경이 수월함)
  - 프로젝트 윤곽이 들어나는 시점에 들어오는 요구사항을 받아드려야함. 받아드리지 않으면 사용자 요구사항을 무시하는 것과 같음
