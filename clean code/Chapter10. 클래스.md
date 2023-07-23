## 클래스 체계
  - 자바 관례에 따르면 가장 먼저 변수 목록이 나온다.
  - static public -> static private -> private instance
  - public이 필요한 경우는 거의 없다.
  ### 캡슐화
  - 변수와 유틸리티 함수는 가능한 공개하지 않는 편이 낫다.
  - 비공개 상태를 유지할 온갖 방법을 강구한다.캡슐화를 풀어주는 결정은 언제나 최후의 수단이다.

## 클래스는 작아야한다.
  - 첫째도, 둘재도 작아야한다.
  - 클래스 이름은 해당 클래스 책임을 기술해야 한다. 작명은 클래스 크기를 줄이는 첫 번째 관문이다.
  - 클래스 이름이 간결하게 떠오르지 않는다면 크기가 너무 크거나 모호하기 때문이다.
### 단일 책임 원칙
  - SRP (Single Responsibility Principle)
  - 클래스나 모듈을 변경할 이유는 단 하나뿐이어야 한다는 원칙이다.
  ``` java
  public class SuperDashboard extends JFrame implements MetaDataUser {
  	public Component getLastFocusedComponent()
  	public void setLastFocused(Component lastFocused)
  	public int getMajorVersionNumber()
  	public int getMinorVersionNumber()
  	public int getBuildNumber() 
  }
```
``` java
// 버전 정보를 다루는 메서드를 따로 빼내 Version이라는 독자적 클래스를 만든다.

  public class Version {
  	public int getMajorVersionNumber() 
  	public int getMinorVersionNumber() 
  	public int getBuildNumber()
  }
```
- 시스템은 큰 클래스 몇개보다 작은 클래스 여럿으로 이뤄진 것이 더 바람직한다.
- 작은 클래스는 맡은 책임이 하나, 변경할 이유가 하나, 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.

### 응집도 Cohesion
- 클래스는 인스턴스 변수가 작아야한다.
- 클래스 메서드는 클래스 인스턴스 변수를 하나이상 사용해야 한다.
- 일반적으로 메서드가 변수를 더 많이 사용할수록 메서드와 클래스는 응집도가 더 높다.
- 응집도가 높다는 말은 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미.

### 응집도를 유지하면 작은 클래스 여럿이 나온다.
- 변수가 아주 많은 큰 함수가 하나 있고, 큰 함수 일부를 작은 함수로 빼고 싶은데 빼내려는 코드가 큰 함수에 정의된 변수 넷을 사용한다. 그렇다면 변수 넷을 함수의 인수로 넘겨야 할까?
  변수 네개를 클래스 인스턴스 변수로 승격하면 새 함수는 인수가 필요없고, 그만큼 함수를 쪼개기 쉬워진다.
- 클래스가 응집력을 잃는다면 쪼개라.
- 리팩토링 시
  - 프로그램의 동작을 검증하는 테스트 슈트를 작성한다.
  - 한번에 하나씩 수 차례에 걸쳐 조금씩 코드를 변경한다.
  - 코드를 변경할 때마다 테스트를 수행해 원래의 프로그램과 동일하게 동작하는지 확인한다.

## 변경하기 쉬운 클래스
- 시스템은 변경이 불가피하나 변경 때마다 의도대로 동작하지 않을 위험성이 높아진다.
- 깨끗한 시스템은 클래스를 체계적으로 정리해 변경에 따르는 위험을 낮춘다.
- OCP클래스(Open-Closed Principle)
  - 확장에 개방적이고 수정에 폐쇄적이어야한다는 원칙.
  - 수정 할 때 건드릴 코드가 최소인 시스템 구조가 바람직하다.
### 변경으로부터 격리
- 구체적인 클래스는 상세한 구현(코들)를 포함하며 추상 클래스는 개념만 포함한다.
