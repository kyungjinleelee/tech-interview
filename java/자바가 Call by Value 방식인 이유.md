## 자바가 Call by Value 방식인 이유에 대해 설명해주세요.
자바는 무조건 값을 복사해서 전달합니다. 이 말은 즉,

- **원시 타입(int, double 등)** 은 값을 그대로 복사해서 넘기고,
- **참조 타입(Student, List, Map 등)** 은 참조값(즉, 객체의 주소) 을 복사해서 넘긴다는 의미입니다.

즉, "참조값(객체의 주소)을 넘겼지만 **복사해서 넘긴 것**이므로 **Call By Value**다" 라는 것입니다.

<br>

> 자바의 데이터 타입은 다음과 같이 크게 두 가지로 나누어집니다.
>
> - 원시 타입(primitive type) - Numeric Type(byte, short, int, float, long, double, char), Boolean Type(boolean)
> - 참조 타입(reference type) - Class Type, Interface Type, Array Type, Enum Type, 기타 참조 타입(String 등)
>
> 그래서 메소드 파라미터로 원시 타입을 전달하는 것과 참조 타입을 전달하는 것에는 동작 방식에 차이가 있습니다.

<br>

## 예제
### 1. 원시 타입 (Primitive)
```java
public class Example {
    public static void main(String[] args) {
        int a = 10;
        changeValue(a);
        System.out.println(a); // 결과: 10 (변경 안됨)
    }

    public static void changeValue(int x) {
        x = 20; // x만 바뀌고 a는 그대로
    }
}
```
- x는 a의 복사본이므로 x를 바꿔도 a에는 영향이 없다.

### 2. 참조 타입 (Reference Type)
```java
class Student {
    public String name;
}

public class Example {
    public static void main(String[] args) {
        Student s = new Student();
        s.name = "Alice";
        changeName(s);
        System.out.println(s.name); // 결과: Bob (객체 내용은 변경됨)
    }

    public static void changeName(Student student) {
        student.name = "Bob"; // 같은 객체를 가리키므로 내부 필드는 변경됨
    }
}
```
- `student`는 `s`가 가리키는 객체의 주소값을 복사해서 가지고 있음. 복사된 참조를 통해 객체 내부를 수정하니까 내용은 바뀐다.

### 3. 참조 자체를 새 객체로 바꾸는 경우
```java
public class Example {
    public static void main(String[] args) {
        Student s = new Student();
        s.name = "Alice";
        replaceStudent(s);
        System.out.println(s.name); // 결과: Alice (바뀌지 않음)
    }

    public static void replaceStudent(Student student) {
        student = new Student(); // 새로운 객체를 할당
        student.name = "Charlie";
    }
}
```
- 이 경우는 참조 변수 자체를 새 객체로 바꾸지만, 복사된 참조변수만 바뀐 거라서 원래 `s`는 영향을 받지 않는다.

<br>

## 결국 주소 값을 넘기고 원본 값이 변경되니까 Call by Reference 방식 아닌가요?🤔
**자바의 참조**와 **Call by Reference의 참조**는 약간 다른 의미를 가지고 있습니다.

자바에서 참조의 의미는 **객체가 힙에 저장된 위치**를 가리키는 **메모리 주소**입니다.
실제 객체에 대한 참조가 아니라, 객체에 접근하고 조작하는 방법이라고 볼 수 있는 것입니다. 

Call by Reference는 참조 자체를 넘기기 때문에 새로운 객체를 할당하면 원본 변수도 영향을 받습니다.

```java
public class CallByExampleTest {
     @Test
     void referenceTest() {
        int[] arr = { 10 };       // 0x001
        newArr(arr);

        assertThat(arr).isEqualTo("0x001");
        }

     void newArr(int[] arrArg) {
        arrArg = new int[]{ 20 }; // 0x002
     }
}

```
전달받은 값을 새로운 객체로 변경해도 원본 변수는 변하지 않습니다. 

따라서 참조 타입 역시 Call by Value 방식으로 동작함을 보여줍니다.
newArr()가 종료되고 사용되지 않는 객체(0x002)는 GC에 의해 수거될 것입니다.

(참고: [자바에서 원시 타입과 참조 타입은 메모리 어디에 저장되나요? (스택 vs 힙)](삽입예정))

![image](https://github.com/user-attachments/assets/961e9532-51b5-4c03-be89-6294b94ed707)

결국 자바는 항상 Call by Value 방식으로 데이터를 전달하고, 호출자 변수와 수신자 파라미터는 Stack 영역 내에서 각각 독립적으로 존재하는 다른 변수라는 것이 핵심입니다.

---

## 정리
- Java에서 함수 호출 시 모든 인자는 값이 복사되어 전달됩니다.
- 참조 타입의 경우에도 객체 자체가 아닌 참조값(주소) 이 복사됩니다.
- 따라서 참조된 객체의 내용은 바뀔 수 있지만, 참조 변수 자체를 새로 바꿔도 원본에는 영향이 없습니다 → Call By Reference 방식이 아님을 의미

=> **참조 값을 넘기기 때문에 객체 내부는 바뀌지만, 객체 자체는 바뀌지 않는다!**

> 자바는 객체의 참조값을 스택에 값으로 복사해서 넘기기 때문에, 힙 객체 내부는 수정 가능하지만 객체 자체를 바꿀 순 없음.
> 따라서 자바는 Call by Value, 메모리 구조상도 그렇게 동작함.

---

### 관련 질문
- [Call By Value와 Call By Reference의 차이에 대해 설명해주세요.](https://github.com/kyungjinleelee/tech-interview/blob/main/cs/%EA%B8%B0%ED%83%80/Call%20By%20Value%EC%99%80%20Call%20By%20Reference%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94.md)
- [자바에서 원시 타입과 참조 타입은 메모리 어디에 저장되나요? (스택 vs 힙)](삽입예정)

### 참고 자료
- [[Java] 자바는 Call By Value(Pass By Value) 방식으로만 동작한다](https://mangkyu.tistory.com/322)
- [[Java] 자바가 Call by Value 방식인 이유](https://dev-coco.tistory.com/189)
- [코드라떼 - 자바 메모리 모델 기초](https://www.codelatte.io/courses/java_programming_basic/IGO4YNAECLV8MSVF)
- [코드라떼 - Java == Call By Value](https://www.codelatte.io/courses/java_programming_basic/R842DJSB0BTCRLKM)
