
<클린코드> 5~7장

# 5장. 형식 맞추기

### 형식을 맞추는 목적

처음에 잡아놓은 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 미친다. 원래 코드는 사라질지라도 개발자의 스타일과 규율은 사라지지 않는다.
<br><br><br>

### 적절한 행 길이를 유지하라

200줄 정도. 일반적으로 작은 파일이 이해하기 쉽다.
<br><br><br>
### 신문 기사처럼 작성하라

소스 파일 첫 부분은 고차원 개념과 알고리즘을 설명한다. 아래로 갈수록 의도를 자세하게 표현한다. 마지막에는 가장 저차원 함수와 세부 내역이 나온다.
<br><br><br>
### 개념은 빈 행으로 분리하라
<br><br><br>
## 세로 밀집도

세로 밀집도는 연관성을 의미한다. 서로 밀접한 코드 행은 세로로 가까이 놓여야 한다.

### **수직 거리**

밀접한 개념은 한 파일에 속해야 한다. 세로 거리로 연관성(한 개념을 이해하는데 다른 개념이 중요한 정도(을 표현한다.

- **변수 선언 :** 변수는 사용하는 위치에 최대한 가까이 선언한다. 짧은 함수에서 지역변수는 각 함수 맨 처음에 선언한다. 긴 함수에서 블록 상단이나 루프 직전에 변수를 선언하기도 한다.
- **인스턴스 변수** : 인스턴스 변수는 클래스 맨 처음에 선언한다. 변수 간에 세로로 거리를 두지 않는다. 위/아래든 각 언어에서 잘 알려진 위치에 인스턴스 변수를 모은다.
- **종속 함수** : 한 함수가 다른 함수를 호출하면 두 함수는 세로로 가까이 배치한다. 가능하면 호출하는 함수를 호출되는 함수보다 먼저 배치한다.
    - **상수 넘기기** : 특정 상수를 알아야 마땅한 함수에서 실제로 사용하는 함수로 그 상수를 넘겨야 한다. 상수가 적절하지 않은 저차원 함수에 묻히지 않도록.
- **개념적 유사성** : 친화도가 높을수록 코드를 가까이 배치한다. 친화도가 높은 경우는
    - 한 함수가 다른 함수를 호출 (직접적인 종속성)
    - 변수와 그 변수를 사용하는 함수
    - 비슷한 동작을 수행하는 함수들
- **세로 순서**
    - 호출되는 함수는 호출하는 함수보다 나중에 배치한다.
    - 가장 중요한 개념을 먼저 표현한다.
    - 가장 중요한 개념을 표현할 때는 세세한 사항을 최대한 배제한다.
    - 세세한 사항은 가장 마지막에 표현한다.
<br><br><br>
### 가로 형식 맞추기

프로그래머는 짧은 행을 선호한다. 최대 약 80자까지 선호. 120자로 제한.

- **가로 공백과 밀집도**
    - 가로로는 공백을 사용해서 밀접한 개념과 느슨한 개념을 표현한다.
    - 할당연산자를 강조하기 위해 앞뒤에 공백을 둔다.  `i += 1`
    - 함수 이름과 이어지는 괄호 사이에는 공백을 넣지 않는다. `public void func() {..}`
    - 괄호 안 인수는 공백으로 분리한다.
    - 연산자 우선순위를 강조하기 위해 공백을 사용한다.
        - 승수 사이에는 공백 X
        - 코드 형식 맞춰주는 도구 대다수가 연산자 우선순위 고려 못하므로 수식에 똑같은 간격을 사용한다.
- **가로 정렬**
    - 선언부의 변수이름이나 할당문의 오른쪽 피연산자에 대해 가로 정렬은 별로 유용하지 못하다.
    - 코드의 엉뚱한 부분을 강조해서 진짜 의도가 가려진다.
    - 정렬이 필요할 정도로 목록이 길다면 목록 길이가 문제인 것. 선언부가 길다면 클래스를 쪼개야 한다.
- **들여쓰기**
    - 계층에서 각 수준은 이름을 선언하는 범위이자, 선언문과 실행문을 해석하는 범위이다.
    - 범위로 이루어진 계층을 표현하기 위해 코드를 들여쓴다.
    - 들여쓰는 정도는 계층에서 코드가 자리잡은 수준에 비례한다.
    - 클래스 내부의 메서드는 클래스보다 한 수준 들여쓴다.
    - **들여쓰기 무시하기 X**
        - 간단한 if, while, 함수는 한 행으로 쓰고 싶기도 함. 그래도 들여쓰기.
    - **가짜범위**
        - 비어있는 while, for문 ..
        - 피하지 못할 때는 빈 블록을 올바르게 들여쓰고, 괄호로 감싼다.
        - while 문 끝에 세미콜론 붙일 때 새 행에 제대로 들여쓰기.
- **팀 규칙**
    - 모든 팀원은 그 규칙을 따라야 한다.
    - 어디에 괄호를 넣을지, 들여쓰기는 몇 자로, 클래스/변수/메서드 명명법 등
    - 정한 규칙에 따라 IDE 코드 형식기를 설정한다.
<br><br><br>
# 6장. 객체와 자료 구조

### 자료 추상화

- 변수를 private으로 선언하더라도 각 값마다 get함수와 set함수를 제공하면, 구현을 외부로 노출한 것이다.
- 구현을 감추려면 추상화가 필요하다. 함수라는 계층을 넣는다고 구현이 감춰지지 않는다.
- 추상 인터페이스를 제공해서 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한 의미의 클래스이다.
- 개발자는 객체가 포함하는 자료를 표현할 가장 좋은 방법을 고민해야 한다.
- 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 게 좋다.
- 인터페이스나 조회/설정 함수만으로는 추상화가 안 이뤄진다.
<br><br><br>

1-1. 구체적인 Point 클래스 (구현을 외부로 노출)

```java
public class Point {
  private double x; // 직교좌표계 사용
  private double y;
}
```

1-2. 추상적인 Point 클래스 (구현을 완전히 숨김)

```java
public interface Point {
  double getX(); // 직교좌표계인지 극좌표계인지 모름
  double getY();
  void setCatesian(double x, double y);
  double getR();
  double getTheta();
  void setPolar(double r, double theta);
}
```

1-3. 구체적인 Vehicle 클래스 (구현을 외부로 노출)

```java
public interface Vehicle {
  double getFuelTankCapacityInGallons(); // 자동차 연료 상태를 구체적 숫자로 알려줌
  double getGallonsOfGasoline();
}
```

1-4. 추상적인 Vehicle 클래스 (구현을 완전히 숨김)

```java
public interface Vehicle {
  double getPercentFuelRemaining(); // 자동차연료상태를 백분율이라는 추상적개념으로 알려줌
}
```
<br><br><br>
### 자료/객체 비대칭

- **객체** : 함수만 공개. 추상화 뒤로 자료를 숨김.
- **자료 구조** : 자료를 공개. 함수 제공 X
<br><br><br>

2-1. 절차적인 코드 (자료 구조)

새 도형 추가 —> Geometry 클래스의 함수 모두 고쳐야 함.

새 함수 추가 —> 도형 클래스들(Square, Rectangle, Circle)은 영향 X.

```java
public class Square {
	public Point topLeft;
	public double side;
}

public class Rectangle {
	public Point topLeft;
	public double height;
	public double width;
}

public class Circle {
	public Point center;
	public double radius;
	public double width;
}

public class Geometry {
	public final double PI = 3.141592653585793;

	public double area(Object shape) throws NoSuchShapeException {
		if(shape instanceOf Square) {
			Square s = (Square)shape;
			return s.side * s.side;
		}
	else if(shape instanceOf Rectangle) {
			Rectangle r = (Rectangle)shape;
			return r.height * r.width;
		}
	else if(shape instanceOf Circle) {
			Circle c = (Circle)shape;
			return PI * c.radius * c.radius
		}
	}
}
```

2-2. 다형적인 코드 (객체지향)

새 도형 추가 —>기존 함수에 영향 X. 

새 함수 추가 —> 도형 클래스 전부 고쳐야 함. (VISITOR 기법)
<br><br><br>
<aside>
📎 **VISITOR 패턴**

---

동작하는 객체로부터 알고리즘을 분리하는 패턴.

상속 없이 클래스에 메서드를 효과적으로 추가하기 위해 사용. 대신 캡슐화 위반.

데이터 구조 안을 돌아다니는 주체인 방문자를 나타내는 클래스를 준비해서 처리를 맡긴다. 

새로운 처리를 추가하고 싶을 땐 새로운 방문자를 만들고 데이터 구조는 방문자를 받아들이면 된다.

방문자 패턴은 **개방-폐쇠 원칙**을 적용하는 방법 중 한 가지이다.

[확장에 대해 열려있다.]

클래스를 설계할 때, 특별한 이유가 없는 한 확장을 금지해서는 안된다.

[수정에 대해 닫혀있다.]

확장을 할 때 마다 기존의 클래스를 수정하면 안된다.

- 예시
    
    ```java
    // Element.java
    public interface Element {
        public void accept(Visitor visitor);
    }
    
    // Cart.java
    public class Cart implements Element {
    	ArrayList<Element> cart = new ArrayList<>();
        
        public Cart() {
        	cart.add(new Snack());
            cart.add(new Milk());
        }
    
        @Override
        public void accept(Visitor visitor) {
        	System.out.println("카트가 준비되었습니다.");
            visitor.visit(this);
            
            for(Element element : cart) {
            	element.accept(visitor);
            }
        }
    }
    
    // Snack.java
    public class Snack implements Element {
        @Override
        public void accept(Visitor visitor) {
        	System.out.println("과자가 준비되었습니다.");
            visitor.visit(this);
        }
    }
    // Milk.java
    public class Milk implements Element {
        @Override
        public void accept(Visitor visitor) {
        	System.out.println("우유가 준비되었습니다.");
            visitor.visit(this);
        }
    }
    // Visitor.java
    public interface Visitor {
        public void visit(Cart cart);
        public void visit(Snack snack);
        public void visit(Milk milk);
    }
    
    // Shopper.java
    public class Shopper implements Visitor {
    	@Override
        public void visit(Cart cart) {
        	System.out.println("카트를 사용합니다.");
        }
        
        @Override
        public void visit(Snack snack) {
        	System.out.println("과자를 카트에 넣습니다.");
        }
        
        @Override
        public void visit(Milk milk) {
        	System.out.println("우유를 카트에 넣습니다.");
        }
    }
    
    // Client.javva
    public class Client {
        public static void main(String args[]) {
            Shopper shopper = new Shopper();
            Cart cart = new Cart();
            cart.accept(shopper);
        }
    }
    ```
    
</aside>

```java
public class Square implements Shape {
	public Point topLeft;
	public double side;

	public double area() {
		return side*side;
	}
}

public class Rectangle implements Shape {
	public Point topLeft;
	public double height;
	public double width;

	public double area() {
		return height*width;
	}
}

public class Circle implements Shape {
	public Point center;
	public double radius;
	public double width;

	public double area() {
		return PI*radius*radius;
	}
}
```

> **자료 구조**를 사용하는 절차적인 코드는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다. 반면, **객체 지향 코드**는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.
> 

> **절차적인 코드**는 새로운 자료 구조를 추가하기 어렵다. 그러려면 모든 함수를 고쳐야 한다.
> 
> 
> **객체 지향 코드**는 새로운 함수를 추가하기 어렵다. 그러려면 모든 클래스를 고쳐야 한다.
> 

→ 객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 반대는 객체 지향 코드에서 쉽다!
<br><br><br>
### 디미터 법칙

: 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다. 

객체는 자료를 숨기고 함수를 공개한다.

→ 객체는 조회 함수로 내부를 공개하면 안 된다는 의미

클래스 C의 메서드 f는 아래 객체의 메서드만 호출해야 한다.

1. 클래스 C
2. f가 생성한 객체
3. f 인수로 넘어온 객체
4. C 인스턴스 변수에 저장된 객체

**기차 충돌**

: 여러 객체가 한 줄로 이어진 기차처럼 보이는 것. 조잡해 보임.

`String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();`

→ 메소드가 반환하는 객체의 메소드를 사용해서 디미터 법칙 위반

→ 아래 코드로 변환하는 것이 바람직함.

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
String outputDir = scratchDir.getAbsolutePath();
```

→ ctxt, opts, scratchDir이 **객체**라면 디미터 법칙 위반

→ ctxt, opts, scratchDir이 **자료 구조**라면 내부 구조를 노출하므로 디미터 법칙이 적용되지 않음.

```java
final String outputDir = cxtx.opts.scratchDir.absolutePath;
```

코드를 위와 같이 구현했다면 디미터 법칙을 거론할 필요가 없어진다.

자료구조는 무조건 함수 없이 공개 변수만 포함하고, 객체는 비공개 변수와 공개 함수를 포함한다면 된다.

**잡종 구조**

절반은 객체, 절반은 자료 구조인 잡종 구조.

- 공개 변수, 공개 함수, 주요 함수, getter, setter 모두 섞여 있는 구조
    - public 조회/설정 함수는 private 변수를 그대로 노출
- 클래스, 자료 구조 양쪽에서 단점만 모아 놓은 피해야 할 구조→ 새로운 함수는 물론, 새로운 자료 구조도 추가하기 어렵다.

**구조체 감추기**

`ctxt`, `options`, `scratchDir`이 객체라면 함수 내에서 줄줄이 사탕으로 호출해서는 안된다. 객체라면 내부 구조를 감춰야 하기 때문.

```java
// ctxt 객체에 임시 파일 생성하라고 시킴
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

ctxt 객체는 내부구조를 드러내지 않으며, 함수는 자신이 몰라야 하는 여러 객체를 탐색할 필요가 없다.
<br><br><br>
## 자료 전달 객체

자료 구조체의 전형적인 형태는 공개 변수만 있고 함수가 없는 클래스다. 이런 자료 구조를 DTO(Data Transfer Object)라 한다.

***DTO***

공개 변수만 있고 함수가 없는 클래스

***Bean***

비공개 변수와 getter, setter가 있는 클래스

***활성 레코드***

DTO의 특수한 형태. 공개, 비공개 변수와 getter, setter, 그리고 탐색 함수가 있는 자료구조. —> 활성레크를 객체 취급하지 마라. 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성.
<br><br><br>

## 결론

> **객체**는 동작을 공개하고 자료를 숨긴다.
> 
> 
> → 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기는 쉽다.
> 
> → 기존 객체에 새 동작을 추가하기는 어렵다.
> 

> **자료 구조**는 별다른 동작 없이 자료를 노출한다.
> 
> 
> → 기존 자료구조에 새 동작을 추가하기는 쉽다.
> 
> → 기존 함수에 새 자료 구조를 추가하기는 어렵다.
> 
<br><br><br>

**바람직한 구조**

- 객체: 비공개 변수와 공개 함수만 포함
- 자료 구조: 함수 없이 공개 변수만 포함

**적합한 쓰임**

- 객체: 새로운 자료 타입 추가
- 자료 구조: 새로운 메소드 추가
<br><br><br>

# 7장. 오류 처리

오류 처리 코드가 여기저기 흩어져 있으면 코드가 하는 일을 하악하기 어려워진다.
<br><br><br>

## 오류 코드보다 예외(exception)를 사용하라

오류 플래그 설정 / 호출자에게 오류 반환하는 방법 —> 호출자 코드 복잡, 오류 확인 잊어버림

- 오류를 발견하면 예외를 던지는 코드

→ 가독성 높아짐. 디바이스 종료 알고리즘 & 오류 처리 알고리즘 분리

```java
public class DeviceController {
	...
	public void sendShutDown() {
		try {
			tryToShutDown();
		} catch (DeviceShutDownError e) {
			logger.log(e);
		}
	}

	private void tryToShutDown() throws DeviceShutDownError {
		DeviceHandle handle = getHandle(DEV1);
		DeviceRecord record = retrieveDeviceRecord(handle);
		pauseDevice(handle); 
		clearDeviceWorkQueue(handle); 
		closeDevice(handle);
	}

	private DeviceHandle getHandle(DeviceID id) {
		...
		throw new DeviceShutDownError("Invalid handle for: " + id.toString());
		...
	}
	...
}
```

## Try-Catch-Finally 문부터 작성하라

try 블록은 트랜잭션과 비슷. try 블록에서 무슨 일이 생기든 catch 블록은 프로그램 상태를 일관성 있게 유지해야 한다.

### *TDD 방식으로 메소드 구현*

1. *단위 테스트* 를 만든다.
    
    ```java
    @Test(expected = StorageException.class)
     public void retrieveSectionShouldThrowOnInvalidFileName() {
         sectionStore.retrieveSection("invalid - file");
     }
    ```
    
2. 단위 테스트에 맞춰 *코드를 구현* 한다.
    
    ```java
    public List<RecordedGrip> retrieveSection(String sectionName) {
         // 실제로 구현할 때까지 비어 있는 더미를 반환한다.
         return new ArrayList<RecordedGrip>();
     }
    ```
    
    예외가 발생하지 않기 때문에 *단위 테스트에서 실패* 한다.
    
3. 파일 접근을 시도하도록 구현한다.
    
    ```java
    public List<RecordedGrip> retrieveSection(String sectionName) {
         try {
             FileInputStream stream = new FileInputStream(sectionName);
         } catch (Exception e) {
             throw new StorageException("retrieval error", e);
         }
         return new ArrayList<RecordedGrip>();
     }
    ```
    
    테스트가 성공할 것이다.
    
4. *리펙터링*이 가능해졌다.
    
    ```java
    public List<RecordedGrip> retrieveSection(String sectionName) {
         try {
             FileInputStream stream = new FileInputStream(sectionName);
             stream.close();
         } catch (FileNotFoundException e) {
             throw new StorageException("retrieval error", e);
         }
         return new ArrayList<RecordedGrip>();
     }
    ```
    

## 미확인 예외를 사용하라

### 미확인 예외란?

*checked 예외* 는 컴파일 단계에서 확인되며 반드시 처리해야 하는 예외.

- IOException
- SQLException

*Unchecked 예외* 는 실행 단계에서 확인되며 명시적인 처리를 강제하지는 않는 예외.

- NullPointerException
- IllegalArgumentException
- IndexOutOfBoundException
- SystemException

### 확인된 예외의 단점

확인된 예외는 OCP(Open Closed Principle)을 위반한다.

<aside>
📎 **OCP**
기존의 코드를 변경하지 않으면서, 기능을 추가할 수 있도록 설계가 되어야 한다는 원칙

</aside>

하위 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 전부 고쳐야 한다. 모듈과 관련된 코드가 전혀 바뀌지 않아도 모듈을 다시 빌드한 다음 배포해야 한다.

• 확인된 예외는 예상되는 모든 예외를 사전에 처리할 수 있다는 장점이 있지만, 일반적인 애플리케이션은 의존성이라는 비용이 이익보다 더 크다.

## 예외에 의미를 제공하라

오류 메세지에 정보를 담아 예외와 함께 던진다. 실패한 연산 이름과 실패 유형도 언급한다.

catch 블럭에서 로깅 기록.

## 호출자를 고려해 예외 클래스를 정의하라

가장 큰 관심사는 오류를 잡아내는 방법이어야 함.

1. 외부 라이브러리를 호출하고 모든 예외를 호출자가 잡아내고 있는 코드.
    
    ```java
    ACMEPort port = new ACMEPort(12);
    
     try {
         port.open();
     } catch (DeviceResponseException e) {
         reportPortError(e);
         logger.log("Device response exception", e);
     } catch (ATM1212UnlockedException e) {
         reportPortError(e);
         logger.log("Unlock exception", e);
     } catch (GMXError e) {
         reportPortError(e);
         logger.log("Device response exception");
     } finally {
         ...
     }
    ```
    
    위 경우는 예외에 대응하는 방식이 예외 유형과 무관하게 거의 동일함
    
2. 호출 라이브러리 API를 감싸 한가지 예외 유형을 반환하는 방식으로 단순화
    
    ```java
    LocalPort port = new LocalPort(12);
     try {
         port.open();
     } catch (PortDeviceFailure e) {
         reportError(e);
         logger.log(e.getMessage(), e);
     } finally {
         ...
     }
    ```
    
    ```java
    public class LocalPort {
         private ACMEPort innerPort;
    
    		// localPort 클래스는 ACMEPort 클래스가 던지는 예외를 잡아 변환하는 wrapper 클래스임
         public LocalPort(int portNumber) { 
             innerPort = new ACMEPort(portNumber);
         }
    
         public void open() {
             try {
                 innerPort.open();
             } catch (DeviceResponseException e) {
                 throw new PortDeviceFailure(e);
             } catch (ATM1212UnlockedException e) {
                 throw new PortDeviceFailure(e);
             } catch (GMXError e) {
                 throw new PortDeviceFailure(e);
             }
         }
         ...
     }
    ```
    

외부 API를 감싸면 아래와 같은 장점이 있다.

- 에러 처리가 간결해짐
- 외부 라이브러리와 프로그램 사이의 의존성이 크게 줄어듦
- 프로그램 테스트가 쉬워짐
- 외부 API 설계 방식에 의존하지 않아도 됨
<br><br><br>
## 정상 흐름을 정의하라

때로는 중단이 적합하지 않은 때도 있다.

- **특수 사례 패턴**
    - 클래스를 만들거나 객체를 조작해서 특수 사례 처리 (클래스/객체가 예외적인 상황을 캡슐화해서 처리)
    - 클라이언트 코드가 예외적인 상황을 처리할 필요 없게 한다.
    

1.총합 계산하는 코드

```java
 try {
     MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
     m_total += expenses.getTotal();
 } catch(MealExpencesNotFound e) {
     m_total += getMealPerDiem();
 }
```

2.*getTotal* 메소드에 예외 시 처리를 넣어 클라이언트 코드를 간결하게 처리

```java
public class PerDiemMealExpenses implements MealExpenses {
     public int getTotal() {
         // 기본값으로 일일 기본 식비를 반환한다.
         // (예외가 아닌)
     }
 }
```

```java
// 클라이언트 코드
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
 m_total += expenses.getTotal();
```
<br><br><br>
## null을 반환하지 마라

null 반환의 문제점

- 호출자에게 null을 체크할 의무를 줌
- *NullPointerException* 의 발생 위험이 있음
- null확인이 너무 많아짐

차라리 예외를 던지거나 특수 사례 객체를 반환하라.

사용하는 외부 API가 null을 반환한다면 감싸기 메서드를 구현해 예외를 던지거나 특수사례객체 반환하도록 한다.

null 확인하는 코드

```java
public void registerItem(Item item) {
	if (item != null) {
		ItemRegistry registry = peristentStore.getItemRegistry();
		if (registry != null) {
			Item existing = registry.getItem(item.getID());
			if (existing.getBillingPeriod().hasRetailOwner()) {
				existing.register(item);
			}
		}
	}
}
```

```java
// bad
List<Employee> employees = getEmployees();
if(employees != null) {
	for(Employee e : employees) {
		totalPay += e.getPay();
	}
}

// good
List<Employee> employees = getEmployees();
for(Employee e : employees) {
	totalPay += e.getPay();
}

public List<Employee> getEmployees() {
	if (..직원이 없다면..)
		return Collections.emptyList(); // 빈 리스트 반환
}
```
<br><br><br>
## null을 전달하지 마라

정상적인 인수로 null을 기재하는 API가 아니라면 null 전달하는 방식은 지양. null 은 반환 뿐 아니라 인수 값으로 전달 하는 방식도 피해야 한다.

대안:

- 새로운 예외 유형을 만든다
    
    ```java
    public double xProjection(Point p1, Point p2) {
        if (p1 == null || p2 == null) {
            throw InvalidArgumentException("Invalid..예외발생");
        }
        return (p2.x - p1.x) * 1.5
    }
    ```
    
- assert문 사용
    
    ```java
    public class MetricsCalculator {
    	public double xProjection(Point p1, Point p2) {
    		assert p1 != null : "p1 should not be null";
    		assert p2 != null : "p2 should not be null";
    
    		return (p2.x - p1.x) * 1.5;
    	}
    }
    ```
    
<br><br><br>
## 결론

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.
- 오류 처리를 프로그램 논리와 분리해서 독자적인 사안으로 고려해야 한다.
