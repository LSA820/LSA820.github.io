---
layout: single
title: "[Java] 객체지향 프로그래밍"
categories:
  - CS
---

# 객체지향 프로그래밍 (OOP)

- **캡슐화(Encapsulation)**
- **상속(Inheritance)**
- **다형성(Polymorphism)**
- **추상화(Abstraction)**

---

## 1. 캡슐화 (Encapsulation)

- 필드와 메서드를 하나의 단위로 묶고, 외부에서는 필요한 인터페이스만 제공
- 외부로부터 직접 접근을 제한하고, 내부 구현을 보호함

```java
public class Student {
    private String name;
    private int score;

    public Student(String name) {
        this.name = name;
        this.score = 0;
    }

    public void setScore(int score) {
        if (score >= 0 && score <= 100)
            this.score = score;
    }

    public int getScore() {
        return score;
    }

    public String getName() {
        return name;
    }
}

```

- name, score는 외부에서 직접 접근 불가능 → setter, getter로만 접근 가능
- 잘못된 점수 입력을 방지하는 유효성 검사 로직도 추가할 수 있음


---

## 2. 상속 (Inheritance)
- 기존 클래스의 속성과 메서드를 재사용하면서 새로운 기능을 확장
- 코드 중복을 줄이고 유지보수를 쉽게 만듬

```java
class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void speak() {
        System.out.println(name + " makes a sound");
    }
}

class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }

    @Override
    public void speak() {
        System.out.println(name + " says meow");
    }
}
```

- Cat 클래스는 Animal의 필드와 메서드를 상속
- speak() 메서드를 오버라이드(override) 하여 고양이 특유의 동작을 구현

---

## 3. 다형성 (Polymorphism)
- 같은 메서드 이름으로 다양한 동작을 수행할 수 있는 능력
- 오버로딩(overloading) 과 오버라이딩(overriding) 두 가지 형태

```java
class Calculator {
    public int multiply(int a, int b) {
        return a * b;
    }

    public double multiply(double a, double b) {
        return a * b;
    }
}
```
- 매개변수의 타입/개수가 달라지면 같은 이름의 메서드 사용 가능

```java
class Printer {
    public void print() {
        System.out.println("Default print");
    }
}

class PDFPrinter extends Printer {
    @Override
    public void print() {
        System.out.println("Printing as PDF...");
    }
}
```
- PDFPrinter는 Printer의 print() 메서드를 자신의 방식으로 재정의

---
## 4. 추상화 (Abstraction)
- 공통적인 특징만 추려내어 모델링하고, 불필요한 세부 사항은 감추는 것
- 추상 클래스 또는 인터페이스로 표현됨

```java
abstract class Device {
    public abstract void turnOn();
    public abstract void turnOff();
}

class Television extends Device {
    @Override
    public void turnOn() {
        System.out.println("TV is on");
    }

    @Override
    public void turnOff() {
        System.out.println("TV is off");
    }
}
```

- Device는 공통 동작만 정의 → 구체 동작은 Television에서 구현
- 이렇게 하면 코드 일관성 유지 및 유연한 확장 가능

## 5. SOLID 원칙
- 객체지향 설계에서 지켜야 할 5가지 핵심 원칙으로, 코드의 유지보수성과 확장성을 높이는 데 도움을 줍니다.

###1. 단일 책임 원칙 (SRP)
- 클래스는 하나의 책임만 가져야 함
- 변경 사유가 하나뿐이어야 함

```java
// 잘못된 예
class Report {
    void generate() { }
    void saveToFile() { }
}

// 개선 예
class ReportGenerator {
    void generate() { }
}

class FileSaver {
    void saveToFile(Report report) { }
}
```


###2. 개방-폐쇄 원칙 (OCP)
0 확장에는 열려 있고, 변경에는 닫혀 있어야 한다

```java
interface Shape {
    double area();
}

class Circle implements Shape {
    double r;
    public Circle(double r) { this.r = r; }
    public double area() { return Math.PI * r * r; }
}

class AreaCalculator {
    public double calcArea(Shape shape) {
        return shape.area();
    }
}
```

- Shape 인터페이스에 따라 Circle, Rectangle 등을 확장 가능
- 기존 코드를 변경하지 않고 새 기능을 추가할 수 있음

### 3. 리스코프 치환 원칙 (LSP)
- 자식 클래스는 언제나 부모 클래스로 대체 가능해야 함

```java
class Bird {
    void fly() {
        System.out.println("Bird is flying");
    }
}

class Ostrich extends Bird {
    @Override
    void fly() {
        throw new UnsupportedOperationException("Ostriches can't fly");
    }
}
```
- 위는 LSP 위반: Ostrich는 Bird를 대체할 수 없기 때문

- 해결: Flyable 인터페이스로 분리

### 4. 인터페이스 분리 원칙 (ISP)
- 클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 함

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

class SimplePrinter implements Printer {
    public void print() {
        System.out.println("Print only");
    }
}
```
- 불필요한 인터페이스 구현을 줄이고, 책임을 분리

### 5. 의존 역전 원칙 (DIP)
- 고수준 모듈은 저수준 모듈에 의존하면 안 되며, 둘 다 추상화에 의존해야 함

```java
interface Keyboard {
    void input();
}

class BluetoothKeyboard implements Keyboard {
    public void input() {
        System.out.println("Bluetooth input");
    }
}

class Computer {
    private Keyboard keyboard;

    public Computer(Keyboard keyboard) {
        this.keyboard = keyboard;
    }

    public void use() {
        keyboard.input();
    }
}
```
- Computer는 구체 구현(BluetoothKeyboard)에 의존하지 않고
- 추상 인터페이스(Keyboard) 에 의존함