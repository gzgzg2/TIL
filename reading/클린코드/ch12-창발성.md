# 창발성

### 창발적 설계로 깔끔한 코드를 구현하자
우리들 대다수는 켄트 벡이 제시한 단순한 설계 규칙 네 가지가 소프트웨어 설계 품질을 크게 높여준다고 믿는다.
```text
1. 모든 테스트를 실행한다.
2. 중복을 없앤다.
3. 프로그래머 의도를 표현한다.
4. 클래스와 메서드 수를 최소로 줄인다.
```
위 목록은 중요도 순이다.

### 단순한 설계 규칙 1: 모든 테스트를 실행하라
- 테스트가 불가능한 시스템은 검증도 불가능하다.
- 가장 기본적인 것은 의도한 대로 돌아가는 시스템을 내놓는 것이다.
- 문서로 시스템을 완벽하게 설계하여도 검증할 간단한 방법이 없다면 문서 작성에 투자한 노력을 인정받기 힘들다.
- 테스트가 가능한 시스템을 만들려고 애쓰면 설계 품질이 더불어 높아진다. 테스트 케이스가 많을수록 개발자는 테스트가 쉽게 코드를 작성한다.
- "테스트 케이스를 만들고 계속 돌려라"라는 간단하고 단순한 규칙을 따르면 시스템은 낮은 결합도와 응집력이라는 객체 지향 방법론이 지향하는 목표를 달성한다.

### 단순한 설계 규칙 2~4: 리팩터링
- 테스트 케이스를 모두 작성했다면 코드와 클래스를 정리할 차례이다.
  - 코드를 점진적으로 리팩터링 해나가기.
- 새로 추가하는 코드가 설계 품질을 낮추는 것 같을 때에는 코드를 정리한 후 테스트 케이스를 돌려 기존 기능을 깨트리지 않았다는 것을 확인한다.
- 리팩터링 단계에서는 소프트웨어 설계 품질을 높이는 기법이라면 무엇이든 적용해도 괜찮다. 테스트 케이스가 있다는 가정하에

### 중복을 없애라
- 우수한 설계에서 중복은 커다란 적이다. 중복은 추가 작업, 추가 위험, 불필요한 복잡도를 뜻하기 때문이다.
- 똑같은 코드는 당연히 중복이고 구현 중복도 중복의 한 형태이다.
- 깔끔한 시스템을 만들려면 단 몇 줄이라도 중복을 제거해야겠다는 의지가 필요하다.

```java
// 구현 중복 예시
int size() {}
boolean isEmpty() {}

// 구현 중복 제거하기
boolean isEmpty() {
    return 0 == size();    
}
```

**구현이 중복된 경우 예시**

```java
public void scaleToOneDimension(
        float desireDimension, float imageDimension) {
    if (Math.abs(desiredDimension - imageDimension) <errorThreshold)
        return;
    float scalingFactor = desiredDimension / imageDimension;
    scalingFactor = (float) (Math.floor(scalingFactor * 100) * 0.01f);
    
    RenderedOp newImage = ImageUtilities.getScaledImage (
            image, scalingFactor, scalingFactor);
    image.dispose();
    System.gc();
    image = newImage;
}

public synchronized void rotate(int degrees) {
      RenderedOp newImage = ImageUtilities.getScaledImage(
        image, degrees);
      image.dispose();
      System.gc();
      image = newImage;
}
```
scaleToOneDimension 메서드와 rotate 메서드는 일부 코드가 동일하다. 다음과 같이 코드를 정리해 중복을 제거한다.

```java
public void scaleToOneDimension(
        float desireDimension, float imageDimension) {
    if (Math.abs(desiredDimension - imageDimension) <errorThreshold)
        return;
    float scalingFactor = desiredDimension / imageDimension;
    scalingFactor = (float) (Math.floor(scalingFactor * 100) * 0.01f);

    replaceImage(ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor));
}

public synchronized void rotate(int degrees) {
      replaceImage(ImageUtilities.getScaledImage(image, degrees));
}

private void replaceImage(RenderedOp newImage) {
    image.dispose();
    System.gc();
    image = newImage;
}
```
- 중복된 코드를 새 메서드로 추출하고 나면 클래스 설계의 문제점을 조금 더 쉽게 파악할 수 있다.
  - 위 예제에선 아주 적은 양이지만 공통적인 코드를 새 메서드로 뽑고 보니 클래스가 SRP를 위반한다.
    - 이 경우 추출한 메서드를 새로운 클래스로 옮기는 방법을 선택할 수 있다.
    - 다른 팀원이 새 메서드를 조금 더 추상화 하여 다른 맥락에서 재사용할 기회를 포착할지도 모른다.
    - 이런 소규모 재사용은 시스템 복잡도를 극적으로 줄여준다.

### 고차원 중복 제거
TEMPLATE METHOD 패턴은 고차원 중복을 제거할 목적으로 자주 사용하는 기법이다.

```java
public class VacationPolicy {
    public void accrueUSDivisionVacation() {
        // 지금까지 근무한 시간을 바탕으로 휴가 일수를 계산하는 코드
        // 휴가 일수가 미국 최소 법정 일수를 만족하는지 확인하는 코드
        // 휴가 일수를 급여 대장에 적용하는 코드
    }
    
    public void accrueEUDivisionVacation() {
        // 지금까지 근무한 시간을 바탕으로 휴가 일수를 계산하는 코드
        // 휴가 일수가 유렵연합 최소 법정 일수를 만족하는지 확인하는 코드
        // 휴가 일수를 급여 대장에 적용하는 코드
    }
}
```
최소 법정 일수를 계산하는 코드만 제외하면 두 메서드는 거의 동일하다. 최소 법정일 수를 계산하는 알고리즘은
직원 유형에 따라 살짝 변한다. 여기에 `TEMPLATE METHOD 패턴`을 적용해 눈에 들어오는 중복을 제거한다.

```java
abstract public class VacationPolicy {
    public void accrueVacation() {
        calculateBaseVacationHours();
        alterForLegalMinimums();
        applyToPayroll();
    }
    
    private void calculateBaseVacationHours() {}
    abstract protected void alterForLegalMinimums();
    private void applyToPayroll();
}
```
`TEMPLATE METHOD 패턴`은 흐름은 동일한 상태에서 특정 부분의 구현이 다를 경우 적합하다. 위 예제는
최소 법정일 수를 계산하는 알고리즘에 대한 구현만 차이가 나고 흐름은 동일하기 때문에 `TEMPLATE METHOD 패턴`을 사용해서 개선할 수 있다.
VacationPolicy는 하위 클래스에게 중복되지 않는 정보만 제공해 accrueVacation() 메소드에서 빠진 구멍(alterForLegalMinimums())을 메운다.


### 표현하라
- 자신이 이해나는 코드를 짜기는 쉽다. 하지만 나중에 코드를 유지보수 하는 사람이 코드를 짜는 사람만큼 문제를 깊이 이해할 가능성은 희박하다.
- 시간이 지나 시스템이 복잡해져가면 유지보수 개발자가 시스템을 이해하느라 보내는 시간은 점점 늘어난다. 그러므로 코드는 개발자의 의도를 분명히 표현해야 한다.
  - 소프트웨어 프로젝트 비용 중 대다수는 장기적인 유지보수에 들어간다.
  - 개발자가 코드를 명백하게 짤수록 다른 사람이 그 코드를 이해하기 쉬워진다.

**이해하기 쉽도록 표현하기**
1. 좋은 이름을 선택한다. 이름과 기능이 완전 딴판인 클래스나 함수로 유지보수 담당자를 놀라게 하지 말자
2. 함수와 클래스 크기는 가능한 줄인다. 작은 클래스와 작은 함수는 이름 짓기도 쉽고 구현하기도 쉽고, 이해하기도 쉽다.
3. 표준 명칭을 사용한다. 예를 들어 디자인 패턴은 의사소통과 표현력 강화가 주 목적이다. 클래스가 표준패턴을 사용하여 구현된다면 클래스명에 패턴 이름을 넣어준다.
4. 단위 테스트를 꼼꼼히 작성한다. 테스트 케이스는 소위 `예제로 보여주는 문서`다. 다시 말해, 잘 만든 테스트 케이스를 읽어보면 클래스 기능이 한눈에 들어온다.
5. 마지막으로 표현력을 높이는 가장 중요한 방법은 노력이다. 자신의 작품에 조금만 더 주의를 기울이자.


### 클래스와 메서드 수를 최소로 줄여라.
- 때로는 무의미하고 독단적인 정책 탓에 클래스 수와 메서드 수가 늘어나기도 한다. 가능한 독단적인 견해는 멀리하고 실용적인 방식을 택한다.
  - 목표는 함수와 클래스 크기를 작게 유지하면서 동시에 시스템 크기도 작게 유지하는데 있다.
    - 하지만 이 규칙은 간단한 설계 규칙 네 개 중 우선순위가 가장 낮다.
      - 클래스와 함수 수를 줄이는 작업도 중요하지만, 테스트 케이스를 만들고 중복을 제거하고 의도를 표현하는 작업이 더 중요하다.