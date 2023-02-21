# Serializable에 대해서 살펴보자

## Serializable
- 마커 인터페이스
- 자바에서 외부로 객체를 출력할 때 import 해야하는 인터페이스
- Serializable 인터페이스를 import 하지않고 객체를 출력하게 되면 `NotSerializableException`이 발생한다.

### serialVersionUID
- 객체의 버전을 관리하기 위한 클래스 변수
- `static final long`으로 선언해야 한다.
- `serialVersionUID` 를 재정의하지 않으면 클래스의 내용이 변경될 때 값도 같이 변경된다.
- 직렬화한 객체의 `serialVersionUID` 버전이 현재 버전과 다른 경우 역직렬화 할 수 없다.
- `serialVersionUID` 를 재정의할 경우 기존 클래스가 변경되어도 역직렬화가 가능하다.
    - 이때 새로 추가된 멤버변수의 값은 null로 역직렬화 된다.

### transient 예약어
- 클래스를 직렬화 할 때 제외하고 싶은 멤버변수에 선언할 경우, 해당 필드는 제외되고 직렬화 된다.


## NIO
> I/O의 성능을 개선하기 위해 나타난 클래스  
> NIO는 스트림을 사용하지 않고 Channel과 Buffer를 사용한다

### NIO의 Buffer 클래스 메소드

**상태를 리턴하는 메소드**

| 리턴타입| 메소드        | 설명                     |
| --- |------------|------------------------|
| int | capacity() | 버퍼에 담을 수 있는 크기 리턴      |
| int | limit()    | 버퍼에서 읽거나 쓸 수 있는 첫 위치 리턴 |
| int | position() | 현재 버퍼의 위치 리턴           |

**위치를 변경하는 메소드**

| 리턴타입    | 메소드            | 설명                                                 |
|---------|----------------|----------------------------------------------------|
| Buffer  | flip()         | limit 값을 현재 position으로 지정한 후, position을 0으로 이동     |
| Buffer  | mark()         | 현재 position을 mark                                  |
| Buffer  | reset()        | 버퍼의 position을 mark한 곳으로 이동                         |
| Buffer  | rewind()       | 현재 버퍼의 position을 0으로 이동                            |
| int     | remaining()    | limit-position 계산 결과를 리턴                           |
| boolean | hasRemaining() | position과 limit 값에 차이가 있을 경우 true를 리턴              |
| Buffer  | clear()        | 버퍼를 지우고 현재 position을 0으로 이동하며, limit 값을 버퍼의 크기로 변경 |


