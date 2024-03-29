## 외부코드사용하기
- Sensor 객체를 답는 Map을 만들기 위해 다음과 같이 Map을 생성함.
```java
  Map sensors - new HashMap();
  Sensor s = (Sensor)sensors.get(sensroId);
```
- 이런 형태의 코드가 여러차례 나오며, Map이 반환하는 Object를 올바른 유형으로 변환할 책임은 Map을 사용하는 클라이언트에 있음.      
- 코드는 동작하지만 깨끗한 코드라 보기는 어렵다.     
- 제네릭을 사용하면 가독성이 높아진다.
```java
  Map<String, Sensor> sensors = new HashMap<Sensor>();
  ...
  Sensor s = sensor.get(sensorId);
```
- 제네릭을 사용하는 방법도 Map객체가 필요이상의 기능을 제공하는 건 해결하지 못한다.      
- Map인터페이스가 변할경우 수정의 부담이 늘어남.
```java
  public class Sensors {
    private Map sensors = new HashMap();

    public Sensor getById(String id){
      return (Sensor) sensors.get(id);
    }
  }
```
- 경계 인터페이스인 Map을 Sensors안으로 숨김으로서 다른 프로그램에 영향을 미치지 않는다.    
- Map과 같은 경계인터페이스를 이용할 때는 사용하는 클래스나 클래스 계열 밖으로 노출되지 않게 할 것.
- Map인스턴스를 공개 APi의 인스로 넘기거나 반환값으로 사용하지 말 것.

## 경계 살피고 읽히기
- 외부코드를 사용하기 전 간단한 테스트 케이스를 작성해 익힐 수 있음.
### 학습 테스트는 공짜이상이다.
- 학습 테스트는 패키지가 예상대로 도는지 검증한다.
- 비용이 없다.
- 메인로직에 영향을 주지않고 이해할 수 있다.

## 아직 존재하지 않는 코드를 사용하기
- 아는 코드와 모르는 코드를 분리하는 경계.

## 깨끗한 경계
- 설계가 우수하다면 변경하는데 많은 투자와 재작업이 필요하지 않다.
- 경계에 위치하는 코드는 깔끔하게 분리한다.
- 통제 불가능한 외부 패키지에 의존하는 대신 통제가 가능한 우리코드에 의존하는 편이 좋다.
- 외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리할 것.
  - 새로운 클래스로 경계를 감싸거나 ADAPTER패턴을 사용해 우리가 원하는 인터페이스를 패키지가 제공하는 인터페이스로 변환하자.
- 코드가독성이 높아지고 일관성이 높아지며 외부 패키지가 변했을 때 변경할 코드도 줄어든다.




