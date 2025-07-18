## Gradle에 대해 설명해 주세요.
**Gradle**은 Java, Kotlin, Scala 등 **JVM에서 실행되는 언어**에서 자주 사용되는 **빌드 자동화 도구**입니다. 

기존의 Ant와 Maven의 단점을 보완하여 증분 빌드, 빌드 캐시, 데몬 프로세스를 활용해 빌드 속도를 최적화하고, 멀티 프로젝트를 쉽게 관리할 수 있도록 설계되었습니다. 

또한, 다양한 플러그인과 커스텀 태스크를 사용해 확장성을 높일 수 있으며, Groovy 또는 Kotlin DSL을 사용해 유연한 빌드 스크립트를 작성할 수 있습니다.

## 빌드 자동화 도구를 왜 사용할까요?
- 컴파일, 테스트, 패키징, 배포와 같은 반복 작업을 자동화하여 개발 생산성을 높일 수 있습니다.
- 일관된 빌드 환경을 제공하여 어떤 환경에서나 동일한 빌드 결과를 보장할 수 있습니다.
- 증분 빌드, 빌드 캐시, 병렬 처리 등을 통해 빌드 속도를 최적화할 수 있습니다.
- 테스트 누락 등 수동 작업 시 발생할 수 있는 휴먼 에러를 방지할 수 있습니다.
- 외부 라이브러리를 자동으로 관리하여 의존성 버전 충돌을 줄일 수 있습니다.
- CI/CD와 연동하여 빌드 후 패키징, 배포까지 연속적으로 처리할 수 있습니다.

## Dependency Configuration이 무엇이고 어떤 종류가 있을까요?
Dependency Configuration은 애플리케이션에 필요한 **의존성의 사용 범위를 정의**하는 설정입니다. 

사용 범위를 명확히 구분하는 이유는 빌드 성능 개선과 불필요한 의존성을 제거해서 빌드 결과물의 크기를 최적화하기 위함입니다.
- **implementation**은 컴파일 및 런타임 시점에 모두 필요한 의존성입니다. 해당 의존성이 현재 모듈에서만 필요할 때 사용하고, 일반적인 라이브러리나 프레임워크를 추가할 때 주로 사용합니다.
- **api**는 implementation과 비슷하지만, 다른 모듈에서도 접근할 수 있는 의존성을 정의할 때 사용합니다. 예를 들어, a -> b -> c의 의존성이 있을 때, a에서 c를 사용하려면 b에서 c를 api로 추가해야 합니다.
- **compileOnly**는 컴파일 시점에만 필요한 의존성으로, Lombok과 같은 라이브러리에서 사용합니다.
- **annotationProcessor**는 컴파일 시점에 실행되는 어노테이션 프로세서를 추가할 때 사용합니다. 예를 들어, MapStruct와 Lombok과 같이 컴파일 시점에 어노테이션을 기반으로 특정 프로세스를 수행하는 라이브러리에서 사용합니다.
- **runtimeOnly**는 런타임 시점에만 필요한 의존성입니다. 예를 들어, 데이터베이스 드라이버는 컴파일 시점에 필요하지 않지만, 애플리케이션이 실행될 때만 필요하므로 runtimeOnly로 설정합니다.
- **testImplementation, testCompileOnly, testRuntimeOnly**와 같이 'test'가 포함된 설정은 테스트 코드에서만 사용되는 의존성을 추가할 때 사용합니다.

---
### 꼬리 질문
- [Maven과 Gradle의 차이점을 설명해 주세요.](https://github.com/kyungjinleelee/tech-interview/blob/main/java/Maven%EA%B3%BC%20Gradle%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%84%20%EC%84%A4%EB%AA%85%ED%95%B4%20%EC%A3%BC%EC%84%B8%EC%9A%94.md)

### 참고 자료
- [[10분 테코톡] 루나의 Gradle](https://www.youtube.com/watch?v=ntOH2bWLWQs)
- [[10분 테코톡] 프린의 Gradle](https://www.youtube.com/watch?v=jSInFhiz1kY)
