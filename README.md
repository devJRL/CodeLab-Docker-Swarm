# Docker Swarm Tutorial

Study with '[**도커/쿠버네티스**를 활용한 컨테이너 개발 실전 입문 (위키북스)](https://github.com/wikibook/docker-kubernetes)'

## Key words

[클라우드 네이티브 아키텍처 디자인 백서 (REDHAT)](https://www.redhat.com/cms/managed-files/cl-cloud-native-container-design-whitepaper-f8808kc-201710-a4-ko.pdf)

### SOLID: 객체지향 설계 원칙 from Rovert C. Martin

- **S**ingle Responsibility: _단일 책임_
- **O**pen/Closed: _개방/퍠쇄_
- **L**iskov Subsitution: _리스코프 치환_
- **I**nterface Segregation: _인터페이스 분리_
- **D**ependency Inversion: _의존성 주입_

### Principle of Software Architectrue: 소프트웨어 설계 원칙

- **KISS** (Keep It Simple, stupid): _간단하고 쉽게 유지하기_
- **DRY** (Don't Repeat Yourself): _동일 반복 작업 하지 않기_
- **YAGNI** (You aren't gonna need it): _필요없는 기능 추가하지 않기_
- **SoC** (Seperation of Concerns): _관심사 분리하기_
- **SCP** (Single Concern Principle): _단일 관심사_

### Principle of Container based Application Architecture: 컨테이너 기반 애플리케이션 설계 원칙

- **SCP** (Single Concern Principle): _단일 관심사_
- **CIB** (Container Is Blackbox): _블랙박스로서의 컨테이너_
- **HOP** (High Observablity Principle): _효율적 관측_
- **LCP** (Life-cycle Conformance Principle): _라이프사이클 준수 원칙_
- **IIP** (Image Imunablity Principle): _이미지 불변성 원칙_
- **PDP** (Process Disposablity Principle): _프로세스 폐기 원칙_
- **S-CP**(Self Containment Principle): _독립성 원칙_
- **RCP** (Runtime Containent Principle): _런타임 견제원칙_

### Principle of Container for Best Practice: 모범 사례를 위한 컨테이너 원칙

- Build small image.
- Select strickly userid of sudoer.
- Can be set and exposed runtime ports by both of human and application.
- Must use persistance-data-volume.
- Write meta data for container.
- Concentrate concerns not child processes.
