# Docker와 VM(가상머신)의 차이

## 1. 핵심 개념

- **VM(가상 머신, Virtual Machine)**
  - 하드웨어 위에서 '하이퍼바이저(Hypervisor)'라는 가상화 소프트웨어를 통해 운영체제를 통째로 가상화하고, 그 위에 각자의 커널과 앱을 따로 실행하는 구조.
  - 각 VM은 완전히 독립된 OS 환경(Windows, Linux 등)을 가짐.

- **Docker(도커) - 컨테이너(Container) 기반 가상화**
  - 호스트 OS의 커널을 공유하는 '컨테이너 엔진' 위에서 필요한 라이브러리와 앱만 독립 공간에 올려 실행하는 방식.
  - 도커 컨테이너는 기본적으로 'OS 위에서 격리된 프로세스' 형태.

---

## 2. 구조적 차이

```mermaid
flowchart TD
    A[Physical Machine/Server]
    A --> B[Hypervisor]
    B --> C[VM OS 1] --> D[App 1]
    B --> E[VM OS 2] --> F[App 2]
    A -.-> G[Host OS] -.-> H[Docker Engine] -.-> I[Container 1 (App & Lib)]
    H -.-> J[Container 2 (App & Lib)]
```

- **VM 구조**: 하이퍼바이저 > 각 VM마다 커널/OS 별도 설치 > 앱 실행  
- **Docker 구조**: 도커 엔진 > 컨테이너마다 필요한 파일/라이브러리+앱만 격리 > 커널은 공유

---

## 3. 장단점 비교

|                   | VM                                       | Docker(컨테이너)                    |
|-------------------|------------------------------------------|-------------------------------------|
| 부팅 속도         | 느림 (수 분)                             | 매우 빠름(수 초 이내), 즉시 기동    |
| 리소스 사용량      | 높음 (각 OS마다 RAM, 디스크 점유)         | 적음 (커널 공유, 공간 경량)         |
| 격리 수준         | 완전 (각각의 커널로 강력한 보안)          | 파일 시스템/프로세스 격리 - 약간 낮음 |
| 설치 및 복제      | 복잡 (이미지 큼, 복제 느림)               | 빠름(docker image, docker pull)      |
| 호환성            | 다양한 OS(VM마다 자유롭게 구성)           | 동일 OS 커널 상에서 그에 맞게만 지원 |
| 용도              | 이기종 OS, Legacy 시스템, 완전한 분리 필요 | 빠른 배포, 테스트, 마이크로서비스    |

---

## 4. 대표 활용 예시

- **VM**  
  - Windows/Ubuntu 등 멀티 OS가 필수, 높은 보안/격리, Legacy 등
- **Docker**  
  - CI/CD, 마이크로서비스, 빠른 배포 & 롤백, 테스트 자동화

---

## 5. 결론

- 도커는 '경량화된 격리된 환경'을 빠르고 효율적으로 제공
- VM은 완벽한 OS 격리가 필요한 상황에서 필요
- 최근 DevOps, 마이크로서비스 구조에선 도커(컨테이너)가 대세

---

### 참고 자료

- [공식 Docker Docs - Docker vs Virtual Machines](https://docs.docker.com/get-started/overview/#docker-vs-virtual-machines)
- [Velog: Docker와 VM의 차이](https://velog.io/@lann-studio/Docker-VM-%EC%B0%A8%EC%9D%B4)
