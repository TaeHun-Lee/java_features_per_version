# java_features_per_version


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
```java
public static void main(String[] args) {
	Runnable task = () -> {
		String threadName = Thread.currentThread().getName();
		System.out.println("Hello " + threadName);
	};
	
	task.run();
	Thread thread = new Thread(task);
	thread.start();
	System.out.println("Done!");
}
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

**2. A New HTTP Client**  
Java SE 8까지 사용하던 HttpURLConnection을 대체할 새로운 java.net.http 패키지가 추가되었습니다. 

```java
HttpRequest request = HttpRequest.newBuilder()
  .uri(new URI("https://postman-echo.com/get"))
  .GET()
  .build();

HttpResponse<String> response = HttpClient.newHttpClient()
  .send(request, HttpResponse.BodyHandler.asString());
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

``` java
FooClass<Integer> fc = new FooClass<>(1) { 
    // anonymous inner class
};
FooClass<? extends Integer> fc0 = new FooClass<>(1) {
    // anonymous inner class
};
FooClass<?> fc1 = new FooClass<>(1) { 
    // anonymous inner class
};
// 타입 파라메터없이 List만 사용해도 추론이 가능함
public List getPerson(String id) {
    return new List(findPersonById(id)){};
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
Stream<Optional> person = getPerson(id);
// Optional.stream은 Stream<Optional>을 Stream<Person>으로 바꾸어줌
Stream personStream = person.flatMap(Optional::stream);
// 아래와 같이 Optional로 Stream을 생성할 수 있음. 
Stream<Integer> stream = Optional.of(1).stream();
```

**9. Reactive Stream API 개선 (Flow API)**
리액티브 프로그래밍은 리액티브 스트림을 사용하는 프로그래밍이다. 리액티브 스트림은 잠재적으로 무한의 비동기 데이터를 순서대로 그리고 블록 하지 않는 역압력을 전재해 처리하는 표준 기술이다. 대표적으로 Publisher - Subscriber 형태로 구현 가능하다.

- Publisher
- Subscriber
- Subscription
- Processor

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

### Java SE 11
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

**2. Text Block**
줄 바꿈 문자가 자동으로 포함됩니다.

```java
String str = """
   This
   is
   text block
""";
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
String text = *"""
                Lorem ipsum dolor sit amet, consectetur adipiscing \
                elit, sed do eiusmod tempor incididunt ut labore \
                et dolore magna aliqua.\
                """*;
```

**2. Sealed Classes - Preview**
상속 가능한 클래스를 지정할 수 있는 봉인 클래스가 제공됩니다. 상속 가능한 대상은 상위 클래스 또는 인터페이스 패키지 내에 속해 있어야 합니다.

```java
public abstract sealed class Shape
    permits Circle, Rectangle, Square {...}
```

즉, 클래스가 public인 동안 하위 클래스로 허용되는 유일한 Shape 클래스들은 Circle, Rectangle 및 Square입니다.

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

### Java SE 17
---
#### **기능 변화**

**1. RandomGenerator(의사난수 생성기)**
의사난수 생성기를 통해 예측하기 어려운 난수를 생성하는 API가 정식 출시되었습니다.

```java
RandomGeneratorFactory.all()
.sorted(Comparator.comparing(RandomGeneratorFactory::name))
.forEach(factory -> System.out.println(String.format("%s\t%s\t%s\t%s",
										factory.group(),
										factory.name(),
										factory.isJumpable(),
										factory.isSplittable())));
```

**2. Sealed Class의 정식 도입**
Java 15에서 Preview로 도입되었던 sealed class가 production으로 도입되었습니다.

### Java SE 18
---

**1. 자바 API의 기본 Charset이 [UTF-8](https://namu.wiki/w/UTF-8 "UTF-8")으로 지정되었다.**

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
