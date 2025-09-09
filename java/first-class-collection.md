# 일급 컬렉션(First-Class Collection)이란?

## 정의  
일급 컬렉션은 하나의 컬렉션(List, Set 등)을 감싸는 전용 클래스를 만들어
- 컬렉션에 관한 비즈니스 로직을 담당하고  
- 컬렉션을 직접 외부에 노출하지 않고  
- 불변 및 캡슐화 원칙을 지키도록 설계하는 객체지향 패턴입니다.

대표적으로 List<Order> 대신, 이를 감싸는 `Orders` 클래스를 따로 두는 방식이 있습니다.

---

## 예시 코드(Java)

```java
public class Orders {
    private final List<Order> orders;

    public Orders(List<Order> orders) {
        validate(orders); // 검증 수행
        this.orders = new ArrayList<>(orders);
    }

    public void add(Order order) {
        if (order == null) throw new IllegalArgumentException("Order cannot be null");
        orders.add(order);
    }

    public List<Order> getAll() {
        return Collections.unmodifiableList(orders);
    }

    public double getTotalAmount() {
        return orders.stream().mapToDouble(Order::getAmount).sum();
    }
}

public class OrderService {
    private final Orders orders = new Orders(new ArrayList<>());

    public void addOrder(Order order) {
        orders.add(order);
    }

    public Orders getOrders() {
        return orders;
    }
    // 추가 비즈니스 로직...
}
```

---

## 일급 컬렉션을 사용하는 이유

1. **비즈니스 의미 부여**  
   - 단순 리스트 대신 ‘의미 있는 컬렉션’으로 네이밍  
   - 예: `Orders`, `LottoTickets`, `Students` 등

2. **컬렉션 관련 로직 캡슐화**  
   - 컬렉션 사용 규칙, 검증, 연산(합계 등)을 클래스 내부에서 처리

3. **불필요한 외부 API 노출 방지**  
   - 내부 List의 모든 메서드를 외부에서 무분별하게 사용하는 실수를 방지

4. **불변 컬렉션 구현 용이**  
   - 외부에서 직접 수정하지 못하도록 방어 코드 쉽게 추가

5. **테스트 용이 및 중복 코드 감소**

---

## 활용 예

- 로또 번호 컬렉션
- 주문 목록, 학생 명단 등 ‘동일 역할을 가진 객체 묶음’ 처리에 적합

---

## 참고자료
- [오브젝트: 코드로 이해하는 객체지향 설계](http://www.yes24.com/Product/Goods/74219491)
- [Effective Java, Item 16: 캡슐화](https://wiki.web.fc2.com/wiki/Effective+Java+%EC%98%AC%EB%B0%94%EB%A5%B8+%EC%82%AC%EC%9A%A9%EB%B2%95#item16)
