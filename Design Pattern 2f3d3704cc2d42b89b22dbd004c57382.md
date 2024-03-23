# Design Pattern

태그: CSE

# 라이브러리와 프레임워크의 비교

**프레임워크**는 일반적으로 **라이브러리보다 제약이 있으나, 특정 기능들과 규약을 따르면 빠른 개발과 유지보수를 가능하게 해준다**. 반면, **라이브러리는** 좀 더 프레임워크보다 **자유로운 사용을 허용하며, 개발자가 직접 필요한 기능들을 직접 선택하여 활용할 수 있게 한다**.

## 라이브러리(Library)

- **공통으로 사용될 수 있는 특정한 기능들을 모듈화 한 것(프레임워크와 동일)**
- 폴더명, 파일명 등에 대한 규칙이 없다.
- 프레임워크에 비해 자유롭다. 즉, **자유도가 높다.**
- 개발자가 필요한 기능을 직접 호출하여 사용할 수 있다.
- 개발자가 라이브러리를 자유롭게 조합하여 사용할 수 있으며, 라이브러리를 프로젝트에 맞게 선택하고 적용할 수 있다.
- 제공되는 기능들이 프레임워크에 비해 적다.
- 사용 규칙이 아예 없는 것은 아니다.(메서드 이름, 매개변수 등은 지켜줘야…)
- 라이브러리는 프레임워크에 넣을 수 있다.

> 서울 → 대전까지 가는 경로는 다양하다. 어느 경로를 선택하든 내 마음이다.
> 

## 프레임워크(Framework)

- **공통으로 사용될 수 있는 특정한 기능들을 모듈화 한 것(라이브러리와 동일)**
- 폴더명, 파일명 등에 대한 규칙이 있다.
- 라이브러리에 비해 좀 더 엄격하다. 즉, **자유도가 낮다.**
- 개발자는 프레임워크의 **규칙과 규약을 따라야 한다**.
- 프레임워크는 **제어의 역전(Inversion of Control)** 개념이 적용된다. 즉, 제어의 권한이 프레임워크에 있어서, 프레임워크가 프로그램의 구조와 흐름을 자체적으로 제어하므로 개발자는 그 안에서 코드를 작성해야 한다.
- 제공되는 **기능들이 라이브러리에 비해 많다.**

> 서울 → 제주까지 **비행기**로 가는 경로는 내가 선택할 수 없다.
> 

### 제어의 역전(Inversion of Control)

---

**프로그램의 제어 흐름을 개발자가 직접 제어하는 것이 아니라, 프레임워크나 컨테이너가 대신 제어**하는 것

로봇을 만든다고 가정해보면, 로봇은 먹을 수 있어야 하고, 에너지가 없으면 움직일 수 없어야 한다. 

```java
public class Robot {
    private int energy;

    public Robot() {
        this.energy = 100;
    }

    public void eat(int foodEnergy) {
        this.energy += foodEnergy;
    }

    public void move() {
        if (energy > 0) {
            System.out.println("로봇이 움직입니다.");
            energy -= 10;
        } else {
            System.out.println("에너지가 부족합니다. 충전이 필요합니다.");
        }
    }
}
```

이 코드에서는 로봇이 에너지를 소비하고 충전하며 동작한다. 이렇게 코드 내부에서 로봇의 상태와 동작을 직접 관리하는 것이 일반적인 제어 방식이다.

하지만 제어의 역전을 적용하면, 프레임워크나 컨테이너가 로봇의 동작과 에너지 관리를 대신 해줄 수 있다. 예를 들어, Spring Framework를 사용하여 제어의 역전을 적용해보면 다음과 같다.

```java
// Robot 클래스는 인터페이스를 구현한다.
public class Robot implements Moveable {
    private int energy;

    public Robot() {
        this.energy = 100;
    }

    public void eat(int foodEnergy) {
        this.energy += foodEnergy;
    }

    **// 제어의 역전으로 구현된 move 메서드**
    @Override
    public void move() { // move라는 메서드 이름은 프레임워크가 제공해준다.
        if (energy > 0) {
            System.out.println("로봇이 움직입니다.");
            energy -= 10;
        } else {
            System.out.println("에너지가 부족합니다. 충전이 필요합니다.");
        }
    }
}

// **Moveable 인터페이스**
public interface Moveable {
    void move();
}
```

이제 Robot 클래스는 Moveable 인터페이스를 구현하여 동작한다. 실제 동작은 move 메서드 안에서 처리되지만, 로봇의 상태 관리는 Robot 클래스 내부에 위치한다.

제어의 역전을 이해하기 위해서는 Spring Framework와 같은 컨테이너가 어떻게 동작하는지 자세히 이해해야 하지만, 이해하기 쉽게 설명하자면, 컨테이너가 Robot 객체를 생성하고 move 메서드를 호출해주는 것이다. 이렇게 하면 로봇의 동작을 개발자가 직접 제어할 필요 없이 컨테이너가 대신 처리해준다.

# 디자인 패턴 정의

- 프로그램을 설계할 때 발생하는 **반복적으로 발생하는 문제들을 해결하기 위한 해결책(패턴)**을 정형화한 것들
- 이러한 패턴들은 이미 그 설계와 구조가 정해져 있고, 검증되었다.
- 개발자들이 **비슷한 상황에서 효과적으로 문제를 해결할 수 있도록 도와준다**.
- 디자인 패턴은 라이브러리나 프레임워크, 모든 소프트웨어에 적용될 수 있다.

# 디자인 패턴 장점

- 코드의 가독성, 재사용성, **유지보수성, 확장성**을 향상시킨다.
- 개발자들 간에 **빠르고 정확한 의사소통**을 가능하게 한다.
    
    > *“그 부분은 전략패턴으로 하는 게 어떤가요?”*
    > 

# 디자인 패턴 종류

## **생성패턴(Creational Patterns)**

객체의 **인스턴스 생성과 관련된 디자인 패턴**

> *“객체를 어떻게 생성해야 할까?”*
> 
- **Singleton(싱글톤) 패턴**
- **Factory(팩토리) 패턴**

## **구조패턴(Structural Patterns)**

**클래스나 객체들을 조합하여 더 큰 구조를 만드는데 사용**되는 패턴

> *“생성된 객체들을 어떻게 조직화/구조화하여 효율적으로 만들 것인가?”*
> 

객체 간의 관계를 정의하여, 유연하고 효율적인 구조를 제공한다. 주로 클래스나 객체들의 **인터페이스와 구현의 분리, 상속** 등을 다룬다.

- **프록시(Proxy) 패턴**
- **데코레이터(Decorator) 패턴**

## **행동패턴(Behavioral Patterns)**

**객체들 간의 상호작용과 알고리즘, 책임 할당에 관련**된 패턴

> *“문제를 해결하려면 이 객체들로 무엇을, 어떻게 해야할까?”*
> 

**객체들 사이의 효율적인 통신과 책임의 분산**을 강조한다. 주로 **알고리즘, 이벤트 처리, 객체들의 상태 변화** 등 다양한 행동에 대한 문제들을 해결하는데 사용된다.

- **전략(Strategy) 패턴**
- **옵저버(Observer) 패턴**
- **이터레이터(Iterator) 패턴**

# SOLID 원칙

객체 지향 프로그래밍에서 소프트웨어 디자인을 개선하고 유지 보수를 용이하게 하기 위해 제안된 다섯 가지 원칙들의 묶음

각 원칙은 개별적으로도 중요하지만 함께 사용되었을 때 더 강력한 결과를 얻는다. 

SOLID 원칙을 잘 따르면 코드의 유지 보수성이 높아지고, 변경에 더 유연하게 대응할 수 있으며, 더 좋은 객체 지향 설계를 할 수 있다.

### SRP (단일 책임 원칙 - Single Responsibility Principle)

---

**한 클래스는 단 하나의 책임**을 가져야 한다. 이렇게 함으로써 클래스의 응집성(cohesion)을 높이고, 클래스의 변경이 다른 클래스에 영향을 미치지 않도록 하며, 코드의 유지보수성과 확장성을 높인다.

```java
// SRP를 준수하는 예시 코드

// 학생 정보를 나타내는 클래스
class Student {
    private String name;
    private int age;
    private int studentID;

    // 학생 정보를 출력하는 메서드
    public void printStudentInfo() {
        System.out.println("이름: " + name);
        System.out.println("나이: " + age);
        System.out.println("학번: " + studentID);
    }
}

// 학생 정보를 파일로 저장하는 기능을 담당하는 클래스
class StudentFileHandler {
    // 학생 정보를 파일로 저장하는 메서드
    public void saveStudentToFile(Student student) {
        // 파일 저장 로직 구현
        // ...
    }

    // 파일에서 학생 정보를 읽어오는 메서드
    public Student readStudentFromFile() {
        // 파일 읽기 로직 구현
        // ...
        return new Student();
    }
}
```

위의 코드에서 각 클래스는 하나의 책임만을 가지고 있으며, **`Student`** 클래스와 **`StudentFileHandler`** 클래스는 서로 독립적으로 변화할 수 있다.

### OCP (개방 폐쇄 원칙 - Open/Closed Principle)

---

확장에는 열려 있어야 하지만, 수정에는 닫혀 있어야 한다. 즉 새로운 기능을 추가할 때 기존 코드를 변경하지 않고도 기능을 확장할 수 있도록 설계해야 한다. 이를 위해 인터페이스와 추상화 등을 사용한다. 

아래 코드에서는 OCP를 준수하기 위해 도형을 나타내는 **`Shape`** 클래스를 추상 클래스로 정의하고, 이를 상속받는 **`Circle`**과 **`Rectangle`** 클래스를 구현한다. 이렇게 함으로써 **새로운 도형 클래스를 추가할 때 기존의 `AreaCalculator` 클래스는 수정하지 않아도 된다**. **즉, 기능 확장에는 개방되어 있지만, 기존 코드는 수정되지 않으므로 수정에는 폐쇄적인 특성을 갖게 된다**.

```java
// 도형을 나타내는 기본 추상 클래스
abstract class Shape {
    abstract double calculateArea();
}

// 원을 나타내는 클래스
class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }

    public void setRadius(double radius) {
        this.radius = radius;
    }

    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
}

// 사각형을 나타내는 클래스
class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double getWidth() {
        return width;
    }

    public void setWidth(double width) {
        this.width = width;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    @Override
    double calculateArea() {
        return width * height;
    }
}

// 도형 계산을 담당하는 클래스
class AreaCalculator {
    public static double calculateArea(Shape shape) { 
		// abstract class Shape 의 calculateArea 와 이름이 꼭 같지는 않아도 된다.
        return shape.calculateArea();
    }
}

// 사용 예시
public class Main {
    public static void main(String[] args) {
        Circle circle = new Circle(5.0);
        double circleArea = AreaCalculator.calculateArea(circle);
        System.out.println("원의 넓이: " + circleArea);

        Rectangle rectangle = new Rectangle(4.0, 6.0);
        double rectangleArea = AreaCalculator.calculateArea(rectangle);
        System.out.println("사각형의 넓이: " + rectangleArea);
    }
}
```

### LSP (리스코프 치환 원칙 - Liskov Substitution Principle)

---

하위 클래스는 상위 클래스의 인스턴스로 대체 가능해야 한다. 즉, **어떤 클래스가 상속 관계에 있을 때, 상위 클래스의 기능을 하위 클래스가 무시하지 않고 재정의해야 한다**. 이를 통해 다형성을 지원하고, 계층 구조를 안정적으로 유지할 수 있다.

```java
// 상위 클래스 Rectangle
class Rectangle {
    protected int width;
    protected int height;
    
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
		// default 생성자가 없으므로 유연성이 다소 떨어진다고 볼 수 있다.
    
    public int getArea() {
        return width * height;
    }
}

// 하위 클래스 Square
class Square extends Rectangle {
    public Square(int side) {
        super(side, side);
    }
    
    // 하위 클래스에서는 getArea()를 재정의(Override)하지 않음
    // 상위 클래스의 메서드를 그대로 상속받아 사용
}

public class Main {
    public static void main(String[] args) {
        Rectangle rectangle = new Rectangle(4, 5);
        printArea(rectangle);

        Square square = new Square(4);
        printArea(square);
    }

    public static void printArea(Rectangle rect) {
        System.out.println("넓이: " + **rect.getArea()**);
    }
}
```

주목해야 할 점은 **`Square`** 클래스에서 **`getArea()`** 메서드를 따로 재정의하지 않았음에도 불구하고, printArea 메서드에서 getArea를 호출하는 부분이다. 이는 LSP를 위반하는 상황이다. LSP를 지키기 위해서는 하위 클래스에서는 상위 클래스의 메서드를 적절하게 오버라이딩하여 사용해야 한다.

예를 들어, **`Square`** 클래스에서 **`getArea()`** 메서드를 다음과 같이 재정의할 수 있다.

```java
// 하위 클래스 Square
class Square extends Rectangle {
    public Square(int side) {
        super(side, side);
    }

    @Override
    **public int getArea() {
        return width * width;
    }**
}
```

이렇게 하면 **`Square`** 클래스는 상위 클래스인 **`Rectangle`** 클래스의 기능을 존중하며 재정의하였으므로 LSP를 지킨 코드가 된다. 이렇게 하위 클래스는 상위 클래스를 무시하지 않고 사용할 수 있다.

### ISP (인터페이스 분리 원칙 - Interface Segregation Principle)

---

클라이언트는 자신이 사용하지 않는 인터페이스에 의존하지 않아야 한다. 즉, 여러 개의 작은 인터페이스가 범용적인 인터페이스 하나보다 낫다. 다시 말하자면, 한 인터페이스에 너무 많은 메서드가 있으면 해당 인터페이스를 구현하는 클래스들이 **사용하지 않는 메서드까지 구현해야 하는 상황이 발생하므로**, **인터페이스를 더 작은 단위로 분리**해야 한다. 이를 통해 불필요한 의존성을 줄일 수 있다.

```java
// 인터페이스
interface Soundable {
    void makeSound();
}

// 동물 클래스들
class Dog implements Soundable {
    @Override
    public void makeSound() {
        System.out.println("멍멍!");
    }
}

class Cat implements Soundable {
    @Override
    public void makeSound() {
        System.out.println("야옹~");
    }
}

class Fish implements Soundable {
    // Fish는 소리를 내지 않음, 무의미한 메서드를 구현해야 함
    @Override
    public void makeSound() {
        // 아무 동작도 하지 않음
    }
}
```

모든 동물들이 소리를 내는 것은 아니고, 소리를 내는 동물들도 각각 다른 소리를 낼 수 있다. 이런 상황에서 인터페이스를 분리하지 않으면 모든 동물 클래스가 무의미한 메서드를 구현해야 하는 문제가 발생한다. 위의 코드에서 **`Fish`** 클래스는 소리를 내지 않지만, **`Soundable`** 인터페이스를 구현하기 때문에 **`makeSound()`** 메서드를 무의미하게 구현해야 한다. 이를 해결하기 위해 인터페이스를 분리한다.

```java
// 인터페이스 분리
interface Soundable {
    void makeSound();
}

interface Swimmable {
    void swim();
}

// 동물 클래스들
class Dog implements Soundable {
    @Override
    public void makeSound() {
        System.out.println("멍멍!");
    }
}

class Cat implements Soundable {
    @Override
    public void makeSound() {
        System.out.println("야옹~");
    }
}

class Fish implements Swimmable {
    @Override
    public void swim() {
        System.out.println("어푸어푸~");
    }
}
```

이제 **`Soundable`** 인터페이스는 소리를 내는 기능에만 집중하고, **`Swimmable`** 인터페이스는 수영하는 기능에만 집중한다. 

### DIP (의존성 역전 원칙 - Dependency Inversion Principle)

---

**상위 수준의 모듈은 하위 수준의 모듈에 의존하면 안 되며, 둘 모두 추상화된 인터페이스에 의존해야 한다**. 즉, 추상화된 인터페이스를 통해 의존 관계를 맺어야 한다. 이를 통해 모듈 간의 결합도를 낮추고 유연한 구조를 만들 수 있다.

또한, 추상화는 세부사항에 의존해서는 안 된다. 세부 사항은 추상화에 따라 달라져야 한다.

아래 예시는 전구와 스위치가 각각 하위 수준의 모듈이고, 그들을 제어하는 '전구 컨트롤러'를 상위 수준의 모듈로 작성한 코드다.

```java
public interface Bulb {
    void turnOn();
    void turnOff();
}
```

```java
public class SimpleBulb implements Bulb {
    @Override
    public void turnOn() {
        System.out.println("전구가 켜졌습니다.");
    }

    @Override
    public void turnOff() {
        System.out.println("전구가 꺼졌습니다.");
    }
}
```

```java
public interface Switch {
    void press();
}
```

```java
public class SimpleSwitch implements Switch {
    private Bulb bulb;

    public SimpleSwitch(Bulb bulb) {
        this.bulb = bulb;
    }

    @Override
    public void press() {
        if (bulb != null) {
            if (isBulbOn()) {
                bulb.turnOff();
            } else {
                bulb.turnOn();
            }
        }
    }

    private boolean isBulbOn() {
        // 이 메서드는 현재 전구의 상태를 확인하는 로직이라고 가정한다.
        // 실제로는 전구 객체의 상태를 확인해야 한다.
        return false;
    }
}
```

```java
public class BulbController {
    private Switch bulbSwitch;

    public BulbController(Switch bulbSwitch) {
        this.bulbSwitch = bulbSwitch;
    }

    public void pressSwitch() {
        bulbSwitch.press();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Bulb bulb = new SimpleBulb();
        Switch bulbSwitch = new SimpleSwitch(bulb);
        BulbController bulbController = new BulbController(bulbSwitch);

        // 스위치를 누름으로써 전구를 제어
        bulbController.pressSwitch();
    }
}
```

전구와 스위치를 다루는 클래스 간에 강한 의존성이 없이도, '전구 컨트롤러'를 통해 전구를 제어하고 있다. 상위 수준의 모듈인 '전구 컨트롤러'는 하위 수준의 모듈인 전구와 스위치에 의존하지 않고, 추상화된 인터페이스를 통해 느슨한 결합을 유지한다.

# 의존성주입(DI, Dependency Injection)과 의존관계역전원칙(Dependency Inversion Principle)

### 의존성주입(DI, Dependency Injection)

---

객체 간의 의존 관계를 느슨하게 만들기 위해 사용하는 디자인 패턴

객체가 직접 자신이 필요로 하는 의존성을 생성하거나 관리하지 않고, 외부로부터 주입받는다.

메인 모듈이 직접 다른 하위 모듈에 대한 의존성을 주는 대신, 중간에 의존성 주입자(dependency injector)가 이 부분을 가로챔으로써, 메인 모듈에서 하위 모듈들로의 의존성을 간접적으로 주입하게 하는 방식이다.

![inflearn - CS 지식의 정석 중](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled.png)

inflearn - CS 지식의 정석 중

이를 통해 메인 모듈과 하위 모듈간의 의존성이 느슨해지며 특정 하위 모듈을 쉽게 교체하거나 추가/삭제할 수 있게 된다.

![inflearn - CS 지식의 정석 중
DI가 적용되지 않음](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%201.png)

inflearn - CS 지식의 정석 중
DI가 적용되지 않음

![DI가 적용됨. 화살표가 역전된 것을 볼 수 있다.](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%202.png)

DI가 적용됨. 화살표가 역전된 것을 볼 수 있다.

DI를 하게 되면 의존관계역전원칙(Dependency Inversion Principle)이 적용된다.

### 의존관계역전원칙(DIP: Dependency Inversion Principle)

---

SOLID 원칙 중 하나

1. 상위 모듈은 하위 모듈에 의존해서는 안 된다. 둘 다 추상화에 의존해야 한다.
2. 추상화는 세부사항에 의존해서는 안 된다. 세부 사항은 추상화에 따라 달라져야 한다.

### 의존성 주입의 장점

---

- 새로운 모듈을 쉽게 추가/교체할 수 있는 구조가 된다.
- 모듈별로 단위 테스트가 용이해진다.

### 의존성 주입의 단점

---

- 새로운 모듈을 하나 더 두게 되는 것이기 때문에 코드 복잡도가 증가
- 종속성 주입 자체가 컴파일을 할 때가 아닌 런타임 때 일어나기 때문에 컴파일을 할 때 종속성 주입에 관한 에러를 잡기가 어려워질 수 있다.
    
    일반적으로 자바 코드는 컴파일 단계에서 문법적인 오류를 확인하고, 이러한 오류를 수정하여 빌드를 수행한다. 그러나 의존성 주입 패턴을 사용하는 경우 의존성이 런타임 때에 주입되기 때문에, 컴파일러는 이러한 의존성을 정적으로 파악하기 어렵다. 즉, 코드를 컴파일하는 시점에서는 의존성이 실제로 주입되지 않기 때문에, 해당 의존성이 유효한지 여부를 확인하는 것이 어려워진다. 이로 인해 다음과 같은 상황들이 발생할 수 있다.
    
    - 런타임 오류: 의존성이 런타임에 주입되기 때문에, 프로그램이 실행되는 도중에 해당 의존성이 올바르게 주입되지 않아 발생하는 오류를 컴파일 단계에서 사전에 확인할 수 없다. 따라서 프로그램이 실행 중에 런타임 오류가 발생할 수 있다.
    - 타입 불일치: 주입되는 의존성의 타입이 실제 필요한 타입과 일치하지 않는 경우가 있을 수 있다. 이러한 경우 런타임에서 타입 불일치 오류가 발생하며, 컴파일러는 이를 사전에 확인할 수 없다.
    - 주입되는 의존성의 누락: 컴파일 시점에서는 주입되는 의존성이 정확하게 어떤 객체인지 확인할 수 없으므로, 런타임에 해당 의존성이 주입되지 않는 상황이 발생할 수 있다.
    
    이러한 단점들을 극복하려면, 주로 다음과 같은 방법들을 활용한다.
    
    - 테스트 코드 작성: 주입되는 의존성에 대한 테스트 코드를 작성하여 런타임 오류를 최대한 사전에 발견할 수 있도록 한다.
    - 의존성 주입 프레임워크 사용: 의존성 주입을 위한 프레임워크를 사용함으로써 컴파일 시점에서 의존성을 확인하고 주입하는 작업을 보다 쉽게 처리할 수 있다.
    - 주입되는 의존성에 대한 문서화: 코드에 주입되는 의존성에 대한 문서화를 철저히 하여 해당 의존성의 사용 방법과 제약사항을 개발자들에게 명확하게 전달한다.
    - 정적 분석 도구 사용: 정적 분석 도구를 활용하여 코드를 검사하고, 잠재적인 문제를 사전에 찾아내도록 한다.
    
    의존성 주입 패턴은 느슨한 결합과 테스트 용이성 등의 장점이 있지만, 이와 같은 런타임 시점의 단점을 고려하여 적절한 대응 방법을 선택하여 사용해야 한다.
    

# 싱글톤(Singleton)

Singleton = "single" + "-ton"

- "Single": "단일한" 또는 "하나뿐인"
- "-ton": 어미 "-ton"은 "town"이라는 뜻. 도시나 마을의 이름을 형성하는 데에 사용된다.
- [생성패턴(Creational Patterns)](https://www.notion.so/33c4c98ea0d44e6ba86f4154b900385e?pvs=21)의 일종
- **하나의 클래스에 오직 하나의 인스턴스만 가진다.** → 싱글톤 패턴은 인스턴스 생성 비용을 낮춘다. → API 요청을 줄일 수 있다.
- **공유 자원에 대한 중복 생성 방지, 설정 정보**와 같은 데이터에 **전역적인 접근**을 제공하는 데에 사용된다.
- **하나의 인스턴스를 다른 모듈들이 공유한다.** → 싱글톤 패턴은 객체의 의존성을 높인다. == 싱글톤 패턴은 모듈 간의 결합을 강하게 만든다.
    - 특히, TDD(Test Driven Development)를 할 수 없게 만든다.
        - 싱글톤 때문에 단위 테스트를 할 수 없기 때문
            - **단위 테스트는 서로 독립적이어야 한다.** → 싱글톤은 독립적인 테스트 인스턴스를 만들 수 없게 한다.
            - **단위 테스트는 각 테스트의 순서가 중요하지 않다.** 즉, 어떤 순서로든 실행할 수 있어야 한다.
    - 모듈 간의 결합을 낮추기 위해 의존성 주입(DI, Dependency Injection)을 이용할 수 있다.
        - **의존성==종속성**: “A가 B에 의존한다.” == “A가 B를 사용한다.” == “B가 바뀌면 A도 바뀌어야 한다.”
        - 의존성 주입의 장점: 모듈들을 쉽게 교체할 수 있는 구조가 된다.
        - 의존성 주입의 단점: 모듈들이 많이 분리되므로 클래스 수가 늘어나고 복잡성이 증가한다.
        - 의존성 주입의 원칙
            - 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
            - 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.
            - 추상화는 세부 사항에 의존하지 말아야 한다.
- **I/O 바운드(디스크 연결, 네트워크 통신, 데이터베이스) 작업 등 인스턴스 생성 비용이 많은 작업에 주로 사용**된다.
- 설정값을 모든 어플리케이션에서 유지하고자 하는 상황 등에서도 사용된다. → 여러 클래스를 만들 필요 없이 싱글톤 인스턴스만을 가져오게 하면 되기 때문에, 코드의 복잡도가 감소하고 유지보수성은 증가하며, 자원의 낭비를 막을 수 있다.

<aside>
💡 **I/O 바운드란?**

### I/O 바운드

---

I/O 바운드는 **컴퓨팅에서 성능을 제한하는 주요 요인 중 하나**로, **Input/Output Bound**의 줄임말이다. 

입력과 출력 작업은 하드 디스크, 네트워크, 외부 장치와의 **데이터 교환 등과 관련된 작업**을 의미한다. 이러한 작업은 CPU가 직접 처리하는 메모리 내의 연산과는 달리, 비교적 많은 시간이 소요된다. 예를 들어, 파일을 디스크에 저장하거나 읽는 작업, 네트워크를 통해 데이터를 전송하는 작업 등이 I/O 작업에 해당한다.

I/O 바운드 상태는 주로 다음과 같은 경우에 발생한다:

1. **대용량 파일의 입출력**
2. **네트워크 통신**
3. **데이터베이스 액세스**

**I/O 바운드 상태에서는 CPU가 대기하며 I/O 작업이 완료될 때까지 기다려야 한다**. 이런 경우, 성능을 향상시키기 위해 비동기 I/O, 스레드 풀, 캐싱 등과 같은 기술을 사용하여 I/O 작업을 최적화하는 것이 중요하다.

반대로 **CPU 바운드는 입력과 출력 작업보다는 CPU 연산에 의해 제한되는 상태를 의미**한다. 이 경우에는 CPU의 성능을 최적화하는 것이 중요하다. 따라서 **성능 향상을 위해서는 시스템이 I/O 바운드인지, CPU 바운드인지를 파악하고 적절한 최적화 방법을 적용하는 것이 필요**하다.

</aside>

## 싱글톤이 아닌 케이스

```jsx
class Rectangle {
	constructor(height, width) {
		this.height = height;
		this.width = width;
	}
}
const a = new Rectangle(1, 2)
const b = new Rectangle(1, 2)
console.log(a === b) **// false**
```

## 싱글톤인 케이스

```jsx
public class Singleton {
    private static Singleton instance;

    private Singleton() { 
			// new 키워드로 새로운 인스턴스를 생성할 수 없게 막는다.
			// 이렇게 함으로써 여러 인스턴스의 생성을 막을 수 있고, 
			// 자체 생성한 단일 인스턴스만을 강제할 수 있다.
      // Private constructor to prevent instantiation from outside the class
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## 싱글톤은 SOLID 원칙 중 "단일 책임 원칙"과 "의존성 역전 원칙"을 위반한다.

싱글톤에 의해 다른 객체들이 해당 싱글톤 객체에 의존함으로써 객체간의 결합도가 높아지기 때문에 단일 책임 원칙(SRP)의 위반, 상위 수준의 모듈은 하위 수준의 모듈인 싱글톤에 의존하게 되므로 의존성 역전 원칙(DIP)의 위반이다.

**따라서 싱글톤은 필요한 경우에만 사용해야**하고, **가능한 한 다른 디자인 패턴을 사용하거나, 의존성 주입(DI) 등을 활용하여 SOLID 원칙을 지키도록 설계하는 것이 좋다**.

## 싱글톤 패턴을 구현하는 방법

### 기초

---

```java
public class Company {
    // 이대로두면 외부에서 Company 인스턴스를 여러 개 만들 수 있다.
    // 특히 디폴트 생성자에 의해.
    // 하지만 Company는 전 세계에서 유일해야 한다.
    **private** Company() {} // 외부에서 생성하지 못하게 디폴트 생성자를 닫는다.

    **private** **static** Company instance = new Company(); // 외부에서 쓸 유일한 Company 객체를 직접 만들어준다.

    // 이제 외부에서 이 유일한 객체에 접근할 방법을 제공해줘야 한다.
    **public static** Company getInstance() {
        // 외부에서는 이 클래스의 객체를 직접 만들 수 없다.
        // 따라서 클래스 이름으로 getInstance 메서드를 호출 할 수 있도록 static 메서드로 만들어줘야 한다.
        if (instance == null) { // 방어적인 태도를 취하여, 객체가 없다면 만들도록 한다.
            instance = new Company();
        }
        return instance;
    }
}
```

```java
import java.util.Calendar;

public class Test {
    public static void main(String[] args){
        Company companyA = Company.getInstance();
        System.out.println("companyA = " + companyA);
        Company companyB = Company.getInstance();
        System.out.println("companyB = " + companyB);

//        Calendar cal = new Calendar(); // NO
        Calendar cal = Calendar.getInstance(); // OK
    }
}
// companyA = FCJava.ch18.Company@**30f39991**
// companyB = FCJava.ch18.Company@**30f39991
// 싱글톤에 의해 유일한 객체가 생성되었기 때문에, 객체의 힙 메모리 가상 주소가 동일하다.**
```

![[https://gitlab.com/easyspubjava/javacoursework/-/blob/master/Chapter2/2-18/README.md](https://gitlab.com/easyspubjava/javacoursework/-/blob/master/Chapter2/2-18/README.md)](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%203.png)

[https://gitlab.com/easyspubjava/javacoursework/-/blob/master/Chapter2/2-18/README.md](https://gitlab.com/easyspubjava/javacoursework/-/blob/master/Chapter2/2-18/README.md)

### **synchronized 방법**

---

최초 접근한 쓰레드가 싱글톤 패턴 생성 여부를 확인하여, 싱글톤이 없으면 새로 만들고, 인스턴스가 이미 있으면 해당 인스턴스를 반환한다. 

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() { // getInstance가 호출됐을 때 싱글톤 인스턴스를 생성하든 반환하든 한다.
        if(instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

하지만, 이 방식은 **메서드의 원자성이 결여되어 있다**.

<aside>
💡 **메서드의 원자성?**

메서드의 원자성(Atomicity)이란, 해당 메서드가 한 번에 실행되는 단일 불변의 작업 단위를 의미한다. 즉, **해당 메서드의 모든 단계가 한꺼번에 실행되거나 전혀 실행되지 않는 것을 보장하는 것을 말한다**. 

위의 코드에서는 Singleton 디자인 패턴을 사용하여 클래스 내부에서 단 하나의 인스턴스만 생성하도록 구현되어 있다. 그러나 이 코드에는 메서드의 원자성이 결여되어 있다. 

일단, Java는 multi-thread 언어다. 그렇다면 아래와 같은 질문이 나올 수 있다.

> *“쓰레드가 여러 개 일 땐 어떤 일이 벌어지지?”*
> 

만약 여러 개의 스레드가 거의 동시에 getInstance() 메서드를 호출하면, 각각의 스레드의 입장에서는 instance가 null이고, 이에 따라 각자 인스턴스를 생성한다. 이로 인해 여러 개의 Singleton 인스턴스가 생성되어, 싱글톤의 의미가 사라지게 된다.

메서드의 원자성을 보장하려면, getInstance() 메서드를 동기화(synchronized)하는 방법을 사용할 수 있다. 동기화를 통해 여러 스레드가 동시에 getInstance() 메서드를 호출하는 것을 막을 수 있으며, 순차적으로 하나의 스레드만이 인스턴스를 생성하도록 보장할 수 있다.

동기화를 적용한 Singleton 클래스:

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    public static **synchronized** Singleton getInstance() { // 동기화 추가
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

```java
package kr.fc.java;

import static java.lang.Math.*;

public class Singleton_thread {
    private static String name = "dohk";

    public static void main(String[] args){
        Singleton_thread t = new Singleton_thread();
        new Thread(() -> {
            for(int i = 0; i<10; i++) {
                t.say("Im dohk");
            }
        }).start();

        new Thread(() -> {
            for (int i = 0; i<10; i++){
                t.say("XXXXXXXXXX");
            }
        }).start();
    }

    public void say(String word) { // synchronized 키워드 삽입 시, thread-safe.
        name = word;

        try {
            long sleep = (long) (random() * 100);
            Thread.sleep(sleep);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        if (!name.equals(word)) {
            System.out.println("word = " + name); // if console output is displayed, then this program is not thread-safe.
        }
    }
}
```

</aside>

<aside>
💡 **synchronized?**
synchronized 키워드는 인스턴스를 반환하기 전까지 인스턴스를 별도의 격리 공간에 넣어놓는 역할을 한다. **최초로 접근한 스레드가 해당 메서드 호출 시에 다른 스레드가 접근하지 못하도록 잠가놓는(lock하는) 효과가 있다**.
**하지만, 쓰레드들이 해당 메서드를 호출할 때마다 lock이 걸리기 때문에 성능이 떨어진다**.

</aside>

→ DCL 방식으로 해결

### static

---

static은 객체가 얼마나 만들어지든, 메모리의 지정된 공간에 딱 하나만 존재하게 해준다. 컴파일 시점에 static에 의해 선언된 객체가 차지할 용량이 처음부터 결정된다. → 즉, 런타임에 동적으로 생성되지 않음

static은 프로그램 실행 시 미리 인스턴스를 생성한다. static은 클래스 로딩과 동시에 만들어지기 때문에, 이 방식을 이용하여 미리 싱글톤 인스턴스를 만드는 방식이다. 따라서, 매 호출때마다 미리 만들어진 인스턴스를 반환한다.

이 방식의 문제점은 **싱글턴 인스턴스가 필요없는 경우에도 무조건 싱글톤 클래스를 호출**해서 인스턴스를 만들어야 하기 때문에 **자원이 낭비된다**는 점이다. 

```java
package kr.fc.java;

public class Singleton {
    private **final** **static** Singleton instance **= new Singleton()**;
		// getInstance를 통해서 인스턴스를 만드는 1번 방식이 아닌, 처음부터 만드는 방식이다.
		// 무조건 생성 방식

    private Singleton() {

    }

    public static Singleton getInstance() { // getInstance가 호출됐을 때 싱글톤 인스턴스를 생성하든 반환하든 한다.
        return instance;
    }

    public static void main(String[] args){
        // 싱글톤 인스턴스 얻기
        Singleton singletonInstance = Singleton.getInstance();

        singletonInstance.getInstance();
        // 인스턴스를 사용하여 원하는 작업을 수행
        System.out.println("싱글톤 인스턴스를 사용하는 예시");
        // 여기에 원하는 작업을 추가
    }
}
```

```java
public class Singleton {
    private **static** Singleton instance = null;

    **static {
        instance = new Singleton();
    }**

    private Singleton() {

    }

    public static Singleton getInstance() { // getInstance가 호출됐을 때 싱글톤 인스턴스를 생성하든 반환하든 한다.
        return instance;
    }

    public static void main(String[] args){
        // 싱글톤 인스턴스 얻기
        Singleton singletonInstance = Singleton.getInstance();

        singletonInstance.getInstance();
        // 인스턴스를 사용하여 원하는 작업을 수행
        System.out.println("싱글톤 인스턴스를 사용하는 예시");
        // 여기에 원하는 작업을 추가
    }
}
```

→ holder 클래스 방식으로 해결

<aside>
💡 `final` 키워드

---

Java에서 **`final`**은 변수, 메서드, 클래스에 적용할 수 있는 키워드다. **`final`** 키워드는 사용되는 위치에 따라 다양한 의미를 가진다:

- `**final` 변수**: **`final`** 키워드가 변수에 적용되면, **해당 변수는 상수(Constant)가 된다**. 이는 변수에 할당된 **값을 변경할 수 없음**을 의미한다.

```java
final int MAX_VALUE = 100; // 상수 MAX_VALUE
```

- `**final` 메서드**: **`final`** 키워드가 메서드에 적용되면, **해당 메서드는 하위 클래스에서 오버라이딩(재정의)할 수 없다**. 즉, **`final`** 메서드는 **상속된 메서드를 변경하거나 재정의하는 것을 방지**한다.

```java
public class Parent {
    public final void printMessage() {
        System.out.println("부모 클래스의 메시지");
    }
}

public class Child extends Parent {
    // 오류: **부모 클래스의 final 메서드를 오버라이딩할 수 없음**
    // public void printMessage() {
    //     System.out.println("자식 클래스의 메시지");
    // }
}
```

- `**final` 클래스**: **`final`** 키워드가 클래스에 적용되면, 해당 클래스는 상속될 수 없다. 즉, **다른 클래스에서 이 `final` 클래스를 상속받는 것을 금지**한다.

```java
final class FinalClass {
    // FinalClass의 멤버들
}

// 오류: **final 클래스를 상속받을 수 없음**
// class SubClass extends FinalClass {
//     // SubClass의 멤버들
// }

```

**`final`** 키워드의 주요 목적은 **변경을 허용하지 않는 요소들을 명시적으로 표시하여 코드의 안정성과 가독성을 높이는** 데에 있다. 상수, 보안, 상속 제한 등 다양한 상황에서 **`final`** 키워드가 유용하게 사용될 수 있다.

</aside>

### 중첩 클래스(Lazy holder)와 정적 멤버: 추천

---

<aside>
💡 Holder 클래스

---

**클래스 안에 클래스를 정의하는 것을 "중첩 클래스(Nested Class)"라고 한다**. 중첩 클래스에는 여러 종류가 있으며, 그 중 하나가 **"정적 중첩 클래스(Static Nested Class)"**다. 정적 중첩 클래스는 보통 **"Holder 클래스"**로 불리기도 한다.

Holder 클래스는 바깥쪽 클래스의 인스턴스에 종속되지 않고, 바깥쪽 클래스의 정적 멤버처럼 동작한다. 따라서 **Holder 클래스는 바깥쪽 클래스의 인스턴스 생성 없이도 사용할 수 있다**.

```java
public class OuterClass {
    // 바깥쪽 클래스의 멤버들

    // 정적 중첩 클래스 (Holder 클래스)
    public static class InnerClass {
        // InnerClass의 멤버들
    }
}
```

Holder 클래스의 주요 장점은 바깥쪽 클래스의 **인스턴스에 종속되지 않아서 단독으로 사용할 수 있으며**, 바깥쪽 클래스의 비공개(private) 멤버에도 접근할 수 있다는 점이다. Holder 클래스를 사용하여 코드를 더 모듈화하고 가독성을 높일 수 있다.

</aside>

싱글톤 클래스가 최초에 로딩되더라도 함께 초기화되지 않도록 내부 클래스를 하나 더 만들고, getInstance()가 호출될 때 내부 클래스가 로딩되어 인스턴스를 생성하게 하는 방식이다. 

따라서, 필요할 때만 자원이 할당된다.

```java
package kr.fc.java;

public class Singleton {
    private **static** class singleInstanceHolder { // Holder class
        private static final Singleton INSTANCE = new Singleton();
    }

    public **static** Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}
```

<aside>
💡 **`static`**과 **`final`** 키워드의 순서

private `static` `final` Singleton INSTANCE = new Singleton();
private `final` `static` Singleton INSTANCE = new Singleton();

**`static`**과 **`final`** 키워드의 순서가 바뀌어 있다. 이는 Java에서 **`static final`**이든 **`final static`**이든 동일한 의미를 가지므로 **기능적으로 동일**하다.

</aside>

```java
class Singleton {
    private static class singleInstanceHolder {
        private static **final** Singleton **INSTANCE** = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld{ 
     public static void main(String []args){ 
        Singleton a = Singleton.getInstance(); 
        Singleton b = Singleton.getInstance(); 
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());  
        if (a == b){
         System.out.println(true); 
        } 
     }
}
/*
705927765
705927765
true

1. 클래스안에 클래스(Holder), static이며 중첩된 클래스인 singleInstanceHolder를 
기반으로 객체를 선언했기 때문에 **한 번만 로드**되므로 싱글톤 클래스의 인스턴스는 애플리케이션 당 하나만 존재하며 
클래스가 두 번 로드되지 않기 때문에 두 스레드가 동일한 JVM에서 2개의 인스턴스를 생성할 수 없다. 
그렇기 때문에 동기화, 즉 synchronized를 신경쓰지 않아도 된다. 
2. final 키워드를 통해서 read only 즉, 다시 값이 할당되지 않도록 하고 있다.
3. 중첩클래스로 만들었기 때문에 싱글톤 클래스가 로드될 때 클래스가 메모리에 로드되지 않고 
어떠한 모듈에서 getInstance()메서드가 호출될 때 싱글톤 객체를 최초로 생성 및 리턴하게 된다. 
*/
```

이 코드는 **"Holder" 방식(클래스 안의 클래스를 만드는 방식)**의 싱글톤 구현 방법을 사용한다. Holder 방식은 내부 클래스를 사용하여 인스턴스를 생성하는 방법으로서, **스레드 안정성(***static으로 싱글톤을 구현할 경우, 싱글턴 인스턴스가 필요없는 경우에도 무조건 싱글톤 클래스를 호출해서 인스턴스를 만들어야 하기 때문에 자원이 낭비된다. holder 방식은 이를 방지한다.***)과 지연 초기화(***자신을 instance로 갖기 위해 new 키워드를 사용하는데, 이 이전까지는 초기화가 이루어지지 않는다.***)를 보장**한다. 

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // private 생성자
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

1. `static`으로 자기 자신을 인스턴스로 갖는 멤버변수를 만든다.
2. 생성자를 외부에서 생성하지 못하게 `private`으로 막는다.
3. `getInstance` 메서드로만 접근할 수 있도록 한다.
    - `static` 메서드
    - 인스턴스가 없으면 만들어준다.

`getInstance` 메서드는 항상 동일한 인스턴스를 반환하는 것이 보장된다.

### 이중 확인 잠금(DCL: Double Checked Locking)

---

**인스턴스 생성 여부를 싱글톤 패턴 잠금 전에 한 번, 객체 생성 전에 한 번, 이렇게 총 2번 체크**하여, **인스턴스가 존재하지 않을 경우에만 잠금**을 걸게한다.

```java
package kr.fc.java;

public class Singleton {
    private **volatile** Singleton instance;

    private Singleton() {

    }

    public Singleton getInstance() {
        if (instance == null) { // check 1
            **synchronized (Singleton.class)** { 
						// instance가 null일 때, synchronized로 lock한다.
                if (instance == null) { // check 2
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

<aside>
💡 **volatile?**
volatile을 알려면, 메모리 구조에 대해 알아야 한다.
메모리 구조는 다음과 같다.

![Untitled](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%204.png)

CPU 캐시 메모리인 L3, L2, L1이 있다. Java에서는 Thread 2개가 열리면 변수를 메인 메모리인 RAM에서 가져오는 것이 아니라, 캐시 메모리에서 가져온다. 즉 두 Thread 간에 데이터가 공유되지 않는다. 만약 메인메모리를 기반으로 데이터를 저장하고 읽어온다면? **Thread 끼리 데이터 동기화**가 이루어질 것이다. volatile은 이걸 가능하게 한다.

![Untitled](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%205.png)

</aside>

### enum

enum은 기본적으로 thread safe를 보장한다.

# **Factory(팩토리) 패턴**

- 상위 클래스에서는 기본적인 뼈대. 인스턴스 생성은 하위 클래스를 생성
- [생성패턴(Creational Patterns)](https://www.notion.so/33c4c98ea0d44e6ba86f4154b900385e?pvs=21)의 일종
- 상위 클래스와 하위 클래스로 분리하여, 하위 클래스에서는 객체의 생성을, 상위 클래스에서는 생성된 객체의 사용을 담당한다.
- 상위 클래스와 하위 클래스로 분리되기 때문에 결합이 느슨해진다. → 느슨한 결합 → 코드 간 결합도(의존성)을 낮추기 때문에 유지보수성이 증가한다.
- 상위 클래스는 하위 클래스가 어떻게 인스턴스를 생성하는지 알 필요가 전혀 없다. → 유연성 증가
- 객체 생성 로직이 하위 클래스에 있기 때문에 로직의 수정은 한 곳에서만 하면 된다. → 유지 보수성 증가

```python
from abc import ABC, abstractmethod

# 추상 클래스 정의
class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass

# 구체적인 클래스 정의
class Dog(Animal):
    def speak(self):
        return "멍멍!"

class Cat(Animal):
    def speak(self):
        return "야옹!"

# 팩토리 클래스 정의
class AnimalFactory:
    def get_animal(self, animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        else:
            raise ValueError("유효한 동물 타입이 아닙니다.")

# 클라이언트 코드
factory = AnimalFactory()

dog = factory.get_animal("dog")
print(dog.speak())  # 출력: 멍멍!

cat = factory.get_animal("cat")
print(cat.speak())  # 출력: 야옹!

```

# **프록시(Proxy) 패턴**

<aside>
💡 **프록시?**
대리인/비서
무언가를 대신 처리하는 녀석

</aside>

<aside>
💡 **프록시 객체**
어떤 대상의 기본적인 동작을 가로채어 대신 처리할 수 있는 객체

</aside>

[구조 패턴(structural patterns)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21)의 일종

- 다른 무언가와 이어지는 인터페이스 역할을 하는 클래스
- 다른 객체를 대신하여 제어하는 대리자(Proxy)를 사용하여 접근을 제어하는 패턴
- 어떤 객체를 사용하고자 할 때 해당 객체를 직접 참조하는 것이 아닌, 해당 객체를 proxy하는 proxy객체를 통해서 대상 객체에 접근하는 것
    - 해당 객체가 실제로 메모리에 존재하지 않더라도 참조할 수 있다.
    - 실제 해당 객체의 기능이 반드시 필요한 시점까지 해당 객체의 생성을 미룰 수 있다.
- 실제 객체에 대한 접근을 중간에 가로채서, 필요한 추가 작업을 수행하거나 접근 제한, 필터링, 수정 등의 작업을 할 수 있다.
- 데이터 검증, 캐싱, 로깅, 원격 객체 접근, 지연 로딩, 보안 등에 사용된다.
- [개방 폐쇄 원칙(OCP)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21) 준수: 기존 코드를 변경하지 않고도 새로운 기능을 추가할 수 있다.
- [단일 책임 원칙(SRP)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21) 준수: 기존 코드가 해야 하는 일만 유지할 수 있다.
- 프론트엔드 작업 시 CORS 에러를 해결하는 방법
    - 프론트엔드는 127.0.0.1:3000으로 테스팅
    - 백엔드 서버는 127.0.0.1:12010
    
    → 포트 번호가 서로 다르기 때문에 CORS 에러 발생 → 프록시 서버로 프론트엔드 서버에서 요청되는 오리진을 127.0.0.1:12010으로 바꿔줌
    
- 단, 성능이 저하될 수 있다.
    - 객체를 생성할 때 한 단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우
    - 객체 생성을 위해 스레드 생성 및 동기화가 구현되어야 하는 경우

```python
from abc import ABC, abstractmethod

# Subject interface
class ImageSubject(ABC):
    @abstractmethod
    def display(self):
        pass

# Real Subject
class RealImage(ImageSubject):
    def __init__(self, filename):
        self._filename = filename
        self.load_image()

    def load_image(self):
        print(f"Loading image: {self._filename}")

    def display(self):
        print(f"Displaying image: {self._filename}")

# Proxy
class ImageProxy(ImageSubject):
    def __init__(self, filename):
        self._filename = filename
        self._real_image = None

    def display(self):
        if self._real_image is None:
            self._real_image = RealImage(self._filename)
        self._real_image.display()

# Client code
if __name__ == "__main__":
    # Create proxy objects
    image1 = ImageProxy("cat.jpg")
    image2 = ImageProxy("dog.jpg")

    # Display images
    image1.display()  # RealImage is loaded and displayed
    image2.display()  # RealImage is loaded and displayed

    # 이미지는 이미 로드되었으므로 RealImage가 다시 로드되지 않음
    image1.display()
```

```
Loading image: cat.jpg
Displaying image: cat.jpg
Loading image: dog.jpg
Displaying image: dog.jpg
Displaying image: cat.jpg
```

위 코드에서는 **`ImageSubject`**라는 추상 클래스를 정의하고, 이를 상속하는 **`RealImage`** 클래스를 만든다. 이 클래스는 실제 이미지를 로드하고 표시하는 역할을 한다. 그리고 **`ImageProxy`** 클래스는 **`ImageSubject`** 인터페이스를 구현하며, **`RealImage`** 객체에 대한 대리자 역할을 한다. 이를 통해 이미지가 실제로 필요할 때만 로드되도록 하여 자원을 절약할 수 있다.

<aside>
💡 **프록시 서버**
클라이언트와 인터넷 사이에서 
서버와 클라이언트 사이에서 중계 역할을 하는 컴퓨터나 애플리케이션으로서, 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해준다.
캐시 내에 데이터를 두고, 요청이 들어오면 해당 요청에 대해 다시 저 멀리 있는 원격 서버에 요청하지 않고 캐시 안에 있는 데이터를 활용한다.
→ 불필요한 외부와의 연결을 하지 않으므로 트래픽을 줄일 수 있다.

</aside>

프록시의 또 다른 예는 ‘군대’가 될 수 있다. 군대는 편지를 검열 할 수 있는데, 이는 정치적 성향의 편지를 받을 수게 하기 위함이다. 따라서, 편지가 중간에 가로채지고 검열된 후 전달된다.

또한, VPN도 프록시의 일종이다. 클라이언트는 프록시 서버인 VPN을 통해 인터넷에 연결하고, VPN(프록시 서버)은 클라이언트의 요청을 받아서 해당 요청을 대신 처리하거나 인터넷에 접근한다. 이렇게 함으로써 클라이언트의 IP 주소를 가리거나 인터넷 접속을 안전하게 하는 등 트래픽을 제어할 수 있다. VPN은 이러한 프록시 서버의 역할에 더해, 클라이언트와 VPN 서버 간에 암호화된 터널을 만든다. 이를 통해 클라이언트의 인터넷 트래픽을 암호화하고 보호한다. 

## 장점

- 실제 객체로 접근하기 전에 전처리, 캐싱 등의 작업이 가능하다.
→ 기존 객체의 리소스가 무거울 경우, 캐싱 과정을 통해 부하를 줄일 수 있다.
- 객체와 클라이언트의 요청 사이에 위치하기 때문에 방패(보안) 역할을 한다.

## 단점

- 코드의 복잡도가 증가한다.
- 로직이 난해해져 가독성이 떨어진다.
- 성능이 저하될 수 있다: 빈번한 객체 생성이 필요한 경우, 객체 생성을 위해 스레드 생성 및 동기화가 구현되어야 하는 경우
→ 객체를 생성할 때 한 단계를 더 거치기 때문

# **데코레이터(Decorator) 패턴**

- 객체에 추가적인 기능을 동적으로 더할 수 있도록 하는 것. 상속을 사용하지 않고서도 객체를 확장시킬 수 있는 방법이다.

# **전략(Strategy) 패턴**

[행동 패턴(behavioral pattern)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21)의 일종

- == 정책(policy) 패턴
- 객체의 행위를 직접 수정하지 않은 채, 캡슐화된 알고리즘을 컨텐스트 안에서 바꿔가면서 여러 전략 중 원하는 전략을 동적으로 선택/교체할 수 있게 한다. 즉, 실행 중에 알고리즘을 변경할 수 있다.
    
    > *"상황에 따라 유연하게 다른 알고리즘을 쓰고 싶은데, 어떻게 하면 될까?”*
    > 
    - ex) 결제 방식 ⇒ {네이버페이, 카카오페이}
    - ex) 로그인 방식 ⇒ {자체 회원가입 회원의 로그인, 구글 로그인, 애플 로그인, 카카오 로그인}
    - ex) 검색 방식 ⇒ {전체 검색, 이미지 검색, 뉴스 검색, 지도 검색}
    
    <aside>
    💡 **컨텍스트**
    개발자가 어떤 작업을 완료하는 데에 필요한 모든 관련 정보
    
    </aside>
    
- 알고리즘을 캡슐화 했기 때문에 코드 재사용성이 높다.
- 알고리즘의 수정을 용이하게 할 수 있다.

```python
from abc import ABC, abstractmethod

# 전략을 나타내는 추상 클래스
class Strategy(ABC):
    @abstractmethod
    def execute(self, data):
        pass

# 구체적인 전략 클래스들
class ConcreteStrategyAdd(Strategy):
    def execute(self, data):
        return sum(data)

class ConcreteStrategyMultiply(Strategy):
    def execute(self, data):
        result = 1
        for num in data:
            result *= num
        return result

# 컨텍스트 클래스
class Context:
    def __init__(self, strategy):
        self._strategy = strategy

    def set_strategy(self, strategy):
        self._strategy = strategy

    def execute_strategy(self, data):
        return self._strategy.execute(data)

# 클라이언트 코드
if __name__ == "__main__":
    data = [1, 2, 3, 4, 5]

    # 전략 객체 생성
    add_strategy = ConcreteStrategyAdd()
    multiply_strategy = ConcreteStrategyMultiply()

    # 컨텍스트 객체 생성
    context = Context(add_strategy)

    # 컨텍스트에서 전략 실행
    result = context.execute_strategy(data)
    print("Sum:", result)

    # 전략 변경
    context.set_strategy(multiply_strategy)

    # 컨텍스트에서 새로운 전략 실행
    result = context.execute_strategy(data)
    print("Product:", result)

```

*병휘님 코드가 더 좋아보임*

## 전략 패턴이 적용되지 않은 케이스

![Untitled](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%206.png)

![Untitled](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%207.png)

새로운 검색 전략이 추가될 때마다 `onClick` 메서드를 수정해야 한다. → 유지보수성 감소

## 전략 패턴이 적용된 케이스

![Untitled](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%208.png)

![Untitled](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%209.png)

전략 패턴이 적용된 위 코드를 보면, onClick 메서드 내의 코드가 간단해졌고, SearchStrategy 인터페이스

# **옵저버(Observer) 패턴**

[행동 패턴(behavioral pattern)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21)의 일종

- 어떤 객체(subject)의 상태에 변화가 일어나면 관련된 다른 객체들(observers)에게 변화를 알려주거나, 자동으로 업데이트 되도록 하는 디자인 패턴
- 이벤트 발생 시, 대상자는 목록의 각 관찰자들에게 통지하고, 관찰자들은 알림을 받아 조치를 취한다.
- observer들은 subject의 변화에 따라 추가적인 변화 사항이 생기는 객체들이다.
- 관찰자라는 단어 뉘앙스 상, 관찰자가 능동적으로 대상을 관찰하는 것처럼 느껴지지만, 사실은 대상 객체로부터 수동적으로 전달 받기를 기다리고 있다.

> *"한 객체의 상태가 변경되면, 의존하는 다른 객체들에게 자동으로 알려줘야 하는데 어떻게 하면 될까?”*
> 
- ex) 트위터, 인스타그램 등에서 ‘팔로우’한 계정에 새로운 포스트가 올라오면 푸쉬 알림이 오는 것
- MVC(Model - View - Controller) 패턴에도 쓰인다.
    - MVC의 Model과 View의 관계는 Observer 패턴의 Subject 역할과 Observer 역할의 관계에 대응된다.
    - 하나의 Model에 복수의 View가 대응한다.
- 한개의 관찰 대상자(Subject)와 여러 개의 관찰자(Observer A, B, C)로서 **일 대 다 관계**로 구성

![https://stackabuse.com/observer-design-pattern-in-python](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled.jpeg)

https://stackabuse.com/observer-design-pattern-in-python

- Pub/Sub(발행/구독) 모델로도 알려져 있다.
- Subject의 상태 변경을 주기적으로 조회하지 않고 자동으로 감지할 수 있다.
- 구독자는 알림 순서를 제어할 수 없고, 무작위 순서로 알림을 받는다.
- 다수의 옵저버 객체를 등록한 이후에 별도로 해지하지 않는다면 메모리 누수가 발생할 수도 있다.

```python
# 옵저버(Observer) 클래스
class Observer:
    def update(self, message):
        pass

# 구독자(Subscriber) 클래스
class Subscriber(Observer):
    def __init__(self, name):
        self.name = name

    def update(self, message):
        print(f"{self.name} received message: {message}")

# 발행자(Publisher) 클래스
class Publisher:
    def __init__(self):
        self.observers = []

    def add_observer(self, observer):
        self.observers.append(observer)

    def remove_observer(self, observer):
        self.observers.remove(observer)

    def notify_observers(self, message):
        for observer in self.observers:
            observer.update(message)

# 메인 함수
if __name__ == "__main__":
    # 발행자 객체 생성
    publisher = Publisher()

    # 구독자 객체들 생성 및 발행자에 등록
    subscriber1 = Subscriber("Subscriber 1")
    subscriber2 = Subscriber("Subscriber 2")
    publisher.add_observer(subscriber1)
    publisher.add_observer(subscriber2)

    # 발행자가 메시지를 발행하면 구독자들에게 알림
    publisher.notify_observers("Hello, World!")
```

```
Subscriber 1 received message: Hello, World!
Subscriber 2 received message: Hello, World!
```

`Publisher` : 관찰 당하는 입장. 발행자/게시자

- 관찰자인 Observer들을 리스트(List, Map, Set ..등)로 모아 합성(compositoin)한 채로 가지고 있음 → `self.observers.append(observer)`
- 관찰자인 Observer들을 내부 리스트에 등록/삭제 하는 인프라를 갖는다. (`add_observer`, `remove_observer`)
- Subject가 상태를 변경하거나 어떤 동작을 실행할 때, Observer 들에게 이벤트 알림(`notify_observers`)을 발행한다.

`Observer` : 구독자들을 묶는 인터페이스 (다형성)

`Subscriber` : 관찰자/구독자

- Subscriber들은 Subject가 발행한 알림을 받고, 현재 상태 정보를 얻는다.
- Publisher로부터 통보를 받은 Observer는 값을 바꾸거나, 삭제하는 등의 작업을 수행한다.
- Observer들은 언제든 Subject의 그룹에서 추가/삭제 될 수 있다.

![[https://www.scaler.com/topics/design-patterns/observer-design-pattern/](https://www.scaler.com/topics/design-patterns/observer-design-pattern/)](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%2010.png)

[https://www.scaler.com/topics/design-patterns/observer-design-pattern/](https://www.scaler.com/topics/design-patterns/observer-design-pattern/)

## 사용

- 대상 객체의 상태가 변경될 때마다 트리거해야 할 때
- 한 객체의 상태가 변경되면 다른 객체도 변경해야 할 때

## 장점

- Subject의 상태 변경을 주기적으로 조회하지 않아도 자동으로 그 변화를 감지할 수 있다.
- 발행자의 코드를 변경하지 않은 채, 새 구독자를 도입할 수 있다. → [개방 폐쇄 원칙(OCP)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21) 준수
- Subscriber와 Publisher의 관계는 런타임 시점에 결정된다.
- 상태를 변경하는 객체(Subject)와 변경을 감지하는 객체(Observer)가 느슨한 결합을 맺는다. → 의존성이 낮아진다. → 유지보수가 편해진다.

## 단점

- Subscriber는 알림 순서를 제어할 수 없다. → Subscriber는 Publisher로부터 무작위적인 순서로 알림을 받는다.
- 코드 복잡도가 증가한다; 구조/동작을 파악하기 힘들기 때문
- 옵저버 객체를 여러 개 등록하고서 구독을 해지하지 않는다면 메모리 누수가 발생할 수 있다.

# **이터레이터(Iterator) 패턴**

[행동패턴(Behavioral Patterns)](https://www.notion.so/33c4c98ea0d44e6ba86f4154b900385e?pvs=21)의 일종

- 이터레이터(iterator)를 사용하여 일련의 데이터 집합에 대하여 순차적인 접근(순회)을 지원하는 패턴

<aside>
💡 **데이터 집합이란?**
그룹으로 묶인 자료구조. 대표적으로 리스트, 트리, 그래프, 테이블, 딕셔너리 등이 있다.

</aside>

- 리스트 같은 경우 처음부터 순서가 연속적인 데이터 집합이기 때문에 for문 등을 통해 순회할 수 있지만 해시, 트리, 딕셔너리, 맵 같은 자료구조는 데이터 저장 순서가 정해져 있지 않다. 이렇게 각기 다른 자료구조들을 똑같은 iterator 인터페이스로 쉽게 순회 할 수 있게 하는 것이 이터레이터 패턴이다. → 컬렉션 객체 안에 들어있는 모든 원소들에 대한 접근 방식을 공통화한다. == 어떤 종류의 컬렉션에서도 순회하기 위한 인터페이스가 통일된다.
- 객체지향의 특성 상, 객체 내부의 요소를 직접 접근할 수 없다. 이터레이터 패턴은 객체 내부의 요소에 직접 접근하는 대신 이터레이터를 사용하여 순차적으로 순회할 수 있게 해준다. → 객체의 내부 구조를 노출하지 않고도 순회 할 수 있게 한다.
- 컬렉션 객체의 내부 구조를 노출하지 않고, 컬렉션 내의 모든 요소에 접근하는 방법을 제공하는 패턴. 컬렉션의 요소들을 반복적으로 접근하고 싶을 때 사용하며, 코드의 가독성을 높여준다.

```python
class MyIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index >= len(self.data):
            raise StopIteration
        value = self.data[self.index]
        self.index += 1
        return value

# 예제 데이터
my_list = [1, 2, 3, 4, 5]

# 이터레이터 생성
my_iterator = MyIterator(my_list)

# 이터레이터를 사용하여 데이터 접근
for item in my_iterator:
    print(item)
```

```python
1
2
3
4
5
```

위 코드에서는 **`MyIterator`** 클래스를 정의하여 이터레이터를 구현한다. **`__iter__`** 메서드는 이터레이터 객체 자체를 반환하고, **`__next__`** 메서드는 다음 요소를 반환한다. 만약 더 이상 요소가 없다면 **`StopIteration`** 예외를 발생시켜 순회를 종료한다.

위와 같은 `__iter__`, `__next__`를 구현하는 것은 일종의 멍키 패칭이다.

<aside>
💡 **멍키 패칭**
멍키 패칭(monkey patching)은 소스 코드를 건드리지 않고 런타임에 클래스나 모듈을 변경하는 행위
멍키 패칭을 이용하면 어떤 클래스를 특정 자료형으로 만들 수 있다.

</aside>

## 순회 가능하지 않은 자료구조에 적용 가능

이터레이터 패턴은 순회 가능한 자료구조에만 적용되는 것은 아니다. 

이터레이터 패턴은 객체에 이터레이터를 구현하여 객체의 내부 구조를 노출하지 않고도 순차적으로 접근할 수 있게 해준다. 이를 순회 가능한 자료구조뿐만 아니라 다양한 객체에 적용할 수 있다.

예를 들어, 파일을 읽는 객체에 이터레이터 패턴을 적용하여, 파일의 각 줄을 차례로 반환하여 순회할 수 있도록 이터레이터를 구현할 수 있다.

또한, 데이터베이스 결과 집합을 순회하는 객체에 이터레이터를 사용하여 데이터베이스 결과 집합의 각 행을 차례로 반환하고 처리할 수 있다.

따라서 이터레이터 패턴은 순회 가능한 자료구조에만 적용되는 것이 아니라, 다양한 객체에 적용할 수 있다. 이를 통해 객체의 내부 구조를 노출하지 않고도 객체의 요소를 순차적으로 처리하는 데 유용하다.

## 사용

- 자료구조에 상관없이 객체 접근 순회 방식을 통일하고자 할 때
- 자료구조의 내부 구조를 클라이언트로부터 숨기고자 할 때
    - 클라이언트가 자료구조의 내부 구현 방식을 알고 있으면, 자료구조의 내부 구현이 달라질 때 클라이언트 코드도 변경되어야 하기 때문에 숨기는게 옳다.

## 장점

- 순회하기 위한 인터페이스가 통일된다. → 여러 자료구조에 대해 동일한 순회 방법을 제공한다.
- 자료구조의 내부 구현 방식 및 순회 방식을 알지 않아도 된다. → 순회 인터페이스만 알면
    - Client에서 iterator로 접근하기 때문에 자료구조 구현에 수정 사항이 생기더라도 iterator에 문제가 없다면 문제가 발생하지 않는다.
- [단일 책임 원칙(SRP)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21)을 준수한다.
- 전체 코드에서 자료구조가 변경되어도 클라이언트 구현 코드는 동일한 이터레이터 인터페이스를 사용하기 때문에 코드에 추가적인 수정이 필요하지 않다. → [개방 폐쇄 원칙(OCP)](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21)을 준수한다.

## 단점

- 코드의 복잡도가 증가한다.
    - 간단한 자료구조에 이터레이터 패턴을 적용하는 것은 복잡도만 증가시킨다.
    
     → 이터레이터 객체를 만드는 것이 유용한 상황인지 판단할 필요가 있다.
    

# MVC 패턴

Model, View, Controller

각자의 고유한 역할에 충실할 것을 요구하는 패턴

[**구조패턴(Structural Patterns)**](https://www.notion.so/Structural-Patterns-8bde5304c2dc48b489c42d32a83dbf99?pvs=21) 의 일종이다.

하나의 애플리케이션, 프로젝트를 구성할 때 그 구성요소를 세 가지의 역할로 구분한 패턴

![[https://m.blog.naver.com/jhc9639/220967034588](https://m.blog.naver.com/jhc9639/220967034588)](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/Untitled%2011.png)

[https://m.blog.naver.com/jhc9639/220967034588](https://m.blog.naver.com/jhc9639/220967034588)

사용자가 controller를 조작하면 controller는 model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자에게 전달한다.

## Model

애플리케이션의 정보, 데이터

데이터베이스, 처음의 정의하는 상수, 초기값, 변수

이러한 데이터들의 가공을 책임지는 컴포넌트

### **Model은 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다.**

글자가 있는 네모박스가 있을 때, Model은 박스의 위치, 박스의 크기, 글자, 글자의 위치 등의 데이터를 가지고 있어야 한다.

### **Model은 뷰나 컨트롤러에 대해서 어떤 정보도 알고 있어서는 안 된다.**

데이터 변경이 일어나더리도 모델에서 화면 UI를 직접 조정해서 수정해서는 안 된다. 즉, 뷰를 참조하는 내부 속성값을 가지면 안 된다.

### Model의 속성 변경 등 Model에 변화가 생기면, 변경을 통지해야만 한다.

모델의 속성 중 텍스트 정보가 변경이 된다면, 이벤트를 발생시켜 구독 중인 다른 객체에 전달해야 하며(→[옵저버 패턴](https://www.notion.so/Design-Pattern-2f3d3704cc2d42b89b22dbd004c57382?pvs=21)), 누군가 모델을 변경하도록 요청하는 이벤트를 보냈을 때 이를 수신할 수 있는 처리 방법을 구현해야 한다. 또한 모델은 재사용가능해야 한다.

## View

사용자들이 볼 수 있는 화면, 입력과 출력을 담당; 즉, input 텍스트, 체크박스 항목 등과 같은 사용자 인터페이스 요소, 데이터 및 객체의 입력, 그리고 보여주는 출력을 담당

애플리케이션의 메인 로직을 담당

### 모델이 가지고 있는 정보를 따로 저장해서는 안 된다.

뷰는 화면에 글자를 표시 하기 위해, 모델로부터 데이터를 전달받는다. 뷰는 모델로부터 전달받은 이 데이터를 뷰 내부에 저장하거나 유지하면 안 된다.

### 뷰는 다른 구성요소들을 몰라도 되며, 모델이나 컨트롤러만 구성요소들을 알면 된다.

뷰는 뷰 자기 자신을 제외하고는 다른 요소를 참조하거나 어떻게 동작하는지 알아서는 안 된다. 그냥 뷰는 데이터를 받고 화면에 표시해주는 역할만 하면 된다.

### 뷰는 변경사항에 대한 처리방법을 가지고 있어야 한다.

모델에 변경이 일어났을 때 뷰에는 이를 처리할 방법이 구현되어 있어야 한다.

또한 뷰는 사용자가 화면에 표시된 내용을 변경했을 때, 모델이 변경된 사항을 처리할 수 있도록 이 변경사항을 모델에게 전달해야 한다(변경 통지).

## Controller

데이터와 사용자 인터페이스 요소들을 이어주는 다리 역할

즉, 사용자가 데이터를 클릭하고, 수정하는 것에 대한 이벤트들을 처리한다.

### 컨트롤러는 모델과 뷰에 대해서 알고 있어야 한다.

모델이나 뷰는 서로의 존재를 모른다. 이를 컨트롤러가 중재하기 위해 모델과 뷰에 대해서 알고 있어야 한다. 

### 컨트롤러는 모델과 뷰의 변경사항을 모니터링 해야 한다.

컨트롤러는 모델, 뷰로부터 변경 통지를 받으면 이를 해석한 후, 중재자로서 모델과 뷰 각자에게 필요한 통지를 해야 한다.

## 왜 MVC패턴을 사용해야 할까

사용자가 보는 페이지(뷰), 데이터 처리(모델), 그리고 이 2 가지를 중간에서 제어하는 컨트롤러, 이렇게 세 가지로 구성되는 하나의 애플리케이션을 만들면 각각 맡은 바에만 집중할 수 있다.

즉, 서로 분리하여 각자의 역할에 집중할 수 있도록 개발하면 유지보수성, 애플리케이션의 확장성, 유연성이 증가하고, 중복코딩 등의 문제점이 사라진다.

![[https://twitter.com/nikkisiapno/status/1685652690662293504?s=46](https://twitter.com/nikkisiapno/status/1685652690662293504?s=46)](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/IMG_8747.jpeg)

[https://twitter.com/nikkisiapno/status/1685652690662293504?s=46](https://twitter.com/nikkisiapno/status/1685652690662293504?s=46)

![[https://twitter.com/mwaseemzakir/status/1686255761733586944?s=46](https://twitter.com/mwaseemzakir/status/1686255761733586944?s=46)](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/IMG_8765.jpeg)

[https://twitter.com/mwaseemzakir/status/1686255761733586944?s=46](https://twitter.com/mwaseemzakir/status/1686255761733586944?s=46)

![[https://twitter.com/dave_dotnet/status/1685894308711350273?s=46](https://twitter.com/dave_dotnet/status/1685894308711350273?s=46)](Design%20Pattern%202f3d3704cc2d42b89b22dbd004c57382/IMG_8763.jpeg)

[https://twitter.com/dave_dotnet/status/1685894308711350273?s=46](https://twitter.com/dave_dotnet/status/1685894308711350273?s=46)