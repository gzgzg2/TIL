# Funtion Structure - 2

## Temporal Coupling

- 함수들이 순서를 지키며 호출되어야 한다.
    - ex) open, execute, dose
- Passing a Block

### CQS

- command
    - 시스템 상태 변경 가능
    - side effect를 갖는다.
    - 아무것도 반환하지 않는다.
    - 상태를 변경하는 함수는 exception을 발생시킬 수는 있지만, 값을 반환할 수는 없다.
- query
    - 계산값이나 시스템의 상태를 반환
- CQS 정의
    - 상태를 변경하는 함수는 값을 반환하면 안된다.
    - 값을 반환하는 함수는 상태를 변경하면 안된다.
- 만약 CQ가 합쳐진다면?
    - getCount() 함수가 내부에서 count를 증가시킨다면 매번 getCount를 호출할 때마다 Count값이 변경된다. Count를 증가시키지 않고는 값을 가져오지 못하는 경우이다.

### Tell Don’t Ask

```java
// 예시1
if(user.isLoggedIn())
	user.execute(command);
else
	authenticator.promptLogin();

// 예시2
try {
	user.execute(command);
}
catch(User.NotLooggend In e) {
	annuciator.promptLogin();
}

//예시3
user.execute(command, annuciator);
```

- 예시1, 예시2 는 user 객체에게 상태를 묻는 경우 `Ask`
    - 다른 객체의 상태를 가져다가 작업을 결정하지 마라. 상태를 가지고 있는 객체가 수행하게 하여라
- 예시3은 user가 execute 수행에 대해 대답하는 경우 `tell`
    - login 여부는 User 객체에 속한다. User의 상태를 가져다가 외부에서 작업을 결정할 필요가 없다.
    - login 여부의 상태를 가지고 있는 User 객체가 작업을 수행하고 결과를 전달하는 것이 맞다.
- Tell Don’t Ask 규칙을 지키고 코드를 작성하면 Query 메서드가 줄어든다.

### Train wrecks & Law of Demeter

1️⃣ **Train wrecks**

```java
//예시1
o.getX()
		.getY()
			.getZ()
				.doSomething();

//예시2
o.doSomething();
```

- 예시 1번은 `doSomething()` 을 수행하기 위해서 너무 많은 정보를 알아야한다.
    - 테스트하기도 복잡하다 doSomething을 테스트 하기 위해 Y, Z까지 mocking 해야 한다.
- 예시 1번은 Tell Don’t Ask를 명확하게 위반하는 경우이다.
    - 객체 o에게 Y, Z의 존재여부를 묻고 doSomething을 실행한다.
    - Y, Z의 존재여부를 묻지않고 객체 O가 doSomething을 수행하게 하는것이 위반하지 않는 방법이다.
- 예시 2번은 o가 doSomething을 어떻게 수행해야 할지는 모른다.
    - 그렇지만 o는 어떤 객체에게 tell 하면 되는지 안다.
    - 최종적으로는 처리되기까지 요청이 전달된다.
- java Beans Pattern을 활용한 객체는 `Tell Don’t Ask`를 어기기 쉽게 만든다.
    - 내부 상태값을 외부에서 관찰할 수 있기 때문에 객체에게 실행을 요구하지 않고 상태값을 외부에서 직접 핸들링하기 쉽다

**2️⃣ Law of Demeter**

- 함수가 시스템의 전체를 알게 하면 안된다.
- 개별 함수는 아주 제한된 지식만 가져야 한다.
- 객체는 요청된 기능 수행을 위해 이웃 객체에게 요청해야 한다.
- 요청을 수신하면 적절한 객체가 수신할 때까지 전파되어야 한다.

<aside>
💡 ask대신 tell하면 surrounding과 decouple 된다.

</aside>

### early return

- early return이나 guarded return은 허용됨
- break, 루프 중간에서의 return은 loop를 복잡하게 함
    - 억지로 코드를 돌아가게 하는 코드보다 이해할 수 있게 하는 것이 더 중요하다.
        - 한번 작성된 코드 라인은 계속 사용되는 코드라면 1000번 이상 읽게 된다.

## error handling

- 에러 처리를 위해 특정 에러 코드를 반환하는 것보다 예외를 발생시키는 것이 좋다.
- exception의 이름은 최대한 구체적이여야 한다.

### **checked vs unchecked**

1️⃣ **checked**

- 하위 클래스에서 exception이 유발되면 상위 클래스를 변경해야하고 사용되는 모든 코드를 변경해야 함
- checked exception을 아예 사용하지 말라

**2️⃣ unchecked**

- 굳이 단점을 만들어보자면 함수 호출자가 예외를 캐치하고 프로그램을 이어서 실행해야 할 때 어려움이 있을 수 있음 (명시적으로 나타나지 않기 때문에)



**💡 EJB나 Spring에선 RuntimeException이 발생해야만 rollback**
