# 오류처리
무언가 잘못된 가능성은 늘 존재하며 바로 잡을 책임은 프로그래머에게 있다.

## 오류 코드보다 예외를 사용하라.
## Try-Catch-Fianlly문부터 작성하라.
try블록은 트랜잭션과 비슷하며 이 블록안에서 무슨일이 생겨도 catch블록은 상태를 일관성 있게 유지한다.       
예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성할 것.       
자연스럽게 try블록의 트랜잭션 범위부터 구현하게 되어 범위 내에서 트랜잭션 본질을 유지하기 쉬워진다.
## 미확인(unchecked)예외를 사용하라.
## 예외에 의미를 제공하라.
오류 메시지에 실패한 연산 이름과 실패 유형 등의 정보를 담아 예외와 함께 던질 것. 
## 호출자를 고려해 예외 클래스를 정의하라.
가장 중요한 건 오류를 잡아내는 방법.
외부API의 경우 wrapper클래스로 만들면 외부라이브러이와 프로그램 사이의 의존성이 크게 줄어들게 된다.     
프로그램 테스트하기도 쉽다.     
## 정상 흐름을 정의하라.
캡슐화
## null을 반환하지 마라.
```java
public void registerItem(Item item) {
  if(item != null) {
    ItemRegistry registry = peristenStore.getItemRegistry();
    if (registry != null) {
      Item existing = registry.getItem(item.getID());
      if (existing.getBillingPeriod().hasRetailOwner()) {
        existing.register(item);
      }
    }
  }
}
```
null을 반환하는 코드는 호출자에게 문제를 떠넘긴다.       
null을 반환할 필요가 있을까?         
Collections.emptyList() 같은 걸 사용해 빈 리스트를 반환하면 코드가 깔끔해진다~!
```java
public List<Employee> getEmployees(){
  if(...직원 없음..)
    return Collections.emptyList();
}
```
NullPointerException 발생활률을 줄인다.

## null을 전달하지 마라.
정상적인 인수로 null을 기대하는 API가 아니라면 메스드로 null을 전달하는 코드는 최대한 피한다.       
assert문을 사용하기도 한다.       
하지만 대다수 프로그래밍 언어는 실수로 넘어오는 null을 적절히 처리하는 방법이 없으므로 애초에 null을 넘기지 못하도록 금지하는 정책이 합리적이다.

## 결론
깨끗한 코드는 가독성과 함께 안정성도 높아야한다.        
오류처리를 프로그램 논리와 분리하면 독립적인 추론이 가능하며 코드 유지보수성도 높아진다.       




--------
``` 신규 부서 적응이라는 핑계로,, 책을 대체 얼마나 오래 읽고 있는 건지 약간의 현타. 얼른 끝내고 다음거 하쟈~~~ ```

