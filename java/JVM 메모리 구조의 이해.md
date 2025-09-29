# JVM 메모리 구조의 이해

## 1. JVM 메모리 구조란?

- JVM(Java Virtual Machine)이 Java 프로그램을 실행할 때 사용하는 메모리 영역의 구분과 역할을 뜻함
- 주요 영역: Method Area, Heap, Stack, PC Register, Native Method Stack

---

## 2. 주요 메모리 영역별 특징

### (1) Method Area
- 클래스, 메서드 메타데이터(정보), static 변수, final 상수 등 저장
- JVM 전체가 공유

### (2) Heap
- 객체(인스턴스)와 배열이 저장되는 공간
- GC(Garbage Collection) 대상  
- 모든 스레드에서 공유

### (3) Stack
- 각 쓰레드별로 독립적으로 할당
- 메서드 호출 시의 지역 변수, 참조(primitive, reference) 저장
- 메서드 실행이 끝나면 해당 Stack Frame은 바로 소멸

### (4) PC(Register)
- 현재 수행 중인 JVM 명령어(바이트코드)의 위치를 저장

### (5) Native Method Stack
- JVM 이외의 네이티브 코드(C/C++ 등) 호출 시 사용하는 스택

---

## 3. JVM 메모리 구조 예시 그림

```mermaid
flowchart TD
    MethodArea[메서드 영역(Method Area)]
    Heap[힙 영역(Heap)]
    Stack[스택 영역(Stack)]
    PC[PC 레지스터(PC Register)]
    NativeStack[네이티브 메서드 스택(Native Method Stack)]
    
    subgraph JVM
      MethodArea
      Heap
      Stack
      PC
      NativeStack
    end
```

---

## 4. GC와 메모리 구조

- **GC(Garbage Collection)**: Heap 영역의 객체를 대상으로 메모리 해제 진행
- Stack/Method Area 등은 GC의 영향 없음

---

## 5. 실전에서 유의할 점

- 메모리 할당 최적화 및 GC 튜닝(Heap 사이즈, PermGen/Metaspace 등)
- StackOverflowError, OutOfMemoryError의 주요 원인과 해소법 이해 필요

---

## 6. 면접/실전 한줄 Tip

- "JVM 메모리 구조는 Heap/Stack 등 논리구조로 구분되고, GC는 Heap만 청소한다"  
- "Stack은 각 쓰레드별, Heap/Method Area는 애플리케이션 전역"

---

### 참고 자료

- [JVM 공식 문서](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html)
- [Baeldung - Memory Structure of JVM](https://www.baeldung.com/jvm-memory-structure)
- [한 장으로 보는 JVM 메모리 구조 - inflearn blog](https://www.inflearn.com/blog/jvm-memory-structure)
