## String 객체는 가변일까요, 불변일까요? 그렇게 생각하신 이유도 함께 설명해 주세요.
Java의 `String` 객체는 **불변(immutable)** 입니다.

- **불변 객체**란?
  - 불변 객체는 생성된 후 내부 상태(값)가 변경될 수 없는 객체를 말합니다.
  - String의 경우, 문자열을 한 번 생성하면 그 값을 바꿀 수 없습니다.

<br>

## String의 특징과 설계 이유
String 클래스는 `final`로 선언되어 있으며, 내부적으로 char[] 배열로 데이터를 저장하지만 이 배열 또한 외부에서 수정할 수 없습니다.

또한 String은 참조 타입(Reference Type)이기 때문에 객체의 메서드들(예: `concat`, `substring`, `replace`)은 모두 새로운 문자열을 반환하지, 원래 문자열을 변경하지 않습니다. (= 새로운 String 객체를 참조하고 기존 객체를 수정하지 않는다는 말)

따라서 String 객체를 불변하게 유지할 수 있습니다.

```java
public class StringTest {
    public static void main(String[] args) {
        String original = "hello";
        String modified = original.concat(" world");

        System.out.println("original: " + original);  // hello
        System.out.println("modified: " + modified);  // hello world
    }
}
```
- 위 코드에서 original은 "hello" 문자열을 가지고 있지만, concat을 통해 문자열을 붙여도 원본은 변하지 않고, **새로운 객체(modified)** 가 만들어집니다.

<br>

## String을 불변으로 설계한 이유는 무엇일까요?
String을 불변으로 설계한 덕분에 많은 이점을 얻을 수 있습니다.
- **캐싱과 성능 향상**: **String Constant Pool**을 사용하여 같은 문자열을 재사용할 수 있습니다. (같은 객체를 공유할 수 있기 때문에 메모리를 효율적으로 사용할 수 있게 됩니다) 또한 해시코드를 한 번만 계산하고 이를 **캐싱해서 재사용**할 수 있습니다.
- **보안**: String은 흔히 비밀번호, URL, 파일 경로 등 **민감한 정보를 안전하게** 다룰 수 있습니다. 불변한 객체는 변경할 수 없기 때문에 민감한 정보가 예기치 않게 수정되는 것을 방지할 수 있기 때문입니다.
- **스레드 안정성**: 불변한 객체는 **멀티 스레드 환경에서 thread-safe**합니다. 문자열을 변경하면 String Constant Pool에 새로운 객체를 생성하기 때문에 동기화를 신경쓸 필요가 없습니다.

<br>

## 정리
| 항목       | 설명                                                         |
|------------|--------------------------------------------------------------|
| 가변 여부   | ❌ 가변 아님 (✅ 불변 객체)                                     |
| 값 변경    | 기존 값을 변경하지 않고 새 객체를 생성                          |
| 이유       | 보안, 스레드 안정성, 성능, 설계 일관성을 위해                   |

필요하다면 `StringBuilder`나 `StringBuffer` 같은 가변 문자열 클래스를 사용할 수 있습니다.

<br>

### 번외) 리터럴로 생성한 String 객체와 생성자로 생성한 String 객체를 비교하면 어떤 차이가 있을까요?
두 방식으로 생성한 객체는 같은 문자열을 갖더라도 메모리 상에서 다르게 처리됩니다.
```java
String first = "hello"; // 리터럴로 생성
String second = new String("hello"); // 생성자로 생성
String third = "hello";

System.out.println(System.identityHashCode(first)); // 498931366
System.out.println(System.identityHashCode(second)); // 2060468723
System.out.println(System.identityHashCode(third)); // 498931366
```
**리터럴로 생성한 String 객체**는 **Heap 영역의 String Constant Pool에 저장**되어 동일한 문자열을 재사용할 수 있습니다. 문자열이 String Constant Pool에 이미 존재하면 같은 주소를 참조합니다. 
반면, **생성자로 생성한 String 객체**는 **Heap 영역에 저장**되어 동일한 문자열이더라도 항상 새로운 객체를 생성합니다.

```java
String first = "hello";
String second = new String("hello");
String third = second.intern(); // intern() 메서드 사용

System.out.println(System.identityHashCode(first)); // 498931366
System.out.println(System.identityHashCode(second)); // 2060468723
System.out.println(System.identityHashCode(third)); // 498931366
```
`intern()` 메서드를 사용하면 Heap 영역에 저장된 String 객체를 String Constant Pool에 저장할 수 있습니다. `intern()` 메서드는 해당 문자열이 String Constant Pool에 존재할 경우 그 주솟값을 반환하고, 없을 경우 String Constant Pool에 추가하고 새로운 주솟값을 반환합니다.

---

### 참고 자료
- [Baeldung - Why String Is Immutable in Java?](https://www.baeldung.com/java-string-immutable)
- [Java의 String 이야기(1) - String은 왜 불변(Immutable)일까?](https://readystory.tistory.com/139)
