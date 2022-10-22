# Funtion Structure - 1

### 생성자의 인자가 많을 경우

- 밥 아저씨는 이 경우 차라리 java beans pattern을 사용하는 것이 낫다고 말함
    - 그러나 setter는 필요한 모든 값이 입력되기 전엔 불안전한 상태임
- 이펙티브 자바에서는 인자가 많을 경우 Builder pattern을 사용하라고 하였음
    - 불변 객체를 생성할 수 있음
    - Builder의 멤버변수를 final로 설정할 경우 필수값을 빼먹지않고 할당 가능함

### Boolean 인자 사용 금지

- Boolean을 인자로 사용하는 경우 2가지 이상의 일을 하는 것과 같다.
- true 경우를 위한 일과 false 경우를 위한 일을 2개의 함수로 분리하여라
    - 메소드 이름을 잘 정해서 분리했을 경우 메소드 바디를 읽지않아도 됨

### 입력값을 출력값으로 사용하지 말아라

- 인자는 입력값으로 사용되는 것이다. 인자를 출력으로 사용하지 말아라
    - 보통 입력값이 출력될 것이라고 기대하는 경우는 거의 없음
- 입력값의 상태를 변경하는 경우 return이 void여야 한다.
- 인자를 리턴할 경우 로컬 변수안에 할당하고 그 로컬 변수를 리턴하여라

### Null을 전달하거나 기대하지 말라

- null을 전달하는 함수는 위험하다. 이 경우 함수 내부에서 null을 필수로 검증해야 한다.
- null을 기대하는 함수 또한 마찬가지이다. 외부 API가 아니라 팀원과의 협업에선 null을 절대 반환하거나 기대하지 말아라.
- null을 전달하거나 기대하는 함수는 null인 경우와 null이 아닌 경우의 행동을 수행한다는 것과 같다.
- null 조사를 하는 것은 어떻게 보면 팀원에 대한 믿음이 없다는 것
- null은 단위 테스트에서 검증하여라

### Switch를 피해야하는 이유

- **Switch 문장은 많은 의존성을 가짐**
    - OO의 가장 큰 이점 중 하나는 의존성 관리 능력이다.
        - 다형성 인터페이스를 사용해서 상위 클래스와 하위 클래스의 결합을 낮출 수 있음
        - 상위 클래스는 구현체에 의존하지 않고 인터페이스에 의존함 → 구현체가 변경되어도 상위 클래스는 영향을 받지않음
    - DIP를 지키면 얻을 수 있는 장점
        - 독립적으로 컴파일이 가능함
        - 독립적으로 개발이 가능함 (상위 클래스 개발자와 하위 클래스를 담당하는 개발자가 각각 독립적으로 작업이 가능함)
- Switch는 fan-out problem 문제가 발생할 수 있는 의존을 가짐
- Switch 문장에서 Soruce Code 의존성은 flow of control과 방향이 같다.
    - 모든 외부 모듈에 영향을 받는다.
    - 외부 모듈 하나라도 변경이 일어나면 Switch 문장이 영향을 받음
        - switch에 의존하는 다른 모듈또한 영향을 받음

### Switch 문장 제거하기

- switch 문장을 다형성 인터페이스 호출로 변환
- case에 있는 문장들을 별도의 클래스로 추출하여 변경 영향이 발생하지 않도록 한다.

<img width="752" alt="image" src="https://user-images.githubusercontent.com/56028408/197344973-cc2dd0ea-d10c-4df7-8747-6275f57f90d5.png">
- `UserController`는 `UserService(interface)`에 의존하고 있다. 
- 구현이 아닌 인터페이스에 정의에 의존하고 있기 때문에 인터페이스 정의가 변경되지 않으면 UserController는 영향을 받지 않는다. 


### Application Main Partition

![image](https://user-images.githubusercontent.com/56028408/197344250-669af06d-1f74-429e-add3-d76753ac214e.png)
- 항상 모듈 다이어그램에서 App 파티션과 Main 파티션 사이에 선을 그울 수 있어야함
- 단반향 의존성을 가져야함
- Main Patition은 App Patition의 Plugin 같은 역할
    - Spring으로 예를 들면 Main Partition의 Main은 Bean Factory가 될 수 있음
    - Main은 인터페이스에 대한 구현체를 App Patition 클래스에 주입해주는 역할
- App Patition
    - 애플리케이션 코드가 존재하는 곳
- Main Patition
    - 하위레벨 코드가 존재하는 곳