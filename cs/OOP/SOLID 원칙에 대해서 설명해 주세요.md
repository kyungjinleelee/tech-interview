# SOLID 원칙에 대해서 설명해 주세요.
SOLID 원칙은 객체지향 설계 5원칙이라고도 불리며, 각 원칙의 앞 글자를 따서 만들어졌습니다.

## 1. 단일 책임 원칙(Single Responsibilty Principle)
**단일 책임 원칙**은 클래스가 **오직 하나의 목적이나 이유로만 변경**되어야 한다는 것을 강조합니다. 
여기서 “책임”이란 단순히 메서드의 개수를 뜻하지 않고, 특정 사용자나 기능 요구사항에 따라 소프트웨어의 변경 요청을 처리하는 역할을 의미합니다.
책임이 여러 개면 클래스가 커지고, 책임 간의 결합도가 높아져 요구사항의 변경이 일어났을 때 연쇄적인 변화로 이어질 수 있습니다.

즉, 클래스는 한 가지 변화의 이유만 가져야 하며, 이를 통해 변경이 발생했을 때 다른 기능에 영향을 덜 미치도록 설계됩니다. 이렇게 하면 유지보수가 쉬워지고 코드가 더 이해하기 쉬워집니다.

### 어떻게 이룰 수 있나요?
- **추상화**를 통해 객체를 설계하는 과정에서 적절한 한 개의 역할만 갖도록 구상
- 책임의 분배는 정답이 있는 것이 아니라 설계 상황에 따라 바뀔 수 있으므로 **경험**이 중요

---

## 2. 개방 폐쇄 원칙(Open-Closed Principle)
**개방 폐쇄 원칙**은 **확장에는 열려있고, 변경에는 닫혀 있어야 함**을 강조합니다.
이 때 '확장에 열려 있다'란 객체의 행위가 확장될 수 있다, 즉 행위를 추가해 객체가 하는 일을 바꿀 수 있다를 의미합니다. 

‘변경에 닫혀있다’는 객체의 확장이 소스 코드의 변경을 초래하지 않아야 한다는 것입니다.

OCP를 통해 기존 코드를 쉽게 확장할 수 있으므로 **유연성, 재사용성, 유지보수성**을 얻을 수 있습니다.

### 어떻게 지킬 수 있나요?
- **추상화**를 통해 지킬 수 있음
- 모듈은 잘 변하지 않는 추상화에 의존해 수정에는 닫혀있고 추상화의 구현체를 추가해 확장에는 열려있게 된다.

---

## 3. 리스코프 치환 원칙(Liskov Substitution Principle)
**리스코프 치환 원칙**은 **서브 타입은 언제나 상위 타입으로 교체할 수 있어야 한다**는 원칙입니다. 여기서 '상위 타입'이란, 자신의 기반 타입을 의미합니다.
이 말인 즉 하위 클래스가 상위 클래스로 바뀌어도 역할 수행에 문제가 없어야 한다는 말이고, 서브 타입은 기반 타입이 정해둔 약속을 지켜야 한다는 말과 동일합니다.

### 왜 지켜야 하나요?
리스코프 치환 원칙의 위반은 곧 개방 폐쇄 원칙의 위반이기 때문입니다. 개방 폐쇄 원칙은 상속을 통해 이뤄지고 리스코프 치환 원칙은 규약이 준수된 상속 구조를 보장합니다.
기반 타입으로 대체가 안되면 구현체가 들어왔을 때 (아무리 확장을 해봤자) 적절한 역할의 수행이 어렵게 됩니다.

### 어떻게 지킬 수 있나요?
- 하위 클래스 is-a 상위 클래스 (하위 클래스는 상위 클래스다)
- 구현 클래스 is able to 인터페이스 (구현 클래스는 인터페이스 할 수 있어야 한다)
- ⇒ 두 문장이 잘 지켜진다면 리스코프 치환 원칙이 잘 지켜진다는 신호

---

## 4. 인터페이스 분리 원칙(Interface Segregation Principle)
**인터페이스 분리 원칙**은 인터페이스는 **자신의 클라이언트가 사용할 메서드**만 가지고 있어야 한다는 원칙입니다.

사용하지 않지만 의존성을 가지고 있다면 해당 인터페이스가 변경되는 경우 영향을 받습니다. 따라서, 독립적인 개발과 배포가 불가합니다. 사용하는 기능만 제공하도록 인터페이스를 분리해 변경의 여파를 최소화할 수 있습니다.
즉, 클라이언트는 자신이 사용하지 않는 메서드에 대해선 알고 싶지 않으니 클라이언트 입장에서 인터페이스를 분리해야 함을 강조하는 것입니다.

### 왜 지켜야 하나요? (인터페이스가 비대해지면 왜 문제가 발생할까요?)
- ⇒ 같은 인터페이스를 구현하는 클라이언트 간의 결합도가 높아짐
- ⇒ 특정 클라이언트를 위한 메서드 추가 시, 다른 클라이언트는 구현이 강제되고 변경에 영향

### 어떻게 지킬 수 있나요?
- 자신의 클라이언트가 필요로하는 함수만 선언하면 됨
- ⇒ 인터페이스의 **역할에 충실한 기능만 제공**

---

## 5. 의존성 역전 원칙(Dependency Inversion Principle)
의존성 역전 원칙은 **고수준 모듈**은 **저수준 모듈**에 의존해서는 안되고, 둘 다 추상화에 의존해야 한다고 강조합니다.

SOLID는 서로 연관이 있는데요. 의존성 역전 원칙을 통해서 하위 레벨의 모듈은 개방 폐쇄 원칙을 준수하면서 새로운 타입을 추가할 수 있습니다.

### 왜 지켜야 하나요?
- **추상화**를 자유롭게 재사용하기 위함
  - 상위 객체는 애플리케이션의 본질을 담고 정책을 결정하는 역할
  - 하위 수준의 객체에 의존하면, 하위 수준의 변화가 상위 객체에 영향
  - 구체적인 것의 변화에 따라 규칙이 변한다 → 어색하게 느껴진다
  - 만약, 상위가 하위 객체에 의존을 하고 있다면 맥락에 맞게 변경해 재사용 불가
  - 즉, 추상화가 구체적인 것에 의존하면 추상화를 자유롭게 재사용할 수 없다

### 어떻게 지킬 수 있나요?
- 의존성은 이행적이다
  - ⇒ 한 레이어는 의존하고 있는 다른 레이어의 의존에도 의존하게 된다
    ![image](https://github.com/user-attachments/assets/c1dbffaa-79dc-4349-9284-13d3229193c8)

- 즉, 동일 레벨에서는 추상화를 의존하고, 하위 레벨이 상위 레벨을 의존하게 만들어 의존 관계를 역전
  ![image](https://github.com/user-attachments/assets/90b28e75-b095-429e-aead-c4cc1dcb644e)

---
### 꼬리 질문
- [SOLID 원칙 중 단일 책임 원칙(SRP)이 지켜지지 않으면 어떤 문제가 발생할 수 있나요? 이를 해결한 경험이 있다면 설명해 주세요.](https://github.com/kyungjinleelee/tech-interview/blob/main/cs/OOP/%EB%8B%A8%EC%9D%BC%20%EC%B1%85%EC%9E%84%20%EC%9B%90%EC%B9%99(SRP)%EC%9D%B4%20%EC%A7%80%EC%BC%9C%EC%A7%80%EC%A7%80%20%EC%95%8A%EC%95%98%EC%9D%84%20%EB%95%8C%20%EB%B0%9C%EC%83%9D%ED%95%A0%20%EC%88%98%20%EC%9E%88%EB%8A%94%20%EB%AC%B8%EC%A0%9C%EC%99%80%20%ED%95%B4%EA%B2%B0%20%EA%B2%BD%ED%97%98.md)
- [추상화(Abstraction)란 무엇이며, 코드로 어떻게 구현할 수 있는지 설명해주세요.](#)

### 참고 자료
- [모든 개발자가 알아야 할 SOLID의 진실 혹은 거짓](https://tech.kakaobank.com/posts/2411-solid-truth-or-myths-for-developers/)
- [[10분 테코톡] 클로버의 SOLID](https://www.youtube.com/watch?v=7c0tqHLfxlE)
- [클린 코더스 강의 12. SOLID Foundation](https://www.youtube.com/watch?v=HIWJ8sF8lO8)
