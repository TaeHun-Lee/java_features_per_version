![Pasted image 20230917214119](https://github.com/TaeHun-Lee/java_features_per_version/assets/46686577/e58e2ede-e797-4e24-a06a-a522aed76343)

# Java 1

### Java 1.1
---
**1. 이너 클래스(Inner Class), JavaBeans, RMI, 리플렉션(Reflection), Calendar [유니코드](https://namu.wiki/w/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C) 지원 등이 추가되었습니다.**

> **JavaBeans 란?**  
> JavaBeans는 자바로 작성된 소프트웨어 컴포넌트를 지칭합니다.  
>   
> **Beans 규약**  
>   1. 기본 생성자가 반드시 존재해야 한다.  
>   2. 모든 속성은 비공개이다.  
>   3. 속성에 접근하고 꺼내올 수 있는 getter, setter 메서드를 구성한다.  
>   4. Serializable을 구현한다.

> **RMI 란?**
> Remote Method Invocation의 약자로 분산 애플리케이션을 구축하는 데 사용됩니다.  
>한 시스템(JVM)에 상주하는 객체가 다른 JVM에서 실행 중인 객체에 액세스, 호출할 수 있도록 도와주는 메커니즘입니다. 코드에서는 java.rmi 패키지를 통하여 제공됩니다.

![Pasted image 20230920020917](https://github.com/TaeHun-Lee/java_features_per_version/assets/46686577/87909814-0b8b-437b-80b3-f306baf87d70)

```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface Hello extends Remote {
	void printMsg() throws RemoteException;
}
```

```java
public class ImplExample implements Hello {
	public void printMsg() {
		System.out.println("This is an example RMI program");
	}
}
```

```java
public class Server extends ImplExample { 
	public Server() {}
	public static void main(String args[]) {
		try {
			ImplExample obj = new ImplExample();

			Hello stub = (Hello) UnicastRemoteObject.exportObject(obj, ©);

			Registry registry = LocateRegistry.getRegistry();
			registry.bind("Hello", stub);
			System.err.println("Server ready");
		} catch (Exception e) {
			System.err.println("Server exception: + e.toString()); e.printStackTrace();
		}
	}
}
```

```java
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;
public class Client {
	private Client() {}
	public static void main(String[] args) {
		try {
			Registry registry = LocateRegistry.getRegistry(null);

			Hello stub = (Hello) registry.lookup("Hello");

			stub.printMsg();
		} catch (Exception e) {
			System.err.println("Client exception: " + e.toString());
			e.printStackTrace();
		}
	}
}
```

> **Java Reflection**
> reflection 은 자바의 특징이다. 실행중인 자바프로그램 내부를 검사하고 내부의 속성을 수정할 수 있도록 한다. 예를 들어, 어떤 자바클래스가 가진 모든 멤버의 이름을 얻거나 보여줄 수 있다.  
> 자바에서 클래스가 그 자신을 조사하고 수정하는 것이  많다고 할수는 없으나 다른 언어에서는 볼수 없는 특징이다. reflection 이 구체적인 쓰임중에 하나가 빌더툴을 이용해서 소프트웨어 콤포넌트를 만드는 곳에서 이다. 툴은 reflection 을 사용해서 동적으로 로딩되는 자바 콤포넌트(클래스)의 속성을 얻을 수 있다.

```java
Constructor<?> constructor = aClass.getDeclaredConstructor();
// 생성자 가져오기

Object object = constructor.newInstance();
// 이렇게 인스턴스를 생성할 수 있다.

Member member = (Member) constructor.newInstance();
// 타입 캐스팅을 사용해서 위와 같이 받아올 수 있다.
```

---
### J2SE 1.2
---
**1. Swing GUI, JIT, Collection Framework 등의 굵직한 기능이 추가되었습니다.**

> Collection Framework에는 동기화된 버전의 컬렉션도 제공됩니다. 예를 들어, `ConcurrentHashMap`, `ConcurrentLinkedQueue`와 같은 컬렉션은 스레드 안전성을 제공하며 별도의 동기화 작업 없이 안전하게 사용할 수 있습니다.

**2. 부터 약칭을 J2SE(Java 2 Standard Edition)로 표기하기 시작했으며, 이 표기는 5까지 사용됩니다.**

### J2SE 1.3
---
**1. HotSpot JVM, JNDI, [JPDA](https://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html), JavaSound 등이 추가되었습니다.**

> **HotSpot JVM**
> CPU코어가 하나뿐인 사용자를 위해 만들어진 것이 HotSpot 클라이언트 컴파일러이다

> **JDNI**
> JNDI(Java Naming and Directory Interface)는 디렉터리 서비스에서 제공하는 데이터 및 객체를 발견(discover)하고 참고(lookup) 하기 위한 자바 API다.
> ex ) web.xml , server.xml

> **JPDA**
> 로컬 톰캣에서 디버깅 할 때와 원격(Remote) 서버에서 디버깅 활 때의 환경 차이를 제어하기 위해 동일한 디버깅 환경을 조성하기 위해 사용한다.
### J2SE 1.4
---
**1. assert, [정규표현식](https://namu.wiki/w/%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D), [IPv6](https://namu.wiki/w/IPv6), XML API, JCE, JSSE, JAAS, Java Web Start 등이 추가되었습니다.**

> **assert**
> Java의 `assert` 문을 사용하여 가정을 검증하고 코드를 안정화하는 것은 Java의 철학 중 하나인 "실패 원칙"과 관련이 있습니다. 이 원칙은 프로그램이 실패하더라도 가능한 한 빨리 실패를 발견하고 오류를 조기에 처리해야 한다는 것을 강조합니다. `assert` 문은 이러한 철학을 구현하는 데 도움을 주며, 개발자에게 코드의 신뢰성과 안정성을 높이는 도구를 제공합니다.
> 
> 1. **가정 검증 (Assertion Verification):** `assert` 문은 코드 내의 가정을 검증하기 위한 도구로 사용됩니다. 개발자는 코드에서 기대하는 상태나 조건을 명시적으로 검사할 수 있으며, 이러한 가정이 만족되지 않으면 AssertionError가 발생하여 프로그램이 중단됩니다. 
> 2. **디버깅 및 오류 검출:** `assert` 문은 디버깅과 오류 검출에 도움이 됩니다. 코드 내에서 예상치 못한 문제가 발생할 때 `assert` 문을 사용하여 해당 문제를 빠르게 식별하고 원인을 찾을 수 있습니다.  
> 3. **문서화와 가독성:** `assert` 문을 사용하면 코드의 가정을 문서화할 수 있습니다. 이러한 문서화는 다른 개발자가 코드를 이해하고 유지 관리할 때 도움이 됩니다. 또한 코드가 나중에 변경될 때 이러한 가정이 여전히 유효한지 확인할 수 있습니다. 
> 4. **선행조건과 후행조건 검사:** `assert` 문을 사용하여 메서드나 함수에 대한 선행조건 (precondition)과 후행조건 (postcondition)을 검사할 수 있습니다. 선행조건은 메서드 호출 전에 검사되며, 후행조건은 메서드 실행 후에 검사됩니다. 이는 코드의 일관성과 안정성을 높이는 데 도움을 줍니다.
> 5. **테스트 코드 작성:** `assert` 문은 단위 테스트 및 통합 테스트를 작성하는 데 필수적입니다. 테스트 케이스 내에서 예상 결과와 실제 결과를 비교하여 테스트를 수행하고 결과를 확인하는 데 사용됩니다.
> 6. **유효성 검사:** `assert` 문을 사용하여 입력 데이터나 중간 계산 결과의 유효성을 검사할 수 있습니다. 이는 예외를 던지기 전에 무효한 입력을 확인하는 데 도움이 됩니다.
> 또한 기본적으로 Java에서는 `assert` 문이 비활성화되어 있으며, 활성화하려면 명령줄 옵션 또는 실행 환경 설정을 변경해야 합니다. 또한 `assert` 문은 프로그램의 런타임 동작에 영향을 미치지 않는다는 가정하에 사용되므로, 런타임 성능에 부정적인 영향을 미치지 않도록 주의해야 합니다.

> 정리하자면 Assert는 실제 런타임 환경에서 에러가 발생하지 않도록 테스트 단계에서 가정과 검증을 거치기 위해 사용하는 것이며 런타임 환경에서 사용은 지양하는 것이 좋을 것 같습니다.

```java
int value = -1;
assert value >= 0 : "음수 값입니다.";
System.out.println("양수 값입니다.");

// 위 경우 AssertionError Exception을 throw
```

### J2SE 5(2004년 9월)
---

#### 기능 변화

**1. Generics**
5 버전의 가장 중요한 신규 기능입니다. 기존에 컬렉션프레임워크를 이용하여 발생할 수 있는 ClassCastException을 컴파일 시간에 검증할 수 있습니다. 이러한 컴파일 검증 기능 뿐만 아니라 코드에 대한 데이터를 명확하게 하여 가독성을 높일 수 있습니다. 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미합니다. 객체 별로 다른 타입의 자료가 저장될 수 있도록 합니다.
```java
ArrayList<String> list = new ArrayList<>();
```

**3. Annotation**
비즈니스 로직에는 영향을 주지 않으면서 해당 타겟의 연결 방법이나 소스 코드의 구조를 변경할 수 있습니다. 소스 코드 안에 메타 데이터를 삽입하는 방식으로 사용 가능 합니다. 개발자는 시스템 구현에 필요한 코드는 Annotation에 위임하고 비즈니스 로직에 집중할 수 있습니다, 따라서 AOP 관점에서 편리하게 프로그램을 구성 가능합니다.

```java
// 어노테이션 생성
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
@Retention(RetentionPolicy.RUNTIME) // 런타임중에도 유효한 어노테이션임을 기술
public @interface Count100 {
// 어노테이션은 @interface 인터페이스명으로 정의
}
```

**4. Concurrency API**
API를 사용하여 병렬 프로그래밍 혹은 멀티 스레드를 손쉽게 구현할 수 있습니다. 

1. **스레드 생성 및 실행:**
```java
// Runnable 인터페이스 구현을 통한 스레드 생성
Runnable task = () -> { 
	String threadName = Thread.currentThread().getName();
	System.out.println("Hello " + threadName);
};
task.run();
Thread thread = new Thread(task);
thread.start();
System.out.println("Done!");
```

2. **Executor 및 스레드 풀 사용:**
```java
// Executor를 사용한 스레드 풀 생성 및 작업 실행
Executor executor = Executors.newFixedThreadPool(3);
Runnable task = () -> {
    System.out.println("Task executed by " + Thread.currentThread().getName());
};
executor.execute(task);
```

3. **동기화 및 Lock 사용:**
```java
// Lock을 사용한 동기화
ReentrantLock lock = new ReentrantLock();
lock.lock();
try {
    // 임계 영역 (Critical Section)
    // 공유 자원에 대한 접근
} finally {
    lock.unlock();
}
```

4. **Wait 및 Notify 사용:**
```java
// wait()과 notify()를 사용한 스레드 간 통신
Object monitor = new Object();

// Producer 스레드
synchronized (monitor) {
    // 데이터를 생산하고
    monitor.notify(); // Consumer 스레드를 깨움
}

// Consumer 스레드
synchronized (monitor) {
    monitor.wait(); // Producer로부터 데이터를 기다림
    // 데이터를 소비
}
```

5. **Fork/Join 프레임워크 사용:**
```java
// ForkJoinPool을 사용한 병렬 처리
ForkJoinPool forkJoinPool = ForkJoinPool.commonPool();
long result = forkJoinPool.invoke(new MyRecursiveTask());
```

6. **CountDownLatch 사용:**
```java
// CountDownLatch를 사용한 작업 완료 대기
CountDownLatch latch = new CountDownLatch(3);

// 스레드에서 작업 수행 후
latch.countDown(); // 작업 완료를 알림

// 다른 스레드에서
latch.await(); // 작업이 완료될 때까지 대기
```

7. **Semaphore 사용:**
```java
// Semaphore를 사용한 리소스 제한
Semaphore semaphore = new Semaphore(3); // 최대 3개의 스레드까지 허용
semaphore.acquire(); // 리소스 획득
// 스레드에서 리소스 사용
semaphore.release(); // 리소스 반환
```

8. **CompletableFuture 사용:**
```java
// CompletableFuture를 사용한 비동기 작업과 조합
CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> 42);
CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> 10);
CompletableFuture<Integer> resultFuture = future1.thenCombine(future2, (x, y) -> x + y);
Integer result = resultFuture.join();
```

**5. Enumeration**
자바 개발자들은 데이터 구조를 더 쉽게 정의하고 사용할 수 있는 열거형(Enum) 기능을 원했고 자바 5에 추가되었습니다. 이를 이용하여 클래스, 인터페이스 열거형으로, 소스 코드를 작성할 수 있습니다.
> **Enumeration**
> Legacy Collection(Vector, Hashtable)
> Iterator의 구 버전

> **Iterator**
> Collection Framework

> **ListIterator**
> arrayList, linkedList 등과 같은 List 컬렉션
> Iterator의 확장

**6. Auto Boxing/Unboxing**
오토박싱/언박싱 기능을 통해 개발자가 기본형 데이터를 래퍼 클래스로 직접 변환하지 않아도 언어 차원에서 자동 변환이 가능하도록 보강되었습니다.

> int -> Integer 등으로 자동 변환

### Java SE 6
---
#### **기능 변화**

1. 이때부터 표기가 J2SE에서 Java SE로 바뀌었습니다.
2. 가비지 컬렉터 G1(Garbage First) GC을 오직 테스트용으로만 사용하도록 추가하였습니다.
> **Garbage Collection**  
> Heap 영역 내에서 unreachable object를 찾아 회수함으로써 메모리 관리 역할을 수행합니다.
> (보충 하기)

3. Scripting API [DOC](https://docs.oracle.com/javase/8/docs/technotes/guides/scripting/prog_guide/api.html)
- 자바 코드 내에서 스크립팅 언어 (예: JavaScript)를 실행할 수 있는 기능을 도입했습니다.
```java
import javax.script.*;

public class EvalScript {
    public static void main(String[] args) throws Exception {
        ScriptEngineManager manager = new ScriptEngineManager();
        ScriptEngine engine = manager.getEngineByName("nashorn");

        // evaluate JavaScript code
        engine.eval("print('Hello, World')");
    }
}
```

### Java SE 7(2011년 7월)
---
#### **기능 변화**

**1. Diamond Operator <>**
General Class 초기화 시 Type Interface를 지원하게 되었습니다.

```java
//Java SE 7 버전 이전
List<Integer> list = new ArrayList<Integer>();

//Java SE 7 버전 이후
List<Integer> list = new ArrayList<>();
```

**2. switch문에서 String 사용**
Tyr-Catch 내에 선언된 Collection 등의 자원을 자동으로 close 처리합니다.

**3. Try-with-resources**
try-catch 블록 사용 시 finally 블록 안에서 명시적으로 close를 호출하지 않아도 자동으로 사용된 자원을 해제해주는 기능

> **Auto close 해주는 기준**
> Try-with-resources 문장을 사용하려면 close 대상인 Resource 클래스가 AutoCloseable 인터페이스를 구현해야 합니다. AutoCloseable 인터페이스에는 close() 메서드가 정의되어 있으며 이 메서드를 사용하여 자원을 명시적으로 해제할 수 있어야 합니다.

```java
public static void main(String args[]) {
    try (
        FileInputStream is = new FileInputStream("file.txt");
        BufferedInputStream bis = new BufferedInputStream(is)
    ) {
        int data = -1;
        while ((data = bis.read()) != -1) {
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// finally에서 close()를 호출해주지 않아도 자동으로 해제해줌
```

### Java SE 8 (LTS)
---
#### **기능 변화**

**1. Lambda Expression**  
메소드를 지칭하는 명칭 없이 구현부를 선언하는 익명 메소드 생성 문법입니다. 별도의 익명 클래스를 만들어서 선언하던 방식을 람다를 통해 대폭 간소화할 수 있으며, 함수형 프로그래밍, 스트림 API 그리고 컬럭션 프레임워크의 개선 등에 영향을 주었습니다. 

> 익명 메소드 (Anonymous Function)과 Lambda Expression의 차이
> - Lambda 표현식은 컴파일러가 파라미터 타입을 추론할 수 있으므로 타입을 명시적으로 선언할 필요가 없습니다.
> - 익명 함수에서는 파라미터 타입을 명시적으로 선언해야 합니다.
> - 두 표현식 모두 서로 상호 변경이 가능합니다.

```java
// Lambda Expression
Function<Integer, Integer> square = x -> x * x;
```
```java
// Anonymous Function
Function<Integer, Integer> square = new Function<Integer, Integer>() {
    @Override
    public Integer apply(Integer x) {
        return x * x;
    }
};
```

> 도입 배경 ?
> **함수형 프로그래밍 지원:** 함수형 프로그래밍은 코드를 좀 더 간결하고 읽기 쉽게 만들어주며, 병렬 처리와 같은 다중 코어 CPU를 활용한 효율적인 프로그래밍을 지원합니다. Lambda 표현식은 함수형 프로그래밍을 자바에 도입하기 위한 핵심 요소 중 하나입니다. Java 8의 Lambda 표현식과 함수형 프로그래밍 개념은 Java의 진화를 이끈 중요한 요소 중 하나이며, 더 현대적이고 효율적인 프로그래밍을 지원합니다. 이러한 변화는 Java 언어와 생태계를 더욱 강력하고 다양한 도구와 라이브러리를 활용할 수 있는 방향으로 나아가게 해주었습니다.

```java
// 기존의 방식 반환티입 메소드명 (매개변수, ...) { 실행문 } 
// 예시 
public String hello() { return "Hello World!"; }

// 람다 방식 (매개변수, ... ) -> { 실행문 ... } 
// 예시 
() -> "Hello World!";
```

**2. Method Reference**
메서드 레퍼런스는 특정 메서드만을 호출하는 람다의 축약형입니다. 메서드 레퍼런스를 새로운 기능이 아니라 하나의 메서드를 참조하는 람다를 편리하게 표현할 수 있는 문법으로 간주할 수 있습니다.

**람다와 메서드 레퍼런스 단축 표현 예시**

|   |   |
|---|---|
|**람다**|**메서드 레퍼런스**|
|(Soccer s) -> s.getGoal()|Soccer::getGoal|
|() -> Thread.currentThread().dumpStack()|Thread.currentThread()::dumpStack|
|(str, i) -> str.substring(i)|String::substring|
|(String s) -> System.out.println(s)|System.out::println|

```java
List<Integer> intList = new ArrayList<Integer>();
intList.add(0);
intList.add(1);
intList.add(2);

/**
이하 메소드 참조 람다식
**/
// 클래스의 static 메소드
List<List<Integer>> resultList = intList.stream().map(Arrays::asList).collect(Collectors.toList());
// 인스턴스의 메소드
List<Integer> resultList = intList.stream().map(intList::get).collect(Collectors.toList());
// 클래스의 생성자
List<Double> resultList = intList.stream().map(Double::new).collect(Collectors.toList());

System.out.println(intList);
System.out.println(resultList);
```

**3. Interface의 Default Methods**
Java SE 8에서는 인터페이스에 디폴트 메소드(Default Methods)라는 것이 추가되었습니다. 인터페이스는 메소드 정의만 할 수 있고 구현은 할 수 없었지만, Java SE 8에서부터는 디폴트 메소드라는 개념이 생겨 구현 내용도 인터페이스에 포함시킬 수 있습니다.

> **왜 쓰는가?**
> ...(중략) ... 바로 "하위 호환성"때문이다. 예를 들어 설명하자면, 여러분들이 만약 오픈 소스코드를 만들었다고 가정하자. 그 오픈소스가 엄청 유명해져서 전 세계 사람들이 다 사용하고 있는데, 인터페이스에 새로운 메소드를 만들어야 하는 상황이 발생했다. 자칫 잘못하면 내가 만든 오픈소스를 사용한 사람들은 전부 오류가 발생하고 수정을 해야 하는 일이 발생할 수도 있다. 이럴 때 사용하는 것이 바로 default 메소드다. (자바의 신 2권)

```java
// 일반적인 인터페이스 구현
public interface Soccer {
    public void doShooting(int n);
}

// 디폴트 메소드를 사용하여 구현 내용을 인터페이스에 포함
public interface Soccer {
    public default void doShooting(int n) {
        System.out.println("doShooting(Shooting)");
    }
}


// 디폴트 메소드가 구현된 인터페이스 또한 상속 받을수 있음
public interface Soccer {
    public default void doShooting(int n) {
        System.out.println("doShooting(Shooting)");
    }
}

public interface SoccerChild extends Soccer {
}
```

> **궁금점**
> default method도 override가 가능한가?
> -> 가능하다.

**4. Null 처리 Optional 추가**
이전까지 변수의 null 여부를 검사하고 대응하기 위해 많은 노력을 했습니다. Java SE 8부터는 Optional이라는 구조체를 제공해서 이전보다 간편하게 NPE(Null Pointer Exception) 이슈에 대응할 수 있습니다.

```java
// Optional 구조체의 생성
Optional<String> optionalStr = Optional.empty();
Optional<String> optionalStr = Optional.of("str");
Optional<String> optionalStr = Optional.ofNullable("str");

// null 일 경우
System.out.println(optionalStr.orElse("empty str"));

// java8 Optional 이전까지의 null 대응
String value = null;
String result = "";
try {
	result = value.toUpperCase();
} catch (NullPointerException exception) {
	throw new Exception();
}

// java8 Optional 이후의 null 대응
String value = null;
Optional<String> valueOpt = Optional.ofNullable(value);
String result = valueOpt.orElseThrow(Exception::new).toUpperCase();
```

**5. 새롭게 추가된 날짜와 시간 API**
기존 Date와 Calendar 클래스의 기능 부족과 비 표준적인 명명 규칙, 그리고 일관되지 못한 속성 값의 문제를 해결하기 위해 새로운 날짜 API가 추가되었습니다.

```java
//javax.time.Clock
Clock.systemUTC();					//current time of your system in UTC. 
Clock.millis();						//time in milliseconds from 1/1/1970.

//javax.tme.ZoneId
ZoneId zone = ZoneId.of(“Europe/London”);		//zoneId from a timezone. 
Clock clock = Clock.system(zone);			//set the zone of a Clock.

//javax.time.LocalDate
LocalDate date = LocalDate.now();			//current date 
String day = date.getDayOfMonth();			//day of the month 
String month = date.getMonthValue();			//month 
String year = date.getYear();				//year
```

**6. Stream API**
Stream이란 순차/병렬 작업을 지원하는 어떠한 순차적인 요소입니다. 쉽게 스트림을 데이터 컬렉션 반복을 처리하는 기능입니다. 람다 표현식, 함수형 인터페이스 그리고 메서드 참조를 이용한 최종 산출물입니다. 기존 컬렉션 프레임워크를 이용할 때보다 간결하게 코드 작성이 가능합니다. 병렬처리, 스트림 파이프라인 등을 통해 하나의 문장으로 다양한 처리 기능을 구현 가능합니다.

```java
void filterTest1() {
    Human human = humans.stream()
        .filter(h -> h.getName().equals("taehun"))
        .findFirst()
        .orElseThrow(() -> new IllegalArgumentException());

    System.out.println(human.getIdx());
}
```

**7. PermGen Area 제거**
![](https://blog.kakaocdn.net/dn/dGAgLe/btrvW45LvuE/38yOEZWjfbza7mMKuXQ6e1/img.png)

Java8 이전에는 초기 설정 시 PermSize와 MaxPermSize를 설정해주어야 했으나, Java8부터는 Permanent Generation이 Metaspace로 대체되었습니다. Metaspace는 런타임 시 메모리 요구 사항에 따라 자체 크기를 조정하며, 필요하다면 MaxMetaspaceSize 매개변수를 설정하여 Metaspace의 양을 조절할 수 있습니다.

> **Permanent Generation**  
> - Permanent Generation은 Class 혹은 Method Code가 저장되는 영역  
> - PermGen은 Heap 영역에 속함  
> - Default로 제한된 크기를 가지고 있음  
>   
> **Metaspace**  
> - Metaspace는 Java 클래스 로더가 현재까지 로드한 Class들의 메타데이터가 저장되는 공간  
> - JVM에 의해 관리되는 Heap 영역이 아니라 OS 레벨에서 관리되는 Native 메모리 영역에 위치  
> - Default로 제한된 크기를 가지고 있지 않고, 필요한 만큼 늘어남

### Java SE 9
---

#### **기능 변화**

**1. 모듈시스템 [jigsaw](http://openjdk.java.net/projects/jigsaw/quick-start) 등장**
이전까지 자바에서는 name space를 사용한 패키지와 gradle, maven을 사용하여 패키지와 라이브러리를 관리하였습니다. 하지만 기존의 방법들에는 서로 다른 패키지간의 캡슐화가 지원되지 않았습니다.

jigsaw는 모듈을 만들고 모듈에 명시적으로 외부에서 호출할 수 있는 API를 선언합니다. 언어 레벨에서 API 작성자가 외부에 노출한 API 외에는 접근할 수 없습니다. 뿐만 아니라 모듈은 그 모듈에서 사용할 다른 모듈을 선언합니다. JDK는 이 기능을 통해 모든 내부 API를 캡슐화하여 구성합니다.

**Jigsaw Project의 목적**

- SE 플랫폼과 JDK를 작은 컴퓨터 디바이스에 보다 쉽게 경량화(scalable down) 할 수 있게 합니다.
- Java SE 플랫폼의 전반적인 구현 부분과 특히 JDK 부분의 보안성과 유지관리성(maintainability)을 향상합니다.
- 애플리케이션의 성능을 높입니다.
- Java SE 그리고 EE 플랫폼에서 개발자들이 라이브러리와 래플리케이션 구성, 유지를 쉽게 만들어 줍니다.
- Jigsaw의 주요 목표는 한마디로 유연한 런타임 이미지를 만들 수 있도록 Java 플랫폼을 모듈화 하는 것입니다.

```java
// Java 코드 최상위에 module-info.java 파일을 만들고, 아래와 같이 사용함
module java.sql {
    requires public java.logging;
    requires public java.xml;
    exports java.sql;
    exports javax.sql;
    exports javax.transaction.xa;
}
```

> 디렉토리 구조 확인해봅시다

**2. A New HTTP Client**  
Java SE 8까지 사용하던 HttpURLConnection을 대체할 새로운 java.net.http 패키지가 추가되었습니다. 

> Java 9 이전

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpURLConnectionExample {
    public static void main(String[] args) throws Exception {
        String url = "https://example.com";

        HttpURLConnection connection = (HttpURLConnection) new URL(url).openConnection();
        connection.setRequestMethod("GET");

        int responseCode = connection.getResponseCode();
        System.out.println("Response Code: " + responseCode);

        BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        String line;
        StringBuilder response = new StringBuilder();

        while ((line = reader.readLine()) != null) {
            response.append(line);
        }
        reader.close();

        System.out.println("Response Body: " + response.toString());
    }
}
```

> Java 9 이후

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class HttpRequestExample {
    public static void main(String[] args) throws Exception {
        String url = "https://example.com";
        HttpClient httpClient = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(new URI(url))
                .GET()
                .build();

        CompletableFuture<HttpResponse<String>> responseFuture = httpClient.sendAsync(request, HttpResponse.BodyHandlers.ofString());

        responseFuture.thenAccept(response -> {
            int statusCode = response.statusCode();
            System.out.println("Response Code: " + statusCode);

            String responseBody = response.body();
            System.out.println("Response Body: " + responseBody);
        });

        // 비동기 작업이 완료될 때까지 대기
        responseFuture.join();
    }
}
```

**3. Jshell - The Java Shell**
Java SE 9은 테스트 프로젝트나 main 메소드 없이 코드를 신속하게 테스트할 수 있는 대화식 REPL(Read-Eval-Print-Loop) 도구를 제공합니다. 

**4. Process API 개선**
OS 프로세스 관리 및 컨트롤을 위해 아래의 두 개 패키지가 추가되었습니다. 아래와 같이 사용할 수 있습니다. (서버 쉘에서 PID를 직접 조회하거나 제어하지 않고 소스 코드로 관리 가능)

```java
java.lang.ProcessHandle
java.lang.ProcessHandle.Info

//다음과 같이 사용 가능
ProcessHandle processHandle = ProcessHandle.current();
ProcessHandle.Info = processHandle.info();
```

**5. try-with-resources 개선**
기존 try-with-resources의 경우 try-catch 블록 밖에서 선언한 변수는 바로 안에서 사용할 수 없었지만 이제 바로 사용이 가능하며 세미콜론을 통해 복수 개의 변수도 사용 가능합니다.

```java
void tryWithResourcesByJava7() throws IOException {
    BufferedReader reader1 = new BufferedReader(new FileReader("test.txt"));
    try (BufferedReader reader2 = reader1) {
        // do something
    }
}


// final or effectively final이 적용되어 reader 참조를 사용할 수 있음
void tryWithResourcesByJava9() throws IOException {
    BufferedReader reader = new BufferedReader(new FileReader("test.txt"));
	try (reader) {
        // do something
    }
}
```

**6. 다이아몬드 연산자(Diamond Operator)**
Java SE 7에는 코드를 보다 읽기 쉽게 만드는데 도움 되는 다이아몬드 연산자(Diamond Operator)라는 새로운 기능이 있었지만, 익명 클래스(Anonymous Inner Class)에는 컴파일 오류가 발생하여 제한적이었습니다.

Java SE 9 이후부터는 익명 클래스에서도 사용할 수 있도록 개선되었습니다.

```java
import java.util.ArrayList;
import java.util.List;

public class DiamondOperatorExample {
    public static void main(String[] args) {
	    // 익명 내부 클래스에서 Diamond Operator 사용 (경고 발생하지 않음)
        List<String> list = new ArrayList<>(){
            {
                add("Java");
                add("C++");
                add("Python");
            }
        };
        
        for (String lang : list) {
            System.out.println(lang);
        }
    }
}
```

**7. Interface Private Method**
인터페이스 내에서 private 메서드 사용이 가능해졌습니다.

```java
interface NewInterface {
    
    private static String staticPrivate() {
        return "static private";
    }
    private String instancePrivate() {
        return "instance private"; 
    } 
    default void check() {
        String result = staticPrivate();
        NewInterface newIf = new NewInterface() {
            // anonymous class
        }; 
        result = newIf.instancePrivate(); 
    }
}}
```

**8. Optional To Stream**

```java
import java.util.Optional;
import java.util.stream.Stream;

interface Person {
    String getName();
}

// 예시로 사용할 Optional<Person> 객체 생성
Optional<Person> optionalPerson = Optional.of(new Person() {
	@Override
	public String getName() {
		return "John";
	}
});

// Java 9부터 추가된 stream 메서드를 사용하여 Optional을 Stream으로 변환
Stream<Person> personStream = optionalPerson.stream();

// Stream의 요소 출력
personStream.forEach(person -> System.out.println("Person Name: " + person.getName()));

// 아래와 같이 Optional로 Stream을 생성할 수 있음. 
Stream<Integer> stream = Optional.of(1).stream();
```

**9. Reactive Stream API 개선 (Flow API)**
리액티브 프로그래밍은 리액티브 스트림을 사용하는 프로그래밍이다. 리액티브 스트림은 잠재적으로 무한의 비동기 데이터를 순서대로 그리고 블록 하지 않는 역압력을 전재해 처리하는 표준 기술이다. 대표적으로 Publisher - Subscriber 형태로 구현 가능하다.

- Publisher
- Subscriber
- Subscription
- Processor

```java
import java.util.concurrent.Flow;
import java.util.concurrent.SubmissionPublisher;

public class FlowExample {

    public static void main(String[] args) throws InterruptedException {
        // SubmissionPublisher는 Publisher와 Subscriber를 연결해주는 클래스입니다.
        SubmissionPublisher<String> publisher = new SubmissionPublisher<>();

        // Subscriber 구현체 생성
        Flow.Subscriber<String> subscriber = new Flow.Subscriber<>() {
            private Flow.Subscription subscription;

            @Override
            public void onSubscribe(Flow.Subscription subscription) {
                this.subscription = subscription;
                subscription.request(1); // 구독 시작
            }

            @Override
            public void onNext(String item) {
                System.out.println("Received: " + item);
                subscription.request(1); // 다음 아이템을 요청
            }

            @Override
            public void onError(Throwable throwable) {
                throwable.printStackTrace();
            }

            @Override
            public void onComplete() {
                System.out.println("Done");
            }
        };

        // Publisher와 Subscriber 연결
        publisher.subscribe(subscriber);

        // 데이터 발행
        publisher.submit("Hello");
        publisher.submit("World");
        publisher.submit("Flow");

        // 스레드가 종료되기 전에 잠시 대기
        Thread.sleep(1000);

        // 종료
        publisher.close();
    }
}

/**
이 예제에서는 SubmissionPublisher를 사용하여 문자열 데이터를 발행하고, Subscriber 인터페이스를 구현하여 데이터를 구독합니다. `onSubscribe`, `onNext`, `onError`, `onComplete` 메서드를 사용하여 데이터 처리와 에러 핸들링을 수행합니다.
**/
```

![Pasted image 20230920032459](https://github.com/TaeHun-Lee/java_features_per_version/assets/46686577/ec05a180-5ee3-4fdd-90d9-0ddb039fd587)

### Java SE 10
---
#### **기능 변화**

**1. Local-Variable Type Interface**
로컬 변수 타입 추론 기능입니다. 로컬 변수 타입을 'var'로 선언할 수 있습니다.

다음과 같은 케이스에만 적용이 됩니다.

- initializer와 함께 선언되는 로컬 변수

- enhanced for-loop 내 index 변수

- traditional for-loop 내 로컬 변수

```java
var list - new ArrayList<String>();	//ArrayList<String> 으로 추론
var stream = list.stream();		//Stream<String> 으로 추론

var numbers = List.of(1, 2, 3, 4, 5);	//List<Integer> 으로 추론

for (var number : numbers){		//Integer 추론
	System.out.println(number);
}
```

**2. [Garbage Collector Interface](http://openjdk.java.net/jeps/304)**
다양한 GC의 코드 고립도를 향상하는 인터페이스를 도입하였습니다.

> 1. **`java.lang.management.GarbageCollectorMXBean` 인터페이스 확장:**
> - `GarbageCollectorMXBean`은 이전에도 GC 관련 정보를 얻을 수 있는 인터페이스였습니다. Java 10에서는 이 인터페이스가 확장되어 사용자 지정 GC 관련 동작을 추가하고 구현할 수 있도록 되었습니다.

> 2. **새로운 `java.lang.management.GarbageCollector` 인터페이스:**
> - `GarbageCollector` 인터페이스는 Java 10부터 도입된 인터페이스로, GC 구현체를 나타내는데 사용됩니다.
> - 사용자는 이 인터페이스를 구현하여 자체 GC 알고리즘을 개발하고 사용할 수 있습니다.

사용자 정의 GC 알고리즘을 만들려면 다음 단계를 따를 수 있습니다:

> 1. `GarbageCollector` 인터페이스를 구현합니다.
> 2. GC 알고리즘에 필요한 로직을 구현합니다.
> 3. 해당 GC 알고리즘을 Java 애플리케이션에 적용합니다.

**3. Thread-Local Handshakes**
VM safepoint를 수행할 필요 없이 개별 스레드를 stop 시키고 콜백을 수행하도록 할 수 있는 기능을 도입하였습니다.

> **VM safepoint란?**  
> "Stop The World"로 표현되며, 모든 스레드를 일시 정지시키는 작업입니다.  
>   
> **safepoint를 발생시키는 경우**  
> - Garbage collection pauses  
> - Code deoptimization  
> - Flusing code cache  
> - Class redefinition  
> - Biased lock revocation  
> - Various debug operation

**4. Root Certificates**
HTTPS 통신에 쓰이는 SSL/TLS 인증서를 발급해주는 인증 기관인 CA 중에서 root CA 목록을 브라우저와 마찬가지로 Oracle JDK에서도 가지고 있습니다. OpenJDK도 이를 Java SE 10부터 지원합니다.

### Java SE 11 (LTS)
---
#### **기능 변화**

**1. HTTP 클라이언트(JEP 321)**
Java SE 9에 인큐베이팅 형태로 넣었던 HTTP 클라이언트 API를 정식적으로 포함했습니다. 이 API는 기존에 제공되는 URLConnection 기반의 HTTP 개발보다 개선된 기능과 명명규칙을 제공합니다. 특히 HTTP 2.0을 지원하여 웹 소켓 기능도 포함되어있습니다.

**2. 새로운 String 메서드 추가**

|   |   |
|---|---|
|strip()|문자열 앞, 뒤의 공백 제거|
|stripLeading()|문자열 앞의 공백 제거|
|stripTrailing()|문자열 뒤의 공백 제거|
|isBlank()|문자열이 비어있거나 공백만 포함되어있을 경우 true를 반환. 즉, String.trim().isEmpty() 호출과 같음|
|lines()|문자열을 라인 단위로 쪼개는 스트림을 반환|
|repeat(n)|지정된 수 만큼 문자열을 반복하여 붙여 반환|

trim()은 U+0020 이하의 값만을 공백으로 인식하여 제거하였습니다.(tab, CR, LF, 공백) 하지만 유니코드에서는 이 외에 다양한 공백 문자가 존재하는데, 이를 처리하기 위해서는 기존엔 Chararcter.isWhitespace(int)를 사용해야만 했습니다.

Java SE 11부터는 strip()으로 편하게 처리할 수 있습니다.

```java
//repeat(n) 예시
String str = "ABC";
String repeated = str.repeat(3);	// "ABCABCABC"
```

**3. Lambda 파라미터로 var 사용**
Java SE 10에서 var 키워드로 변수를 선언하면 타입 추론으로 객체를 생성할 수 있는 기능이 추가되었습니다. Java SE 11에서는 여기에 람다 표현식에서도 var를 사용해서 변수를 선언할 수 있게 되었습니다.

```java
(var x, var y) -> x.process(y) => (x, y) -> x.process(y)
```

**4. Logging API 개선**
JVM 로그 디테일 개선 사항이 있었습니다.
> Package java.util.logging
> Class Logger
### Java SE 12
---

#### **기능 변화**

**1.  문법적으로 Switch문을 확장**
```java
//기존 방식
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}


//Java SE 12 부터의 방식
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}
```

**2. 가비지 컬렉터 개선, 마이크로 벤치마크 툴 추가, 성능 개선의 변경점이 있습니다.**

> 마이크로 벤치마크 툴
 - Java 12부터는 JDK(Microbenchmarking Mode)에 기본적으로 포함된 새로운 마이크로벤치마크 도구가 도입되었습니다. 이 도구는 `java` 명령과 함께 `-XX:+UseMicroBenchmark` 옵션을 사용하여 활성화할 수 있습니다. 이 도구를 사용하면 Java 프로그램의 성능을 미세하게 측정하고 분석할 수 있습니다.

주요 특징과 사용법은 다음과 같습니다:

1. **미세한 벤치마크 측정:** 이 도구는 초당 연산 횟수(OPS)와 같은 성능 측정 값을 표시하여 매우 미세한 벤치마크를 수행할 수 있습니다.

2. **`@Benchmark` 애너테이션:** 벤치마크 메서드를 정의할 때 `@Benchmark` 애너테이션을 사용합니다. 이 애너테이션을 붙인 메서드는 벤치마크로 실행됩니다.

3. **Warmup과 Measurement:** 벤치마크 실행 전에 "웜업(Warmup)" 단계가 있으며, 이 단계에서 JIT(Just-In-Time) 컴파일러를 활성화하고 최적화합니다. 그런 다음 "측정(Measurement)" 단계에서 성능 측정이 이루어집니다.

4. **결과 출력:** 벤치마크 결과는 표준 출력에 출력되며, 초당 연산 횟수(OPS) 및 표준 편차와 같은 성능 지표가 제공됩니다.


다음은 Java 12의 마이크로벤치마크 도구를 사용하는 간단한 예제입니다.
```java
import org.openjdk.jmh.annotations.*;

@State(Scope.Thread)
public class MicrobenchmarkExample {

    @Benchmark
    @BenchmarkMode(Mode.Throughput)
    @Warmup(iterations = 3, time = 1)
    @Measurement(iterations = 5, time = 1)
    public void benchmarkMethod() {
        // 여기에 벤치마크하려는 코드를 작성합니다.
    }

    public static void main(String[] args) throws Exception {
        org.openjdk.jmh.Main.main(args);
    }
}
```

> 결과에는 OPS(초당 연산 횟수)와 표준 편차 등이 포함됩니다.

### Java SE 13
---
#### **기능 변화**

**1. switch문 개선을 위한  yield라는 예약어 추가**
기존 Switch 표현에서 `break value;`와 같은 문법으로 해당 기능을 지원했으나 이제 `yield`로 사용 가능합니다. `yield`는 쉽게 말게 함수의 return과 비슷하다고 할 수 있습니다. yield는 해당 Switch 블록에 특정 값을 Switch의 결과 값으로 반환하는 것입니다.

```java
var a = switch (day) {
    case MONDAY, FRIDAY, SUNDAY:
        yield 6;
    case TUESDAY:
        yield 7;
    case THURSDAY, SATURDAY:
        yield 8;
    case WEDNESDAY:
        yield 9;
};
```

### Java SE 14
---
#### **기능 변화**

**1. 스위치 표현(Standard)**
Java SE 12 및 13에서 preview였던 스위치 표현식이 이제 표준화 되었습니다. (블록 사용 가능, 표현식이 있는 구문 사용 가능)

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    default      -> {
      String s = day.toString();
      int result = s.length();
      yield result;
    }
};
```

**2. record(preview)**
Java로 많은 상용구를 작성하는 고통을 완화하는데 도움이 되는 레코드 클래스가 있습니다.

```java
//데이터, getter/setter, equals/hashcode, toString만 포함되는 이 Java SE 14 이전 클래스
final class Point {
    public final int x;
    public final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

레코드 사용
```java
record Point(int x, int y) { }
```

**3. 유용한 NullPointerExceptions**
NullPointerExceptions은 정확히 어떤 변수가 null인지 설명합니다.

```java
author.age = 35;
---
Exception in thread *"main"* java.lang.NullPointerException:
     Cannot assign field *"age"* because *"author"* is null
```

**4. Pattern Matching**
- 어떤 객체의 형태가 간결하게 패턴으로 표현되도록 하고, 입력에 대해 그 형태가 매칭되는지 확인하는 것
    - 문자열 패턴매칭: 정규표현식 패턴매칭이나 globbing 패턴 매칭
    - 객체 타입 패턴매칭

```java
// 기존 instanceof 연산
if (animal instanceof Cat) {
	Cat cat = (Cat) animal;
    cat.meow();
    // other cat operations
} else if (animal instanceof Dog) {
	Dog dog = (Dog) animal;
    dog.woof();
    // other dog operations
}
```

```java
// java 14 instanceof
if (animal instanceof Cat cat) {
	cat.meow();
} else if (animal instanceof Dog dog) {
	dog.woof();
}
```

### Java SE 15
---
#### **기능 변화**

**1. Text-Blocks / Multiline Strings**
Java SE 13의 실험 기능으로 도입된 여러 줄 문자열의 Production Ready 버전입니다.

```java
String text = """
                Lorem ipsum dolor sit amet, consectetur adipiscing 
                elit, sed do eiusmod tempor incididunt ut labore 
                et dolore magna aliqua.
                """;
```

### Java SE 16
---
#### **기능 변화**
**1. OpenJDK의 버전 관리 : Git**
OpenJDK 의 버전 관리가 Mercurial이었으나,  [Git](https://namu.wiki/w/Git)으로 바뀌었습니다. 이제 OpenJDK 소스를 GitHub에서 볼 수 있습니다.

**2. Unix-Domain Socket Channels**
이제 Unix 도메인 소켓에 연결할 수 있습니다.(macOS 및 Windows(10+)에서도 지원)

```java
 socket.connect(UnixDomainSocketAddress.of(
        *"/var/run/postgresql/.s.PGSQL.5432"*));
```

### Java SE 17 (LTS)
---
#### **기능 변화**

**1. RandomGenerator(의사난수 생성기)**
의사난수 생성기를 통해 예측하기 어려운 난수를 생성하는 API가 정식 출시되었습니다.

> 기존에 난수 생성 어떻게 했더라?

```java
RandomGeneratorFactory.all()
.sorted(Comparator.comparing(RandomGeneratorFactory::name))
.forEach(factory -> System.out.println(String.format("%s\t%s\t%s\t%s",
										factory.group(),
										factory.name(),
										factory.isJumpable(),
										factory.isSplittable())));
```

**2. Sealed Classes**
상속 가능한 클래스를 지정할 수 있는 봉인 클래스가 제공됩니다. 상속 가능한 대상은 상위 클래스 또는 인터페이스 패키지 내에 속해 있어야 합니다.

```java
public abstract sealed class Shape
    permits Circle, Rectangle, Square {...}
```

즉, 클래스가 public인 동안 하위 클래스로 허용되는 유일한 Shape 클래스들은 Circle, Rectangle 및 Square입니다.

> **sealed class의 도입 배경**
> 1. **상속 제어:** 기존에 Java 클래스는 어떤 클래스든지 다른 클래스에서 상속될 수 있었습니다. 이로 인해 클래스의 디자인이 의도치 않게 변경될 수 있고, 예기치 않은 버그와 보안 취약점이 발생할 수 있었습니다. Sealed 클래스는 어떤 클래스가 이를 상속할 수 있는지 명시적으로 제어함으로써 이러한 문제를 방지합니다.
> 1. **API 디자인의 안정성:** Sealed 클래스를 사용하면 API 디자인의 안정성을 높일 수 있습니다. 외부에서 상속된 클래스가 무제한적으로 추가되지 않도록 하여 API의 일관성을 유지할 수 있습니다. 
> 3. **패턴 매칭과 결합:** Sealed 클래스는 패턴 매칭과 결합하여 더 효과적인 코드를 작성할 수 있도록 합니다. 패턴 매칭은 패턴별로 처리할 수 있도록 하며, Sealed 클래스는 이러한 패턴 매칭에서 유용하게 사용될 수 있습니다.

### Java SE 18
---

**1. 자바 API의 기본 Charset이 [UTF-8](https://namu.wiki/w/UTF-8 "UTF-8")으로 지정되었다.**

> 기존의 Java에서 기본 Charset은 사용되는 플랫폼에 따라 달라졌습니다. 예를 들어 Windows 운영 체제에서는 기본적으로 "Windows-1252" 문자 집합이 사용되었고, Unix/Linux 운영 체제에서는 "UTF-8" 문자 집합이 주로 사용되었습니다.

**2. 정적 파일을 서빙하는 기능만 있는 심플한 웹 서버 제공 (커맨드라인 툴)**

**3. Java API Doc에 @snippet 태그 추가**
- 해당 조각에 대한 API 액세스를 제공하여 소스 코드 조각의 유효성 검사를 촉진합니다. 정확성은 궁극적으로 작성자의 책임이지만, `javadoc`관련 도구의 향상된 지원을 통해 정확성을 더 쉽게 얻을 수 있습니다.
- 구문 강조와 이름을 선언에 자동으로 연결하는 등 현대적인 스타일을 지원합니다.
- 스니펫 생성 및 편집을 위한 더 나은 IDE 지원을 활성화합니다.
```java
/**
 * The following code shows how to use {@code Optional.isPresent}:
 * {@snippet :
 * if (v.isPresent()) {
 *     System.out.println("v: " + v.get());
 * }
 * }
 */
```

**5. 벡터 API (세 번째 인큐베이터 단계)**
런타임 시 지원되는 CPU 아키텍처에서 최적의 벡터 명령으로 안정적으로 컴파일하는 벡터 계산을 표현하는 API를 도입하여 동등한 스칼라 계산보다 뛰어난 성능을 달성합니다.

```java
void scalarComputation(float[] a, float[] b, float[] c) {
   for (int i = 0; i < a.length; i++) {
        c[i] = (a[i] * a[i] + b[i] * b[i]) * -1.0f;
   }
}
```
```java
static final VectorSpecies<Float> SPECIES = FloatVector.SPECIES_PREFERRED;
void vectorComputation(float[] a, float[] b, float[] c) {
    int i = 0;
    int upperBound = SPECIES.loopBound(a.length);
    for (; i < upperBound; i += SPECIES.length()) {
        // FloatVector va, vb, vc;
        var va = FloatVector.fromArray(SPECIES, a, i);
        var vb = FloatVector.fromArray(SPECIES, b, i);
        var vc = va.mul(va)
                   .add(vb.mul(vb))
                   .neg();
        vc.intoArray(c, i);
    }
    for (; i < a.length; i++) {
        c[i] = (a[i] * a[i] + b[i] * b[i]) * -1.0f;
    }
}
```

**6. Internet-Address Resolution SPI**
API `InetAddress`는 조회 작업을 위한 여러 메서드를 정의합니다.
- [`InetAddress::getAllByName`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/net/InetAddress.html#getAllByName(java.lang.String))_정방향_ 조회를 수행하여 호스트 이름을 IP 주소 집합에 매핑합니다.
- [`InetAddress::getByName`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/net/InetAddress.html#getByName(java.lang.String))또한 정방향 조회를 수행하여 호스트 이름을 주소 집합의 첫 번째 주소에 매핑합니다.
- [`InetAddress::getCanonicalHostName`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/net/InetAddress.html#getCanonicalHostName())_역방향_ 조회를 수행하여 IP 주소를 정규화된 도메인 이름에 매핑합니다.
    ```java
    var addressBytes = new byte[] { (byte) 192, 0, 43, 7};
    var resolvedHostName = InetAddress.getByAddress(addressBytes)
                                      .getCanonicalHostName();
    ```
- [`InetAddress::getHostName`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/net/InetAddress.html#getHostName())필요한 경우 역방향 조회도 수행합니다.

**7. 외부 함수 & 메모리 API (두 번째 인큐베이터 단계)**

```java
// 1. Find foreign function on the C library path
CLinker linker = CLinker.getInstance();
MethodHandle radixSort = linker.downcallHandle(
                             linker.lookup("radixsort"), ...);
// 2. Allocate on-heap memory to store four strings
String[] javaStrings   = { "mouse", "cat", "dog", "car" };
// 3. Allocate off-heap memory to store four pointers
MemorySegment offHeap  = MemorySegment.allocateNative(
                             MemoryLayout.ofSequence(javaStrings.length,
                                                     ValueLayout.ADDRESS), ...);
// 4. Copy the strings from on-heap to off-heap
for (int i = 0; i < javaStrings.length; i++) {
    // Allocate a string off-heap, then store a pointer to it
    MemorySegment cString = implicitAllocator().allocateUtf8String(javaStrings[i]);
    offHeap.setAtIndex(ValueLayout.ADDRESS, i, cString);
}
// 5. Sort the off-heap data by calling the foreign function
radixSort.invoke(offHeap, javaStrings.length, MemoryAddress.NULL, '\0');
// 6. Copy the (reordered) strings from off-heap to on-heap
for (int i = 0; i < javaStrings.length; i++) {
    MemoryAddress cStringPtr = offHeap.getAtIndex(ValueLayout.ADDRESS, i);
    javaStrings[i] = cStringPtr.getUtf8String(0);
}
assert Arrays.equals(javaStrings, new String[] {"car", "cat", "dog", "mouse"});  // true
```

**9. try문의 finally기능 deprecate.**
아직 제거되진 않았지만 더 이상 사용을 권장하지 않습니다.

### Java SE 21 (LTS)
---
- [JEP 431](https://openjdk.org/jeps/431 "https://openjdk.org/jeps/431"): 순차 컬렉션(Sequenced Collections)

**1. 순차 컬렉션 (Sequenced Collections)**

> **도입 배경**

| |첫 번째 요소|마지막 요소|
|---|---|---|
|`List`|`list.get(0)`|`list.get(list.size() - 1)`|
|`Deque`|`deque.getFirst()`|`deque.getLast()`|
|`SortedSet`|`sortedSet.first()`|`sortedSet.last()`|
|`LinkedHashSet`|`linkedHashSet.iterator().next()`|`// missing`|

> 기존에 정의되어 있던 Collection Framework 및 구현체의 경우 정해진 순서대로 데이터를 얻고자 할 때 명확한 기준점이 없었던 문제가 있었습니다. 따라서 어떤 경우 첫 번째 요소를 얻고자 할 때와 마지막 요소를 얻고자 할 때 구현해야 하는 방식이 달랐습니다.

> 순차화된 컬렉션에는 첫 번째 요소와 마지막 요소가 있으며, 그 사이의 요소에는 후속 요소와 선행 요소가 있습니다. 순차 컬렉션은 양쪽 끝에서(First와 Last) 동일한 기능을 지원하며 처음부터 마지막으로, 마지막에서 처음으로(즉, 정방향 및 역방향) 데이터를 동일하게 다룰 수 있도록 지원합니다.

```java
interface SequencedCollection<E> extends Collection<E> {
    // new method
    SequencedCollection<E> reversed();
    // methods promoted from Deque
    void addFirst(E);
    void addLast(E);
    E getFirst();
    E getLast();
    E removeFirst();
    E removeLast();
}
```

동일하게 SequencedSet, SequencedMap 또한 지원합니다.

![Pasted Image](https://cr.openjdk.org/~smarks/collections/SequencedCollectionDiagram20220216.png)

세부적으로는 기존 클래스와 인터페이스를 개선하기 위해 다음과 같은 수정 사항이 있었습니다.

- `List`는 이제 상위 인터페이스로 `SequencedCollection`가 있습니다.
- `Deque`는 이제 상위 인터페이스로 `SequencedCollection`가 있습니다.
- `LinkedHashSet`은 추가로 `SequencedSet`을 구현합니다.
- `SortedSet`는 이제 상위 인터페이스로 `SequencedSet`가 있습니다.
- `LinkedHashMap`은 추가로 `SequencedMap`을 구현합니다.
- `SortedMap`은 이제 상위 인터페이스로 `SequencedMap`가 있습니다.

또한 Immutable 래퍼를 생성할 수 있는 추가적인 API를 제공합니다.

- `Collections.unmodifiableSequencedCollection(sequencedCollection)`
- `Collections.unmodifiableSequencedSet(sequencedSet)`
- `Collections.unmodifiableSequencedMap(sequencedMap)`

**2. [Generational ZGC](https://openjdk.org/jeps/439 "https://openjdk.org/jeps/439")**
> 메모리 상의 "젊은" 객체와 "오래된" 객체에 대해 별도의 세대를 유지함으로써 애플리케이션 성능을 향상시킵니다. 이를 통해 ZGC는 어려서 죽기 쉬운 어린 객체를 더 자주 수집할 수 있습니다.

**3. [레코드 패턴](https://openjdk.org/jeps/440 "https://openjdk.org/jeps/440")**
> Java SE 14에 도입되었던 레코드 클래스와 패턴 매칭을 결합하여 사용이 가능합니다.
> 예를 들어서 기존의 경우 패턴 매칭을 직관적으로 사용하지 못하고 분해하여 사용하여야 했습니다.
```java
// As of Java 16
record Point(int x, int y) {}

static void printSum(Object obj) {
    if (obj instanceof Point p) {
        int x = p.x();
        int y = p.y();
        System.out.println(x+y);
    }
}
```

> 이제 패턴 매칭에 레코드 클래스를 바로 사용 가능합니다.
```java
// As of Java 21
static void printSum(Object obj) {
    if (obj instanceof Point(int x, int y)) {
        System.out.println(x+y);
    }
}
```

**4. [`switch` 문의 패턴 매칭 정식 출시](https://openjdk.org/jeps/441 "https://openjdk.org/jeps/441")**
> switch 문에서의 패턴 매칭 사용 기능이 프로덕션되었습니다. 이제 보다 직관적으로 swtich 문 내부에서 객체의 타입을 검증할 수 있습니다.
```java
// Prior to Java 21
static String formatter(Object obj) {
    String formatted = "unknown";
    if (obj instanceof Integer i) {
        formatted = String.format("int %d", i);
    } else if (obj instanceof Long l) {
        formatted = String.format("long %d", l);
    } else if (obj instanceof Double d) {
        formatted = String.format("double %f", d);
    } else if (obj instanceof String s) {
        formatted = String.format("String %s", s);
    }
    return formatted;
}
```
```java
// As of Java 21
static String formatterPatternSwitch(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
    };
}
```

**5. [가상 스레드(Virtual Threads)](https://openjdk.org/jeps/444 "https://openjdk.org/jeps/444")** 
> Project Loom으로 알려진 기술입니다. 기존 스레드보다 비용이 저렴하고 더 많이 생성 가능하며, `Spring` 에서 이 기술을 적용하여 기존 코드로도 동시성 처리를 만들어준다고 공언한 원천 기술이 정식 출시되었습니다.
```java
void handle(Request request, Response response) {
    var url1 = ...
    var url2 = ...
 
    try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
        var future1 = executor.submit(() -> fetchURL(url1));
        var future2 = executor.submit(() -> fetchURL(url2));
        response.send(future1.get() + future2.get());
    } catch (ExecutionException | InterruptedException e) {
        response.fail(e);
    }
}
 
String fetchURL(URL url) throws IOException {
    try (var in = url.openStream()) {
        return new String(in.readAllBytes(), StandardCharsets.UTF_8);
    }
}
```

**6. [Windows 32-bit x86 제거 예정](https://openjdk.org/jeps/449 "https://openjdk.org/jeps/449")**
> Windows 32비트 x86 포트가 deprecated 되었습니다. 사용 불가능한 것은 아니나 향후 유지보수를 위해 사용을 추천하지 않습니다.

**7. [자바 Agent의 동적 불러오기를 허용하지 않도록 세팅](https://openjdk.org/jeps/451 "https://openjdk.org/jeps/451")**
> Agent가 실행 중인 JVM에 동적으로 로드되면 경고를 발생 시킵니다. 향후 릴리스에는 [기본 상태 무결성 개선을](https://openjdk.org/jeps/8305968) 위해 기본적으로 Agent의 동적 로드를 허용하지 않으며 이러한 경고를 통해 사용자들을 준비시키는 것을 목표로 하고 있습니다. 시작 시 Agent를 로드하는 Serviceability tools는 어떤 릴리스에서도 경고를 발행하지 않습니다.

**6. [키 캡슐화 메커니즘 API](https://openjdk.org/jeps/452 "https://openjdk.org/jeps/452")**
> 공개 키 암호화를 사용하여 대칭 키를 보호하기 위한 암호화 기술인 키 캡슐화 메커니즘(KEM)용 API가 도입되었습니다.

> KEM은 세 가지 기능으로 구성됩니다.
>- 공개키와 개인키가 포함된 키 쌍을 반환하는 _키 쌍 생성 함수입니다_ .
>- _송신자가 호출하는 키 캡슐화 함수는_ 수신자 의 공개 키와 암호화 옵션을 사용합니다. 비밀 키 _K_ 와 _키 캡슐화 메시지_ ( ISO 18033-2에서는 _암호문 이라고 함)를 반환합니다._ 송신자는 수신자에게 키 캡슐화 메시지를 보냅니다.
>- 수신자의 개인 키와 수신된 키 캡슐화 메시지를 사용하는 수신자가 호출하는 _키 캡슐화 해제 함수_ . 비밀 키 _K 를_ 반환합니다.

> 키 쌍 생성 기능은 기존 [`KeyPairGenerator` API](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/security/KeyPairGenerator.html) 에서 제공하고 있습니다. `KEM`캡슐화 및 캡슐화 해제 기능을 위해 새로운 Class를 도입하였습니다.

```java
package javax.crypto;

public class DecapsulateException extends GeneralSecurityException;

public final class KEM {

    public static KEM getInstance(String alg)
        throws NoSuchAlgorithmException;
    public static KEM getInstance(String alg, Provider p)
        throws NoSuchAlgorithmException;
    public static KEM getInstance(String alg, String p)
        throws NoSuchAlgorithmException, NoSuchProviderException;

    public static final class Encapsulated {
        public Encapsulated(SecretKey key, byte[] encapsulation, byte[] params);
        public SecretKey key();
        public byte[] encapsulation();
        public byte[] params();
    }

    public static final class Encapsulator {
        String providerName();
        int secretSize();           // Size of the shared secret
        int encapsulationSize();    // Size of the key encapsulation message
        Encapsulated encapsulate();
        Encapsulated encapsulate(int from, int to, String algorithm);
    }

    public Encapsulator newEncapsulator(PublicKey pk)
            throws InvalidKeyException;
    public Encapsulator newEncapsulator(PublicKey pk, SecureRandom sr)
            throws InvalidKeyException;
    public Encapsulator newEncapsulator(PublicKey pk, AlgorithmParameterSpec spec,
                                        SecureRandom sr)
            throws InvalidAlgorithmParameterException, InvalidKeyException;

    public static final class Decapsulator {
        String providerName();
        int secretSize();           // Size of the shared secret
        int encapsulationSize();    // Size of the key encapsulation message
        SecretKey decapsulate(byte[] encapsulation) throws DecapsulateException;
        SecretKey decapsulate(byte[] encapsulation, int from, int to,
                              String algorithm)
                throws DecapsulateException;
    }

    public Decapsulator newDecapsulator(PrivateKey sk)
            throws InvalidKeyException;
    public Decapsulator newDecapsulator(PrivateKey sk, AlgorithmParameterSpec spec)
            throws InvalidAlgorithmParameterException, InvalidKeyException;

}
```

> Example
```java
// Receiver side
KeyPairGenerator g = KeyPairGenerator.getInstance("ABC");
KeyPair kp = g.generateKeyPair();
publishKey(kp.getPublic());

// Sender side
KEM kemS = KEM.getInstance("ABC-KEM");
PublicKey pkR = retrieveKey();
ABCKEMParameterSpec specS = new ABCKEMParameterSpec(...);
KEM.Encapsulator e = kemS.newEncapsulator(pkR, specS, null);
KEM.Encapsulated enc = e.encapsulate();
SecretKey secS = enc.key();
sendBytes(enc.encapsulation());
sendBytes(enc.params());

// Receiver side
byte[] em = receiveBytes();
byte[] params = receiveBytes();
KEM kemR = KEM.getInstance("ABC-KEM");
AlgorithmParameters algParams = AlgorithmParameters.getInstance("ABC-KEM");
algParams.init(params);
ABCKEMParameterSpec specR = algParams.getParameterSpec(ABCKEMParameterSpec.class);
KEM.Decapsulator d = kemR.newDecapsulator(kp.getPrivate(), specR);
SecretKey secR = d.decapsulate(em);

// secS and secR will be identical
```
