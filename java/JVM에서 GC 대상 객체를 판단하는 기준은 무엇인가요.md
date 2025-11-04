## JVM에서 GC(Garbage Collection) 대상 객체를 판단하는 기준은 무엇인가요?

> GC는 자바의 메모리 관리 방법의 하나이며, JVM의 힙 영역에서 동적으로 할당했던 메모리 중에서 필요 없어진 객체를 주기적으로 제거하는 것을 의미합니다.

> JVM에서 GC의 대상이 되는 객체는 더 이상 사용되지 않는 객체, 즉 어플리케이션에서 참조하지 않는 객체입니다. GC는 이러한 객체를 자동으로 탐지하여 메모리에서 제거함으로써 메모리 누수를 방지하고 효율적인 자원 관리를 도와줍니다.

<br>

## GC 대상 객체를 판단하는 핵심 기준
### 1. Reachability (도달 가능성)
JVM은 도달 가능성 분석(Reachability Analysis) 를 통해 객체가 GC의 대상인지 판단합니다.

- GC Root에서 출발해 객체를 추적
JVM은 몇 가지 특별한 객체들을 GC Root라고 정의하고, 이 GC Root에서 객체 그래프를 따라가며 도달 가능한 객체들을 추적합니다. (특정 객체에 대한 참조가 존재하면 도달할 수 있으며, 참조가 존재하지 않는 경우에 도달할 수 없는 상태로 간주합니다)

GC Root에서 도달할 수 없는 객체는 **"Unreachable"** 상태로 간주되어 GC 대상이 됩니다.

### 2. GC Root의 예시
다음은 대표적인 GC Root입니다:

- 스레드 스택 내 지역 변수

- 정적 필드(static fields)

- JNI에서 참조하는 객체 (Native method 참조)

- 활성 스레드 객체

이러한 GC Root에서 시작해서 연결된 객체들은 살아있는(live) 객체로 간주됩니다.

### 3. 순환 참조는 GC 대상일 수 있음
Java에서는 순환 참조가 있더라도 GC Root에서 접근할 수 없다면, 해당 객체들은 GC 대상이 됩니다.
```java
class A {
    B b;
}
class B {
    A a;
}

// A와 B가 서로를 참조하더라도,
// 외부에서 A 또는 B를 참조하지 않으면 GC 대상

```

<br>

## 개발자가 GC 대상 판단에 관여할 수는 없나요?
```java
Origin o = new Origin();
WeakReference<Origin> wo = new WeakReference<>(o);
```
자바에서는 java.lang.ref 패키지의 SoftReference, WeakReference 클래스를 통해 개발자가 GC 대상 판단에 일정 부분 관여할 수 있습니다. 
해당 클래스들의 객체(reference object)는 원본 객체(referent)를 감싸서 생성하는데요. 이렇게 생성된 객체는 GC가 특별하게 취급합니다. 
SoftReference 객체에 감싸인 객체는 Root Set으로부터 참조가 없는 경우에, 남아있는 힙 메모리의 크기에 따라서 GC 여부가 결정됩니다. 
반면, WeakReference 객체에 감싸인 객체는 Root Set으로부터 참조가 없는 경우, 바로 GC 대상이 됩니다.

- JVM의 다양한 참조 수준

| 참조 유형           | 설명                                                       | GC 대상 여부            |
|----------------------|------------------------------------------------------------|--------------------------|
| **Strong Reference** | 일반적인 객체 참조 방식 (`new` 키워드 등)                 | ❌ GC 대상 아님 (일반 참조)  |
| **Soft Reference**   | 메모리 부족 시 GC 대상 (`java.lang.ref.SoftReference`)   | ⭕ 메모리 부족 시 GC 대상     |
| **Weak Reference**   | 다음 GC 사이클에서 수거 가능 (`java.lang.ref.WeakReference`) | ⭕ 즉시 수거 가능         |
| **Phantom Reference**| 객체가 수거된 이후 후처리용 (`java.lang.ref.PhantomReference`) | ⭕ 이미 수거됨 (사용 불가) |

---
### 참고 자료
- [네이버 D2 - Java Reference와 GC](https://d2.naver.com/helloworld/329631)
- [[Java] 참조 도달 가능성 reachability](https://lifework-archive-reservoir.tistory.com/323#:~:text=%EC%B0%B8%EC%A1%B0%20%EB%8F%84%EB%8B%AC%20%EA%B0%80%EB%8A%A5%EC%84%B1(reachability)%EC%9D%80%20%EC%9E%90%EB%B0%94%EC%9D%98%20GC%EA%B0%80%20%EA%B0%9D%EC%B2%B4,%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EB%88%84%EC%88%98%EB%A5%BC%20%EB%B0%A9%EC%A7%80%ED%95%9C%EB%8B%A4.)
