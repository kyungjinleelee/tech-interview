## Record를 DTO로 사용하는 이유가 뭔가요?
Java에서 `record` 는 Java 16에서 정식 출시된 특별한 유형의 클래스로 DTO(Data Transfer Object) 역할을 수행하기에 매우 적합한 특성을 가지고 있습니다.

### 1. 불변성 (Immutability) 보장
>
> "DTO는 보통 데이터를 전달하는 역할만 하므로 불변(immutable)해야 한다."
>

record는 기본적으로 **불변** 객체입니다. 기존의 클래스와 달리 모든 필드가 `final` 키워드로 선언되어, 객체 생성 후 변경할 수 없기 때문입니다. DTO는 단순 데이터 전달이 목적이므로 불변성이 강제되는 것이 안전합니다.

```java
// 기존 class 기반 DTO (불변성을 유지하려면 추가 코드 필요)
public class ProductDTO {
    private final String name;
    private final int price;

    public ProductDTO(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public String getName() { return name; }
    public int getPrice() { return price; }
}
```
```java
// Record 기반 DTO (불변성이 기본 적용됨)
public record ProductDTO(String name, int price) {
}
```

### 2. 보일러 플레이트 코드 감소
>
> "DTO는 데이터를 담는 역할만 하므로 최소한의 코드로 정의하는 것이 좋다."
>

기존 class 기반 DTO는 생성자, getter, `equals()`, `hashCode()`, `toString()` 을 직접 구현해야 했습니다. record는 필드 선언만으로 자동으로 생성자, getter, `equals()`, `hashCode()`, `toString()` 등 메서드를 자동으로 생성해 주어 보일러 플레이트 코드를 줄일 수 있습니다.

```java
// 기존 class 기반 DTO
public class ProductDTO {
    private final String name;
    private final int price;

    public ProductDTO(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public String getName() { return name; }
    public int getPrice() { return price; }

    @Override
    public boolean equals(Object o) { ... }   // 직접 구현 필요
    @Override
    public int hashCode() { ... }             // 직접 구현 필요
    @Override
    public String toString() { ... }          // 직접 구현 필요
}
```

```java
// Record 기반 DTO (자동으로 모든 메서드 생성)
public record ProductDTO(String name, int price) {
}

```
> Lombok(롬복) 같은 도구를 사용하면 반복적인 코드를 직접 작성하지 않고도 완전한 기능을 갖춘 DTO를 만들 수 있습니다. 하지만 Record는 기본적으로 불변성을 가지며, 반복적인 코드를 제거하는 또 다른 방식을 제공합니다.

### 3. 결론, 왜 Record를 사용할까?
아래와 같은 특성으로 인해 멀티 스레드 환경에서 데이터가 의도치 않게 변경되지 않고 안전하게 전달할 수 있습니다.
- **불변성 보장**: 일반적으로 변경할 수 있는 DTO와는 다르게 한 번 Record를 만들면 그 데이터를 변경할 수 없습니다.
- **간결한 문법**: 필드만 선언하면 Java가 자동으로 생성자, Getter, equals(), hashCode(), toString() 메서드를 만들어 줍니다.
- **Setter 없음**: Record는 불변 객체이기 때문에 Setter 메서드를 제공하지 않습니다.
- **명확한 의도**: Record를 사용하면 해당 객체가 추가적인 동작이나 로직 없이 데이터를 전달하기 위한 것임을 분명하게 알 수 있습니다.

---

## 그럼 Record로 생성한 모든 객체는 DTO인가요?
모든 Record 객체가 DTO인 것은 아닙니다. Record는 단순히 데이터를 캡슐화하는 역할을 하는데, DTO 외에도 값 객체(Value Objects, VO) 등의 다양한 용도로 사용될 수 있습니다.
```java
// 값 객체로 사용
public record Coordinates(double x, double y) {}
```
DTO는 계층 간 데이터 전송을 목적으로 하는 객체인 반면, VO는 도메인 모델 내에서 특정 값을 표현하는 객체로 사용됩니다. 따라서, Record는 이 두 가지 모두에 적합하게 사용할 수 있지만 그 목적에 따라 사용 방법이 달라집니다.

### Record와 VO를 비교한다면?
Record와 VO는 모두 객체의 상태가 변경되지 않는 것을 보장합니다. 또 데이터를 캡슐화하여 표현하는 데 초점을 맞춥니다. 마지막으로 VO는 값 기반의 동등성을 가지며, Record도 동일한 필드 값을 가지면 동일한 객체로 간주된다는 점이 공통점입니다.

VO는 도메인 모델내에서 특정 개념을 표현하고, 도메인 로직과 밀접하게 관련이 있습니다. 즉, VO는 비즈니스 로직이나 규칙을 가질 수 있습니다. 반면에 Record는 단순히 데이터를 캡슐화하여 전달하는데 의미가 있습니다.

결론적으로 Record는 VO를 구현하는 데 적합하지만, VO의 모든 특성을 완벽히 대체하지는 않습니다. VO는 더 넓은 도메인 맥락에서 사용되며, 비즈니스 로직을 포함할 수 있습니다.

---

## Record를 도메인 객체에 사용할 수 있을까요?
지양하는 것이 좋습니다. 공식 문서에 따르면 아래와 같이 명시되어 있습니다.
- Record는 불변 데이터를 전달하기 위한 캐리어이다.
- 단순한 값의 집합을 표현하는 객체 지향적인 구성을 고안한다.
- 개발자가 확장 가능한 동작보다는 불변의 데이터 모델링에 집중할 수 있도록 도와준다.

따라서 도메인 객체는 단순히 데이터를 갖고 있는 것 뿐만 아니라, 추가적인 비즈니스 로직이 들어가는 경우가 대부분이기 때문에 단순히 데이터만 전달하기 위해 사용되는 Record의 목적성에는 부합하지 않는다고 볼 수 있습니다.

따라서 Record는 도메인 객체보다는 DTO에서 활용하는 것이 바람직하다고 생각합니다.

---

## Record의 한계는 뭐가 있을까요?
- Record는 `extends`를 사용하여 다른 클래스를 상속할 수 없고, 필드가 `final`로 선언되기 때문에 확장이 어렵습니다.
- 또 주로 데이터를 전달하려는 목적으로 설계되었기 때문에 비즈니스 로직을 포함하기에 적절하지 않습니다.
- 마지막으로 Java 14 또는 16 이전 버전에서 호환이 불가능하다는 단점이 있습니다.

### 참고 자료
- [자바 DTO vs Record, 무엇을 사용해야 할까?](https://yozm.wishket.com/magazine/detail/2814/)
- [Java의 레코드 - 전체 튜토리얼 - 사용하지 않는 최고의 새로운 Java 기능](https://www.youtube.com/watch?v=gJ9DYC-jswo)
- [[10분 테코톡] 타칸의 Record](https://www.youtube.com/watch?si=YX2n6iCZuUvjH2S3&v=MiHxFpTgAog&feature=youtu.be)
- [[10분 테코톡] 🎼라흐의 DTO vs VO](https://www.youtube.com/watch?v=J_Dr6R0Ov8E)
