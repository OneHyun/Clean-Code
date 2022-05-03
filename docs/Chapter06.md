- [6장 객체와 자료 구조](#객체와-자료-구조)
    
    [자료 추상화](#자료-추상화)

    [자료 / 객체 비대칭](#자료--객체-비대칭)

    [디미터 법칙](#디미터-법칙)

- [6장을 돌이켜보며](#6장을-돌이켜보며)


***

## 객체와 자료 구조

변수를 비공개, private 상태로 설정하는 이유가 있다.

남들이 변수에 의존하지 않게 하기 위해서이다. 허나, get 함수와 set 함수를 제공한다면, 어떤 변수인지 노출하기에 비공개 변수를 사용한 의미가 존재하지 않는다.

그렇기에 우리는 이 점에 유의하여, 작업을 하여야 한다.

### 자료 추상화

우선, 아래 두 가지 예시가 존재한다.

<pre>
<code>
//1. 구체적인 Point 클래스  
public class Point {
    public double x;
    public double y; 
}

//2. 추상적인 Point 클래스
public interface Point {
    double getX(); 
    double getY();
    void setCartesian(double x, double y); 
    double getR();
    double getTheta(); 
    void setPolar(double r, double theta); 
}
</code>
</pre>

위 예시의 경우, 우리는 추상적 point 클래스가 직교좌표계를 사용하는지, 극좌표계를 사용하는지 알 수 없다.   그럼에도, 자료구조는 명백히 표현하여 알 수 있다.

또한 추상적 point 클래스는 자료 구조 이상을 표현한다.   클래스 메소드가 접근 정책을 강제하고, 좌표를 읽을 때는 각 값을 개별적으로 읽어야 한다.   그렇지만 좌표를 설정 할 때는 두 값을 한 번에 설정해야 한다.

반면, 구체적 point 클래스는 확실히 직교 좌표계를 사용하며, 개별적으로 좌표값을 읽고 설정하도록 강제하고 있다.   구현 또한 노출하고 있고 말이다.

우리는 추상 인터페이스를 제공해 사용자가 구현을 모른 채, 자료의 핵심을 조작할 수 있어야 진정한 의미의 클래스라는 것을 생각하여야 한다.

<pre>
<code>
//1. 구체적인 Vehicle 클래스 
public interface Vehicle {
    double getFuelTankCapacityInGallons(); 
    double getGallonsOfGasoline(); 
}

//2. 추상적인 Vehicle 클래스 
public interface Vehicle {
    double getPercentFuelRemain();
}
</pre>
</code>

위 예제에서 구체적 vehicle 클래스는 자동차 연료 상태를 구체적인 숫자로 알려주는 것을 알 수 있다.

반면, 추상적 vehicle 클래스는 남은 자동차 연료 상태를 백분율이라는 추상적 개념으로 알려주고 있다. 구체적인 숫자를 공개하지 않는다.

이처럼 우리는 자료를 세세하게 공개하기 보다 추상적인 개념으로 표현하는 편이 더 좋다.

***

### 자료 / 객체 비대칭

**객체** 는 추상화 뒤로 자료를 숨긴 채, 자료를 다투는 함수만 공개하며, 

**자료 구조** 는 자료를 그대로 공개하며 별 다른 함수를 제공하지 않는다.

이것이 방금 전, 알아본 객체와 자료 구조의 차이이며, 사소한 차이로 보일지 모르지만, 그 차이의 영향은 크다.

아래 예제를 보자

<pre>
<code>
//1. 절차적인 도형
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
}

public class Geometry {
  public final double PI = 3.141592653589793;

  public double area(Object shape) throws NoSuchShapeException 
  {
    if (shape instanceof Square) {
      Square s = (Square)shape;
      return s.side * s.side;
    } 
    else if (shape instanceof Rectangle) {
      Rectangle r = (Rectangle)shape;
      return r.height * r.width;
    } 
    else if (shape instanceof Circle) {
      Circle c = (Circle)shape;
      return PI * c.radius * c.radius;
    }
    throw new NoSuchShapeException();
  }
}
</code>
</pre>

Square, Rectangle, Circle 은 아무런 함수를 제공하지 않는 단순한 자료구조이며, 도형의 동작은 Geometry 클래스에서 구현한다.

Geometry 클래스에서 함수를 추가하고자 해도, 자료구조들은 아무런 영향을 받지 않는다.   다만, 새로운 도형 자료 구조를 추가하고자 할 경우에는, 동작을 책임지는 Geometry 클래스에 속한 함수들을 모두 고쳐야 한다.

그럼 객체 지향적인 도형으로 예제를 보자.
<pre>
<code>
//2. 다형적인 도형
public class Square implements Shape {
  private Point topLeft;
  private double side;

  public double area() {
    return side * side;
  }
}

public class Rectangle implements Shape {
  private Point topLeft;
  private double height;
  private double width;

  public double area() {
    return height * width;
  }
}

public class Circle implements Shape {
  private Point center;
  private double radius;
  public final double PI = 3.141592653589793;

  public double area() {
    return PI * radius * radius;
  }
}
</pre>
</code>

위 예제, 객체 지향적인 도형 클래스에서 area()는 다형 메서드이다. 그렇기에 Geometry 클래스가 필요없고, 새 도형을 추가해도 기존 함수에 영향이 없다.

반면, 새 함수를 추가 하고자 할 경우는, 도형 클래스 전부를 고쳐야 한다.

이 처럼, 자료 구조 지향 설계와 객체 지향 설계는 서로 양분된다.

#### 두 줄 정리

- 자료 구조 지향 설계
    -- 기존의 자료 구조를 고치지 않으면서 새 함수를 추가하기 쉽다. 
    -- 반면 새 자료 구조를 추가하려면 모든 함수를 고쳐야 한다. 

- 객체 지향 설계
    -- 새 객체를 추가하기 쉽다. 
    -- 반면 객체의 동작을 표현하는 함수를 생성하기가 어렵고, 새로운 함수를 추가하면 모든 객체의 함수를 고쳐야 한다. 

***

### 디미터 법칙

디미터 법칙은 잘 알려진 휴리스틱으로, 모듈은 자신이 조작하는 객체의 내부를 몰라야 한다는 법칙이다.

( 휴리스틱 이론 : 불충분한 시간이나 정보로 인하여 합리적인 판단을 할 수 없거나, 체계적이면서 합리적인 판단이 굳이 필요하지 않은 상황에서 사람들이 빠르게 사용할 수 있게 보다 용이하게 구성된 간편추론의 방법 )

앞 절에서 봤듯이, 객체는 자료를 숨기고 함수를 공개한다. 그것이 객체는 get 함수로 내부 구조를 공개하면 안된다는 의미이다.

우선, 디미터 법칙에 설명하자면, 디미터 법칙은 "클래스 c에 있는 메소드 f는 다음과 같은 객체의 메소드만 호출해야"한다고 한다.

- 클래스 C  
- f가 생성한 객체  
- f 인수로 넘어온 객체  
- C인스턴스 변수에 저장된 객체

이 규칙에서는 허용된 메소드 f가 반환하는 객체의 메소드를 호출하면 안된다. 

아래 예제는 객체인지, 자료 구조인지에 따라 디미터 법칙을 어기는 예제이다.

<pre>
<code>
final String output = ctxt.getOptions().getScratchDir().getAbsolutePath(); 
</code>
</pre>

이 예제는 getOptions() 함수가 반환하는 객체의 getScratchDir() 함수를 호출한 후, 해당 함수가 반환하는 getAbsolutePath()함수를 호출하고 있다.

이와 같은 경우는 우리는 **기차 출동**이라 부른다. 여러 객체가 한 줄로 이어진 기차처럼 보이기 때문이다.

이 예제를 바꾸어 정리하면,
<pre>
<code>
Options opts = ctxt.getOptions(); 
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath(); 
</code>
</pre>
와 같다.

위 예제가 디미터 법칙을 위반하는지 여부는 ctxt, opts, scratchDir이 객체인지, 자료 구조인지에 따라 다르다.

**객체**면 내부 구조를 숨겨야 하기에 디미터 법칙을 어긴 것이다.   **자료 구조**라면, 당연히 노출하여야 하기에 적용되지 않는다.

***

### 6장을 돌이켜보며

객체는 동작을 공개하고 자료를 숨기기에, 새로운 객체를 추가하기는 쉬우나, 새로운 행동, 즉 함수를 추가하기는 번거롭다.

반면, 자료 구조는 자료를 공개하기에, 새로운 행동, 즉 함수를 추가하기는 쉬우나, 기존 함수에 새로운 자료 구조를 추가하기가 번거롭다.

이렇게 객체와 자료 구조의 차이를 알 수 있었다.

하지만 디미터 법칙이 나오고, 이러한 부분들을 간략적으로 이해는 했으나, 전문적인 지식을 다루는 책이다 보니, 한 순간에 이것을 읽고, 누군가에게 설명을 할 수 있을까 라는 생각이 드는 장이었던 것 같았다.

이러한 부분에 누군가 물어본다면, 당당히 설명할 수 있는 날이 되기를 고대한다.

***

## 해시태그 ##
#코딩 #개발자 #노마드코더 #북클럽 #노개북

***

## 작성시기 ##
2022.05.03

***