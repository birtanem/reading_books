# 객체와 자료구조
변수를 private로 정의하는 이유가 있음~! 남들이 변수에 의존하지 않게 만들고 싶기 때문!

## 자료 추상화
- 변수를 private로 선언하더라도 각 값마다 조회(get), 설정(set)함수를 제공한다면 구현을 외부로 노출하는 셈.
- 구현을 감추기 위해선 추상화가 필요하다.
- 즉 추상 인터페이스를 제공해 사용자가 구현을 모른채 자료의 핵심을 조작할 수 있어야 **진정한 의미의 클래스다.**
- 그러므로 개발자는 객체가 포함하는 자료를 표현할 가장 좋은 방법을 고민해야 한다. 아무 생각없이 조회/설정 함수를 추가하는 방법이 가장 나쁘다.

## 자료/객체 비대칭
- 객체는 추상화 뒤로 자료를 숨기고 자료를 다루는 함수만 공개한다.
- 자료구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.
  #### 절차적인 도형
  ``` java
  public class Square{
    public Point topLeft;
    public double side;
  }
  
  public class Rectangle{
    public Point topLeft;
    public double height;
    public double width;
  }
  
  public class Circle{
    public Point center;
    public double radius;
  }
  
  public class Geometry{
    public final double PI = 3.141592653589793;
    
    public double area(Object shape) throws NoSuchShapeException {
      if(shape instanceof Square){
        Square s = (Square) shape;
        return s.side * s.side;
      }
      else if (shape instanceof Rectangle) {
        Rectangle r = (Rectangle) shape;
        return r.height * r.width;
      }
      else if (shape instanceof Circle) {
        Circle c = (Circle)shape;
        return PI * c.radius * c.radius;
      }
      throw new NoSuchShapeException();
    }
  }
  ```
  Geometry클래스에 둘레 길이를 구하는 함수를 추가하려고 할 때 도형클래스는 아무런 영향을 받지 않는다. 새로운 도형을 추가하려면 클래스에 속한 함수를 모두 고쳐야한다.         
  
  #### 다형적인 도형
  ```java
  public class Square implements Shape{
    private Point topLeft;
    private double side;
    
    public double area(){
      return side*side;
    }
  }
  
  public class Rectangle implements Shape{
    private Point topLeft;
    private double height;
    private double width;
    
    public double area(){
      return height * width;
    }
  }
  
  public class Circle implements Shape{
    private Point center;
    private double radius;
    public final double PI = 3.141592653589793;
    
    public double area(){
      return PI * radius * radius;
    }
  }
  ```
  객체 지향적인 클래스에서는 도형을 추가해도 기존 함수에 영향을 미치지 않지만 새 함수를 추가하려면 클래스 전부를 고쳐야 한다. 
  - 객체와 자료구조는 상호보완적인 특질이 있다. 
  - 즉, 자료구조를 사용하는 절차적인 코드는 기존의 자료구조를 변경하지 않으면서 새 함수를 추가하기 쉬우나 새로운 자료구조를 추가하기 어렵고
    객체 지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽지만 함수를 추가하기 어렵다. 
  
## 디미터 법칙
- 잘 알려진 휴리스틱. : 모듈은 자신이 조작하는 객체의 속 사정을 몰라야 한다는 법칙.
- 클래스 C의 메서드f는 다음과 같은 객체의 메서드만 호출해야 한다.         
      ` 클래스C ` , `f가 생성한 객체`, `f인수로 넘어온 객체`, `C인스턴스 변수에 저장된 객체`
   ### 기차충돌
   ``` final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath(); ```
   - 이러한 코드를 기차충돌이라고 한다. 
   - 조잡하다 여겨지는 방식이므로 피한다. 아래와 같은 형식으로 나누는 편이 좋다. 
      ```java
        Options opts = ctxt.getOptions();
        File scratchDir = opts.getScratchDir();
        final String outputDir = scratchDir.getAbsolutePath();
      ```
    ### 잡종구조
    - 절반은 객체 ,절반은 자료 구조인 형태.
    - 새로운 함수 및 새로운 자료구조도 추가하기 어렵다.
    - 단점만 모아놓은 구조이므로 되도록 피할 것.
    
    ### 구조체 감추기
    - 위의 outputDir 예제의 경우 좋은 방식이 아니다. 이 경로의 필요성은..?
    ``` java
      String outFile = outputDir + "/" + className.replace('.', '/') + ".class"; 
      FileOutputStream fout = new FileOutputStream(outFile); 
      BufferedOutputStream bos = new BufferedOutputStream(fout);
    ```
    - 추상화 수준을 뒤섞어 놓아 다소 불편하다. 
    - 점, 슬래시, 파일 확장자, File 객체를 부주의하게 마구 뒤섞으면 안 된다. 
    - 위 코드에 따르면 경로를 얻으려는 이유가 임시 파일을 생성하기 위함을 알 수 있다.
    다음과 같이 ctxt객체에 임시파일을 생성하라고 시킬 수 있다.
    ```
    BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName); 
    ```
    - ctxt는 내부 구조를 드러내지 않으며, 모듈은 자신이 몰라야 하는 여러 객체를 탐색할 필요가 없다. 따라서 디미터 법칙을 위반하지 않는다.

## 자료 전달 객체
- 자료 구조체의 전형적인 형태는 공개변수만 있고 함수가 없는 클래스.
- 자료 전달 객체(DTO, Data Transfer Object)라고 한다.
- 데이터베이스와 통신하거나 소켓에서 받은 메세지의 구문을 분석할 때 유용하며, DB에 저장된 가공되지 않은 정보를 사용할 객체로 변환하는 단계에서 가장 처음으로 사용하는 구조체이기도 하다.
- 일반적 형태는 Bean구조.

   ### 활성레코드
    - DTO의 특수한 형태.
    - 공개 변수가 있거나 비공개변수에 getter/setter함수가 있는 자료구조지만 save/find같은 탐색 함수도 제공한다. 
    - 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과다.
    - 활성레코드에 비즈니스 규칙 메서드를 추가해 이런 자료구조를 객체로 취급하는 개발자가 흔한데 이는 바람직하지 않다. 자료구조도 아니고 객체도 아닌 잡종 구조가 되기 때문.
    - 해결책 : 활성 레코드는 자료구조로 취급 할 것. 비즈니스 규칙을 담으면서 내부자료를 숨기는 객체는 따로 생성한다.
 
 ## 결론
 - 객체는 동작을 공개하고 자료를 숨긴다.
 - 자료구조는 별다른 동작없이 자료를 노출한다. 
 - 즉, 새로운 자료 타입을 추가하는 유연성이 필요하면 객체가 적합하고 새로운 동작을 추가하는 유연성이 필요하면 자료구조와 절차적인 코드가 더 적합하다. 
 
 
 
  

   
  
 
      
