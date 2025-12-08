# 🐳 Docker Analysis & PoC Project
> **현업 표준 컨테이너 기술인 Docker의 도입 배경, 아키텍처, 산업별 성과를 분석하고 유사 OSS(Podman, LXC 등)와 비교**

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)]()
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)]()

---

## 📅 프로젝트 개요 (Project Overview)

- **주제**: 컨테이너 가상화 플랫폼 **Docker** 심층 분석 및 PoC
- **발표일**: 2025년 12월 12일 (금)
- **목표**:
  1. Docker의 기술적 원리(Namespace, Cgroups)와 VM 대비 차별점 분석
  2. 실제 기업(PayPal, Visa 등)의 도입 성과 정량적 확인
  3. Podman, LXC 등 유사 기술과의 비교를 통한 비판적 시각 도출
  4. Docker를 활용한 웹 서버 구축 PoC(Proof of Concept) 수행

---

## 👥 팀원 및 역할 분담 (Team Members)

| 이름 | 역할 | 담당 파트 | GitHub |
|:---:|:---:|:---|:---:|
| ㅇㅇㅇ | 자료조사 | 무엇을하였는지작성해주세요 | 깃허브주소를적어주세요  |
| 최현우 | 자료조사 | 활용및 성과, 아키텍쳐분석 및 시각적 다이어그램 표 | https://github.com/hwc2000 |
| ㅇㅇㅇ | 구현 | 무엇을하였는지작성해주세요 | 깃허브주소를적어주세요 |
| 김재훈 | 발표 자료 제작 | ppt제작 | https://github.com/edd1e-kim |
| ㅇㅇㅇ | 발표 | 무엇을하였는지작성해주세요 | 깃허브주소를적어주세요 |

---

## 📖 1. 도입 배경 및 필요성 (Background)

### 1.1 기존 가상화(VM)의 한계
- **무거운 오버헤드**: Hypervisor 위에 Guest OS를 통째로 올리는 방식은 부팅 속도가 느리고 리소스 낭비가 심함.
- **환경 불일치 문제**: *"내 컴퓨터에서는 되는데 서버에서는 안 돼요(Works on my machine)"* 현상 빈번.

### 1.2 Docker의 해결책: 컨테이너 격리
- **Linux Kernel 기술 활용**: `Cgroups`(자원 할당)와 `Namespace`(공간 격리)를 사용하여 OS 수준의 가상화 구현.
- **이미지(Image) 기반 배포**: 코드, 런타임, 시스템 도구, 라이브러리를 모두 포함하여 **어디서나 100% 동일한 실행 환경** 보장.
- **성과**: 빠른 부팅 속도, 가벼운 용량, 이식성 극대화.

> *"화물을 규격화된 컨테이너에 넣어 운송 효율을 혁신한 것처럼, 소프트웨어를 컨테이너화하여 개발 및 배포 효율을 혁신함."*

---

## 🏗️ 2. 기술적 아키텍처 (Architecture)

*(이곳에 아키텍처 다이어그램 이미지를 추가하면 좋습니다. 예: `![Architecture](./images/architecture.png)`)*

### 2.1 주요 구성 요소
- **Docker Daemon (dockerd)**: 컨테이너 생성, 실행, 이미지 관리를 수행하는 백그라운드 프로세스.
- **Docker Client**: 사용자의 명령(`docker build`, `docker run`)을 받아 데몬에 전달.
- **Docker Registry (Hub)**: 컨테이너 이미지를 저장하고 공유하는 저장소.
- **Containerd**: OCI 표준을 준수하는 컨테이너 런타임 (현재 Docker 엔진의 핵심).

---

## 🏢 3. 산업 활용 사례 및 성과 (Case Studies)

주요 글로벌 기업들은 Docker와 Kubernetes 오케스트레이션을 도입하여 구체적인 비용 절감 및 생산성 향상을 달성했습니다.

| 기업 | 산업 | 도입 효과 (정량적 성과) |
|:---:|:---:|:---|
| **PayPal** | 핀테크 | • 배포 시간 **90% 단축**<br>• 인프라 비용 **약 3,000만 달러 절감** |
| **Visa** | 금융 | • 배포 주기: 수 주(Weeks) → **수 시간(Hours)**<br>• 마이크로서비스 가용성 및 확장성 확보 |
| **MetLife** | 보험 | • 인프라 비용 **30% 절감**<br>• 신규 서비스 Time-to-Market 단축 |

---

## ⚖️ 4. 유사 기술 비교 분석 (Alternatives)

단순히 Docker가 최고라는 관점이 아닌, 목적에 따른 최적의 도구를 선택하기 위해 비교 분석을 수행했습니다.

| 구분 | **Docker** | **Podman** | **LXC/LXD** |
|:---:|:---|:---|:---|
| **핵심 컨셉** | 올인원 컨테이너 플랫폼 | **Daemonless** & Rootless | **시스템 컨테이너** (VM 대체) |
| **아키텍처** | 클라이언트-서버 (Daemon 필수) | 데몬 없음 (Fork/Exec 모델) | OS 전체 가상화에 초점 |
| **보안** | Root 권한 필요 (Rootless 모드 존재하나 복잡) | **기본적으로 Rootless** (보안 우수) | OS 레벨 격리 |
| **주 용도** | 일반적인 개발, CI/CD 표준 | 보안이 중요한 리눅스 서버 환경 | 멀티 OS 운영, 호스팅 |
| **호환성** | 산업 표준 | Docker CLI와 명령어 호환 (`alias docker=podman`) | Linux 커널 의존성 높음 |

**💡 결론**
- **Docker**: 일반적인 개발, 학습, CI/CD 파이프라인 구축 등 **범용적인 활용**에 최적.
- **Podman**: 보안이 중요한 **리눅스 서버**나 **Rootless 환경**이 필수적인 경우.
- **LXC/LXD**: 애플리케이션보다는 **OS 전체를 격리**하여 VM처럼 다루고 싶을 때.
- **containerd**: **Kubernetes 프로덕션 환경**이나 고성능이 요구되는 **클라우드 인프라** 구축 시 표준 런타임으로 선택.

---

## 💻 5. 프로토타입 (PoC) 구현

<br>
<pre>
      ##         .
## ## ##        ==
 ## ## ## ## ##    ===
 /"""""""""""""""""\___/ ===
{                       /  ===-
 \______ O           __/
   \    \         __/
    \____\_______/
