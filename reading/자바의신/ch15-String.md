# String
> String 클래스는 Serializable, Comparable, CharSequence 인터페이스를 구현한  문자열을 다루는 클래스이다. String 클래스로 생성한 문자열은 불변하다는 특징과 내부에서 final로 선언되어 있기 때문에 확장이 불가능하다는 특징이 있다.

### String 연산을 남발하면 위험한 이유
기존 적으로 `String`은 불변하다는 특징을 가지고 있다. 동일한 문자를 생성할 경우 `String pool`에 저장되어 있는 것을 참조하지만,
기본 `String class`를 연산하게 되면 매번 새로운 객체가 생긴다. 이는 GC를 자주 발생시킬 수 있다는 문제점이 있다. 자바에서 GC가 동작할 때
JVM 애플리케이션이 멈추기 때문에 잦은 GC 발생은 프로그램 성능에 영향을 줄 수 있다.

## StringBuilder
- StringBuilder는 append() 메소드를 이용하여 문자열을 더할 경우 새로운 객체를 생성하지 않고 문자열을 더한다.
- 그러나 ThreadSafe 하지 않다.
- JDK5 이상부터는 String + 연산을 할 때 컴파일 단계에서 StringBuilder로 변환하여 컴파일 된다. 그러나 반복문에서 사용하는 연산은 변환되지 않는다.

## StringBuffer
- StringBuffer도 StringBuilder와 동일한 역할을 하는 클래스이다. 차이점으론 StringBuffer는 TreadSafe 하다.
- 여러 스레드에서 동시에 접근하는 인스턴스를 연산할 경우 StringBuffer를 사용하는 것이 조금 더 바람직하다. 