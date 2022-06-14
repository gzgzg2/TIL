# Reflection
> Reflection이란 객체를 통해 클래스의 정보를 분석해내는 Java 프로그래밍 언어의 기법이다. 
> 
> 영어 단어 Reflection은 `반사`등의 의미를 뜻하는데 Java의 리플렉션은 `객체`로 `클래스`를 분석하기 때문에 이 의미를 그대로 수용하였다고 볼 수 있을 것 같다.

💡 리플랙션은 JVM에 로딩되어 있는 클래스와 메서드의 정보를 읽어온다.

## Reflection 관련 클래스들

### 📌 Class 클래스
Class 클래스는 클래스에 대한 정보를 얻을 때 사용하고 생성자는 따로 존재하지 않는다. Class 클래스의 주요 메서드는 아래와 같다.

| return       | name                                                  | description                                                                      | 
|--------------|-------------------------------------------------------|----------------------------------------------------------------------------------| 
| String       | getName()                                             | 클래스의 이름을 리턴한다                                                                    |
| Field[]      | getFields()                                           | public으로 선언된 변수 목록을 Field 클래스 배열타입으로 리턴한다.                                       |
| Field        | getField(String name)                                 | name과 동일한 public으로 선언된 변수를 Field 클래스 타입으로 리턴한다.                                  |
| Field[]      | getDeclaredFields()                                   | 해당 클래스에서 정의된 변수 목록을 Field 클래스 배열 타입으로 리턴한다.                                      |
| Field        | getDeclaredField(String name)                         | name과 동일한 변수를 Field 클래스 타입으로 리턴한다.                                               |
| Method[]     | getMethods()                                          | public으로 선언된 모든 메서드 목록을 Method 클래스 배열 타입으로 리턴한다. 해당 클래스에서 사용 가능한 상속받은 메서드도 포함된다. |
| Method       | getMethod(String name, Class... parameterTypes)       | public으로 선언된 지정된 이름과 매개변수 타입을 갖는 메서드를 Method 클래스 타입으로 리턴한다.                      |
| Method[]     | getDeclareMethods()                                   | 해당 클래스에 선언된 모든 메서드를 Method 클래스 배열타입으로 리턴한다.                                      |
| Method       | getDeclareMethod(String name, Class... parameterTypes) | 지정된 이름과 매개변수 타입을 갖는 해당 클래스에서 선언된 메서드를 Method 클래스 타입으로 리턴한다.                      |
| Constructor[] | getConstrunctors()                                    | 해당 클래스에 선언된 모든 public 생성자의 정보를 Constructor 배열 타입으로 리턴한다.                         |
| Constructor<T> | getConstrunctor(Class... parameterTypes)              | 지정된 매개변수 타입을 갖는 해당 클래스에서 public으로 선언된 생성자를 리턴한다.                                 |
| Constructor[] | getDelaredConstuctors()      | 해당 클래스에서 선언된 모든 생성자의 정보를 Constructor 배열 타입으로 리턴한다.                               |
| Constructor<T>  | getDelaredConstuctor(Class... parameterTypes)       | 지정된 매개변수 타입을 갖는 해당 클래스에서 선언된 생성자를 리턴한다.                                          |

**`getConstrunctor(Class... parameterTypes)` 와 `getDelaredConstuctor(Class... parameterTypes)` 메서드는 책에서 소개한게 아닌 따로 조사 후 추가한 메서드이므로 설명이 옳지 않을 수 있습니다.**


## 📌 Method 클래스
Method 클래스는 메서드에 대한 정보를 얻을 수 있게 해주는 클래스이다. 하지만 Method 클래스에도 생성자가 존재하지않으므로  
Method 클래스의 정보를 얻기 위해서는 Class 클래스의 `getMethods()`를 사용하거나 `getDeclareMethods()`를 사용해야 한다.  
Method 클래스의 주요 메서드는 아래와 같다.

| return    | name                               | description                      | 
|-----------|------------------------------------|----------------------------------|
| Class<?>  | getDeclaringClass()                | 해당 메서드가 선언된 클래스 정보를 리턴한다.        |
| Class<?>  | getReturnType()                    | 해당 메서드의 리턴 타입을 리턴한다.             |
| Class<?>[] | getParameterTypes()                | 해당 메서드를 사용하기 위한 매개변수의 타입들을 리턴한다. |
| String    | getName()                          | 해당 메서드의 이름을 리턴한다.                |
| int       | getModifiers()                     | 해당 메서드의 접근자 정보를 리턴한다.            |
| Class<?>  | getExceptionTypes()                | 해당 메서드에 정의되어 있는 예외 타입들을 리턴한다.    |
| Object    | invoke(Object obj, Object....args) | 해당 메서드를 수행한다.                    |
| String    | toGenericString()                  | 타입 매개변수를 포함한 해당 메서드의 정보를 리턴한다.   |
| String    | toString()                         | 해당 메서드의 정보를 리턴한다.                |

## 📌 Field
Field 클래스는 클래스에 있는 변수들의 정보를 제공하기 위해서 사용한다.  
Method 클래스와 마찬가지로 생성자가 존재하지 않으므로 Class 클래스의 `getField()` 메서드나 `getDeclaredFields()` 메서드를 사용해야 한다.  
Field 클래스의 주요 메서드는 아래와 같다.

| return | name          | description          | 
|--------|---------------|----------------------|
| int    | getModifiers() | 해당 변수의 접근자 정보를 리턴한다. | 
| String | getName()     | 해당 변수의 이름을 리턴한다.     |
| String | toString()    | 해당 변수의 정보를 리턴한다.     |

## 마무리 
책에서 소개한 내용은 여기까지이다.
reflection 클래스는 앞서 소개한 클래스 말고도 존재한다 `Constructor`, `Array` 등등  
사용법은 앞서 소개한 클래스와 비슷하다. reflection은 Spring, ObjectMapper등 익숙한 라이브러리나 프레임워크에서 사용 중이다. 

**reflection 사용예시**
```java
public class ContainerService {
    public static <T> T getObject(Class<T> classType) {
        T instance = createInstance(classType);
        Arrays.stream(classType.getDeclaredFields()).forEach(f -> {
            if (f.getAnnotation(Inject.class) != null) {
                Object filedInstance = createInstance(f.getType()); // 생성한 객체가 필요로하는 멤버객체 인스턴스 생성
                f.setAccessible(true);
                //필드의 클래스 인스턴스와 해당 필드타입과 동일한 인스턴스 넘겨주기
                try {
                    f.set(instance, filedInstance); // 생성한 멤버객체 인스턴스를 멤버객체에 주입
                } catch (IllegalAccessException e) {
                    throw new RuntimeException(e);
                }
            }
        });
        return instance;
    }

    private static <T> T createInstance(Class<T> classType) {
        try {
            return classType.getConstructor(null).newInstance(); // 객체 인스턴스 생성
        } catch (InstantiationException | IllegalAccessException | InvocationTargetException |
                 NoSuchMethodException e) {
            throw new RuntimeException(e);
        }
    }
}
```

