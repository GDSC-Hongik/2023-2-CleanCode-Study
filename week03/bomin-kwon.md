3주차 : <클린코드> 8~10장
<br><br>

# 8장. 경계

### 외부 코드 사용하기

- Map : 객체 유형 제한x, 사용자라면 누구나 clear() 메서드로 map 내용 지울 수 있음.
- Map<String, String> : 여전히 사용자에게 필요하지 않은 기능까지 제공. Map 인터페이스가 변했을 때 수정할 코드 많아짐 (Java 버전 업그레이드)
- 경계 인터페이스인 Map을 클래스 안으로 숨겨야 한다.
- Map 클래스를 사용할 때마다 캡슐화하라는 건 아님. Map을 여기저기 넘기지 말라는 것.
- Map 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용하지 않는다.

```java
// Map<String, Sensor> sensors = new HashMap<Sensor>();

public class Sensors{
	private Map sensors = new HashMap();
	public Sensor getById(String id) {
		return (Sensor) sensors.get(id);
	}
}
```
<br><br>
### 경계 살피고 익히기

학습테스트를 작성해서 외부 코드를 익힌다.

프로그램에서 사용하는 방식대로 외부 API를 호출하고 API를 제대로 이해했는지 확인 가능.
<br><br>

학습테스트 예시 (log4j)

```java
public class LogTest {    
	private Logger logger;     
	
	@Before    
	public void initialize() {        
		logger = Logger.getLogger("logger");        
		logger.removeAllAppenders();        
		Logger.getRootLogger().removeAllAppenders();    
	}     
	
	@Test    
	public void basicLogger() {        
		BasicConfigurator.configure();        
		logger.info("basicLogger");    
	}     

	@Test    
	public void addAppenderWithStream() {        
		logger.addAppender(new ConsoleAppender(new PatternLayout("%p %t %m%n"), ConsoleAppender.SYSTEM_OUT));        
		logger.info("addAppenderWithStream");    
	}     

	@Test    
	public void addAppenderWithoutStream() {        
		logger.addAppender(new ConsoleAppender(new PatternLayout("%p %t %m%n")));        
		logger.info("addAppenderWithoutStream");    
	}
}

```

패키지 새 버전이 나오면 학습 테스트를 돌려 차이가 있는지 확인한다.

경계 테스트가 없다면 낡은 버전을 필요 이상으로 오랫동안 사용하려는 유혹에 빠지기 쉽다.
<br><br>

### 아직 존재하지 않는 코드를 사용하기

경계는 아는 코드와 모르는 코드를 분리하는 데에도 쓰인다.

다른 팀이 아직 API를 설계하지 않았을 때 그쪽 코드의 구체적인 동작방식은 모른다. 따라서 구현을 미루고 이쪽 코드를 진행하기 위해 자체적으로 인터페이스를 정의한다.

예시)

![Untitled](w3%20266a51d899ec4437bbef05a6a294dca0/Untitled.png)

저쪽 팀이 송신기 API를 정의한 후에는 TransmitterAdapter를 구현해 간극을 매운다. ADAPTER 패턴으로 API 사용을 캡슐화해 API가 바뀔 때 수정할 코드를 한 곳으로 모은다.

이와 같은 설계는 테스트도 쉽다. 적절한 FakeTransmitter 클래스를 사용하면 CommunicationsController 클래스를 테스트할 수 있다. Transmitter API 인터페이스가 나온 다음 경계 테스트 케이스를 생성해 우리가 API를 올바로 사용하는지 테스트할 수도 있다.

- 어댑터 패턴 : [https://niceman.tistory.com/141](https://niceman.tistory.com/141)

<br><br>

### 깨끗한 경계

경계에 위치한 코드는 분리하고, 기대치를 정의하는 테스트 케이스를 작성한다.

외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리하자. 

새로운 클래스로 경계를 감싸거나 ADAPTER 패턴을 사용해서 우리가 원하는 인터페이스를 패키지가 제공하는 인터페이스로 변환하자.
<br><br>
<br><br>


# 9장. 단위 테스트

TDD: Test Driven Development

### TDD 법칙

1.실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.

2.컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.

3.현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

실제 코드와 맞먹는 방대한 테스트 코드는 심각한 관리 문제를 유발하기도 한다.
<br><br>

### 깨끗한 테스트 코드 유지하기

실제 코드가 진화하면 테스트 코드도 변한다. 테스트 코드가 지저분하면 변경하기 어려워진다. 

테스트 코드는 실제 코드만큼 중요하다.
<br><br><br>

### 테스트 : 유연성, 유지보수성, 재사용성 제공

테스트 케이스가 있으면 변경이 두렵지 않다. 실제 코드를 점검하는 자동화된 단위 테스트는 설계와 아케텍쳐를 최대한 깨끗하게 보존하는 열쇠이다.

테스트 코드는 가독성이 좋아야 한다.

테스트 코드 예시)

```java
// 목록 9-2 SerializedPageResponderTest.java (리팩토링한 코드)
public void testGetPageHierarchyAsXml() throws Exception {
  makePages("PageOne", "PageOne.ChildOne", "PageTwo");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
}

public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
  WikiPage page = makePage("PageOne");
  makePages("PageOne.ChildOne", "PageTwo");

  addLinkTo(page, "PageTwo", "SymPage");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
  assertResponseDoesNotContain("SymPage");
}

public void testGetDataAsXml() throws Exception {
  makePageWithContent("TestPageOne", "test page");

  submitRequest("TestPageOne", "type:data");

  assertResponseIsXML();
  assertResponseContains("test page", "<Test");
}
```

- `BUILD-OPERATE-CHECK` 패턴이 위와 같은 테스트 구조에 적합하다.
- 각 테스트는 명확히 세 부분으로 나눠진다.
    - 첫 부분은 테스트 자료를 만든다.
    - 두 번째 부분은 테스트 자료를 조작하며,
    - 세 번째 부분은 조작한 결과가 올바른지 확인한다.
- 테스트 코드는 본론에 돌입해 진짜 필요한 자료 유형과 함수만 사용한다.
<br><br><br>

### 도메인에 특화된 테스트 언어

흔히 쓰는 시스템 조작 API를 사용하는 대신, API 위에 함수와 유틸리티를 구현한 후 그 함수와 유틸리티를 사용하면 테스트 코드를 짜기도 읽기도 쉬워진다.
<br><br><br>

### 이중 표준

테스트 API 코드에 적용하는 표준은 실제 코드에 적용하는 표준과 다르다.

실제 코드만큼 효율적일 필요 없다. 테스트 환경은 자원이 제한적일 가능성이 낮다. 실제 환경에서는 절대로 안되지만 테스트 환경에서는 문제 없는 방식이 있다. (메모리, CPU 효율)
<br><br><br>

### 테스트당 assert 하나?

*때로는 함수 하나에 여러 assert 문을 넣기도 한다. 대신 assert문 개수는 최대한 적게.*

JUnit으로 테스트 코드를 짤 때는 함수마다 assert 문을 하나만 사용해야 한다. assert문이 하나인 함수는 결론이 하나라서 코드를 이해하기 쉽다. 

테스트를 분리하면 중복되는 코드가 많아지는 데, 이때 `TEMPLATE METHOD 패턴`을 이용하면 중복을 제거할 수 있다. given/when 부분을 부모 긐캐르세 우도 then 부분을 자식 클래스에 둔다. 아니면 완전히 독자적인 테스트 클래스를 만들어서 @Before 함수에 given/when 부분을 넣고 @Test 함수에 then 부분을 넣는다.

—> 차라리 assert 문을 여러 개 사용하는 게 좋다.
<br><br><br>

### 테스트당 개념 하나 !
<br><br><br>

### 깨끗한 테스트의 규칙: F.I.R.S.T

- **Fast 빠르게** : 테스트는 빨리 돌아야 한다.
- **Independent 독립적으로** : 각 테스트는 서로 의존하면 X. 한 테스트가 다음 테스트가 실행될 환경을 준비해서는 안된다. 의존성 있으면 잇달아 실패해서 원인 진단하기 어려워짐.
- **Repeatable 반복가능하게** : 테스트는 어떤 환경에서도 반복 가능해야 한다. 네트워크 연결안된 상황이라도.
- **Self-Validating 자가검증하는** : 테스트는 Bool값으로 결과를 내야 한다. 통과여부를 알려고 로그 파일을 읽게 만들면 안된다.
- **Timely 적시에** : 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현해야 한다.
<br><br><br>

### 결론

- 개념 당 assert문 수를 최소로 줄여라.
- 테스트 함수 하나는 개념 하나만 테스트하라.
<br><br><br>
<br><br><br>

# 10장. 클래스

### 클래스 체계

- 클래스를 정의하는 표준 자바 관례 순서
1. 변수 목록
    1. public static 변수
    2. private static 변수
    3. private 인스턴스 변수
    - public 인스턴스 변수 (필요한 경우는 거의 없음)
2. 함수 목록
    1. public 함수
    2. private 함수 (자신을 호출하는 public 함수 직후에 넣는다. 따라서 추상화 단계가 순차적으로 내려간다.)
<br><br><br>

### 캡슐화

- 변수와 유틸리티 함수는 가능한 공개하지 않는 편이 낫지만 반드시 숨겨야 하다는 법칙은 없다.
- 때로는 변수나 유틸리티 함수를 protected로 선언해 테스트코드에 접근을 허용하기도 한다.
- 하지만 그 전에 비공개 상태를 유지할 온갖 방법을 강구한다.
- **캡슐화를 풀어주는 결정은 언제나 최후의 수단이다.**
<br><br><br>

### 클래스는 작아야 한다!

- 클래스를 만들때의 규칙은 크기다. 클래스는 작아야 한다.
- 단, 함수와는 다르게(함수는 물리적인 행 수로 측정), 클래스는 **맡은 책임**을 측정한다.

```java
// 목록 10-2, 메소드를 5개로 줄인다고 하더라도 여전히 책임이 많다.
public class SuperDashboard extends JFrame implements MetaDataUser {
  public Component getLastFocusedComponent()
  public void setLastFocused(Component lastFocused)
  public int getMajorVersionNumber()
  public int getMinorVersionNumber()
  public int getBuildNumber() 
}
```

- 클래스 이름은 해당 클래스 책임을 기술해야된다. 작명은 클래스 크기를 줄이는 첫번째 관문이다.
- 간결한 이름이 떠오르지 않는다면 클래스 책임이 너무 많아서이다.
- 예를들어 클래스 이름에 Manager, Processor, Super 등 모호한 이름을 포함할 경우 여러 책임이 있는 것이다.
- 클래스 설명은 "if", "and", "or", "but"을 사용하지 않고 25 단어 내외로 가능해야된다.
- 한글의 경우 만약, 그리고, ~하며, 하지만 이 들어가면 안된다. 들어갈 경우 책임이 많은것!
<br><br><br>

### 단일 책임 원칙

- 단일 책임의 원칙 (**SRP**, Single Responsibility Principle)은 클래스나 모듈을 변경할 이유가 단 하나뿐이어야 한다는 원칙이다.
- "책임" 이라는 개념을 정의하며 적절한 클래스 크기를 제시한다.
- 책임, 즉 변경할 이유를 파악하려고 애쓰다 보면 코드를 추상화 하기도 쉬워진다.
- 목록 10-2 의 경우 변경할 이유가 두가지이다.
    - 첫째 소프트웨어 버전 정보를 추적한다.
    - 둘째 자바 스윙 컴포넌트를 관리한다.

```java
// 목록 10-3, 단일 책임 클래스
// 버전 정보를 다루는 메서드 3개를 따로 빼서 Version 이라는 독자적인 클래스를 만들어 다른 곳에서 재사용하기 쉬워졌다.
public class Version {
  public int getMajorVersionNumber() 
  public int getMinorVersionNumber() 
  public int getBuildNumber()
}
```

- 단일 책임 클래스가 많아지면 큰그림을 이해하기 어려워진다고 우려한다.
    - 이 클래스 저클래스 수없이 넘나들어야 한다고 걱정한다.
- 작은 클래스가 많은 시스템이든, 큰 클래스가 몇 개뿐인 시스템이든 돌아가는 부품은 그 수가 비슷하다.
- 큰 클래스 몇개가 아니라 작은 클래스 여럿으로 이뤄진 시스템이 더 바람직하다.
- 작은 클래스는 각자 맡은 책임이 하나며, 변경할 이유가 하나며, 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.
<br><br><br>

### 응집도 **Cohesion**

- 클래스는 인스턴스 변수 수가 작아야 한다.
- 각 클래스 메서드는 클래스 인스턴스 변수를 하나 이상 사용해야 한다.
- 일반적으로 메서드가 변수를 더 많이 사용할 수록 메서드와 클래스는 응집도가 더 높다.
- 응집도가 높다는 말은 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미이다.

-

- 함수를 작게, 매개변수 목록을 짧게라는 전략을 따르다 보면 때때로 몇몇 메서드만이 사용하는 인스턴스 변수가 아주 많아진다.
- 이는 십중 팔구 새로운 클래스를 쪼개야 한다는 신호다.
- 응집도가 높아지도록 변수와 메서드를 적절히 분리해 새로운 클래스로 쪼개주면된다.
<br><br><br>

### **응집도를 유지하면 작은 클래스 여럿이 나온다**

- 큰 함수를 작은 함수 여럿으로 나누기만 해도 클래스 수가 많아진다.
- 예를 들어, 변수가 아주 많은 큰 함수가 하나 있다.
    - 큰 함수 일부를 작은 함수로 빼내고 싶다.
    - 빼내려는 코드가 큰 함수에 정의 된 변수를 많이 사용한다.
    - 그렇다면 변수들을 새 함수에 인수로 넘겨야 하나? 전혀 아니다!
    - 변수들을 클래스 인스턴스 변수로 승격 시키면 인수가 필요없다.
        - 그러나 이렇게하면 클래스가 응집력을 잃는다.
        - 몇몇 함수만 사용하는 인스턴스 변수가 점점 더 늘어나기 때문이다.
    - 몇몇 함수가 몇몇 인스턴스 변수만 사용한다면 독자적인 클래스로 분리해도 된다.
    - 클래스가 응집력을 잃는다면 쪼개야한다!
<br><br><br>

### 변경하기 쉬운 클래스

- 대다수 시스템은 지속적인 변경이 가해진다. 그리고 변경이 있을 때 마다 의도대로 동작하지 않을 위험이 따른다.
- 깨끗한 시스템은 클래스를 체계적으로 관리해 변경에 따르는 위험을 최대한 낮춘다.

ex) 주어진 메타 자료로 적절한 SQL 문자열 만드는 Sql 클래스.

현재 지원하지 않는 update문을 나중에 추가할 때 클래스에 손대지 않으려면?

```java
// 목록 10-10 닫힌 클래스 집합
abstract public class Sql {
  public Sql(String table, Column[] columns)
  abstract public String generate();
}

public class CreateSql extends Sql {
  public CreateSql(String table, Column[] columns)
  @Override public String generate()
}

public class SelectSql extends Sql {
  public SelectSql(String table, Column[] columns)
  @Override public String generate()
}

public class InsertSql extends Sql {
  public InsertSql(String table, Column[] columns, Object[] fields)
  @Override public String generate()
  private String valuesList(Object[] fields, final Column[] columns)
}

public class Where {
  public Where(String criteria)
  public String generate()
}

public class ColumnList {
  public ColumnList(Column[] columns)
  public String generate()
}
```

- 리팩토링
    - 공개 인터페이스를 각각 SQL 클래스에서 파생하는 클래스로 만들었다.
    - valueList와 같은 비공개 메서드는 해당하는 파생 클래스로 옮겼다.
    - 모든 파생 클래스가 공통으로 사용하는 메서드는 Where와 ColumnList 라는 두 유틸리티 클래스에 넣었다.
- 이렇게 하면 update 문을 추가 할때 기존 클래스를 변경할 필요가 전혀 없다.
- 또한 테스트 관점에서도 모든 논리를 검증하고 증명하기 쉬워졌다.
- 목록 10-10 은 **SRP**를 지원하며, **OCP**도 지원한다.
    - OCP(Open Closed Principle) : 클래스는 확장에 개방적이고 수정에 폐쇄적이어야 한다.
<br><br><br>

### 변경으로부터 격리

- 객체지향 프로그래밍에는 Concrete 클래스(구현)와 Abstract 클래스(개념)가 있다.
- 상세한 구현에 의존하는 클라이언트 클래스는 구현이 바뀌면 위험에 빠진다.
- 따라서 인터페이스와 abstract 클래스를 사용해 구현이 미치는 영향을 격리시켜야 한다.
- 추상화를 통해 테스트가 가능할 정도로 시스템의 결합도를 낮춤으로써 유연성과 재사용성도 더욱 높아진다.
    - 각 시스템 요소가 다른 요소로부터, 그리고 변경으로부터 잘 격리되어 있다는 의미.
    - 시스템 요소가 서로 잘 격리되어 있으면 각 요소를 이해하기 더 쉬움.
- 이렇게 결합도를 최소로 줄이면 자연스럽게 **DIP**를 따르는 클래스가 나온다.
    - DIP(Dependency Inversion Principle) : 클래스가 상세한 구현이 아니라 추상화에 의존해야 한다는 원칙