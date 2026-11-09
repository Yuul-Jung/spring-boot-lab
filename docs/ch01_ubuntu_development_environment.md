# 📘 제01장 — 학습 테크트리 및 Ubuntu 개발환경 구축

> **실습 환경:** Ubuntu 24.04 LTS · IntelliJ IDEA · OpenJDK 21 · Spring Boot / Spring AI · Ollama · Docker · PostgreSQL/pgvector · Elasticsearch  
> **상태:** 학습안내서 작성 / 로컬 PC 실행은 아직 미검증  
> **방침:** 제공받은 강의자료는 참고용 샘플. 프로젝트·명령을 그대로 복제하지 않고, Ubuntu 전용 독립 실습으로 구성한다.

## 1. 과제 개요

이 장에서는 Spring AI 기반 Vector DB(벡터 데이터베이스)와 RAG(Retrieval-Augmented Generation, 검색 증강 생성)를 학습하기 위한 Ubuntu 환경을 준비한다.

- **입력:** Ubuntu PC, 터미널, 인터넷, 설치 전 점검 결과
- **출력:** Java 21·IntelliJ·Python·Docker·Ollama·DB 환경의 설치/구동 확인 기록
- **완료 기준:** 핵심 도구가 버전을 표시하고, Ollama API·컨테이너 DB에 정상 접속하며, IntelliJ에서 Java 프로젝트를 실행할 수 있다.
- **주의:** 이 장은 학습용 절차이다. 실제 PC에서 실행한 검증 결과로 오해하지 않는다.

## 2. 전체 학습 테크트리

| 장 | 제목 | 핵심 학습 및 산출물 |
|---:|---|---|
| 01 | 전체 목차와 Ubuntu 개발환경 구축 | 설치 목록·검증 로그 |
| 02 | IntelliJ에서 Spring Boot + Spring AI 시작 | HTTP 응답을 반환하는 실행 프로젝트 |
| 03 | 벡터·임베딩·코사인 유사도 | 유사도 계산 결과 |
| 04 | Ollama 로컬 LLM·임베딩 | 대화 응답·임베딩 배열 |
| 05 | 메모리 벡터 저장소 | 문장 유사도 검색 |
| 06 | PostgreSQL + pgvector | 영속화된 벡터 검색 |
| 07 | Elasticsearch 구성과 인덱스 | 색인·문서 검색 |
| 08 | Elasticsearch 벡터·하이브리드 검색 | BM25·kNN 비교 |
| 09 | 문서 추출·분할·메타데이터 | 문서 적재 파이프라인 |
| 10 | Spring AI RAG 파이프라인 | 근거 문서 기반 질의응답 |
| 11 | PDF RAG | PDF 검색과 출처 출력 |
| 12 | ERP 및 도메인 문서 RAG | 업무 데이터 질의응답 |
| 13 | 검색 품질·환각·출처 검증 | 검색 지표와 비교 리포트 |
| 14 | LlamaIndex와 Spring AI 비교 | Python 독립 RAG 실습 |
| 15 | RAG 서비스 통합·배포 | 최종 통합 프로젝트 |

**권장 진행:** 한 장의 검증을 끝낸 뒤 다음 장으로 넘어간다. LlamaIndex는 Python 계열 별도 프레임워크이므로 Spring AI의 필수 구성요소처럼 취급하지 않는다.

## 3. 개발환경 전체 구조

```text
Ubuntu 24.04 LTS
  ├─ Java 21 ─ Gradle Wrapper ─ IntelliJ IDEA
  │                             └─ Spring Boot + Spring AI
  ├─ Ollama
  │    ├─ 대화 모델 (LLM)
  │    └─ 임베딩 모델
  ├─ Docker Engine + Compose
  │    ├─ PostgreSQL + pgvector
  │    └─ Elasticsearch
  ├─ Python + venv
  │    └─ LlamaIndex (14장 별도 실습)
  └─ curl · Git · DBeaver(선택)
```

Spring AI는 Java 애플리케이션 계층이며 Ollama 모델 서버, 임베딩 모델, 벡터 저장소와 연결한다. LlamaIndex는 별도 Python 프로젝트에서 비교한다.

## 4. 필수·선택 설치 항목

| 항목 | 구분 | 설치/사용 시점 | 검증 기준 |
|---|---|---|---|
| OpenJDK 21 | 필수 | 1장 | `java -version` |
| IntelliJ IDEA | 필수 | 1장 | GUI 실행 |
| Git · curl | 필수 | 1장 | 버전 출력 |
| Gradle Wrapper | 필수(프로젝트 포함) | 2장 | `./gradlew --version` |
| Ollama | 필수 | 1장 | `ollama --version`, API |
| 대화·임베딩 모델 | 필수 | 1장 | `ollama list` |
| Docker Engine·Compose | 필수 | 1장 | `docker compose version` |
| PostgreSQL·pgvector | 실습 필수 | 6장 | SQL 질의 |
| Elasticsearch | 실습 필수 | 7장 | HTTP 상태·색인 |
| Python·venv | 확장 필수 | 14장 | 가상환경 실행 |
| LlamaIndex | 확장 필수 | 14장 | Python 모듈 임포트 |
| DBeaver | 권장 | 6장 | DB 접속 |
| Pinecone(클라우드) | 선택 | 보충 실습 | API 접속 |

**자원 관리:** PostgreSQL, Elasticsearch 및 여러 LLM을 한꺼번에 켜지 않는다. 필요한 실습 단계에서 해당 서비스를 시작한다.

## 5. 기존 설치 상태 조사

**설치하기 전에 먼저 아래 명령을 실행하고 결과를 기록한다.** 일부 명령이 `command not found`를 출력해도 점검 단계에서는 정상이다.

```bash
# 운영체제와 CPU/RAM 정보
lsb_release -a
uname -m
free -h

# Java / Python / Git / curl
java -version
python3 --version
git --version
curl --version

# 컨테이너 구성
docker --version
docker compose version

# 로컬 AI
ollama --version
ollama list

# GPU 상태(NVIDIA GPU가 있을 때만)
nvidia-smi

# IntelliJ 실행 파일 검색(설치 방식에 따라 다를 수 있음)
command -v intellij-idea || true
command -v idea || true
snap list 2>/dev/null | grep -i intellij || true
```

이미 설치되어 정상 동작하는 프로그램은 재설치하지 않는다.

## 6. 기본 도구 설치

```bash
# 패키지 목록 갱신
sudo apt update

# Java 21 개발 키트, Git, curl, 압축 및 Python 가상환경 도구 설치
sudo apt install -y \
  openjdk-21-jdk git curl unzip ca-certificates \
  python3 python3-venv python3-pip

# 설치 확인
java -version
javac -version
git --version
python3 --version
```

**기능:** `openjdk-21-jdk`는 Java 컴파일러·실행환경, `git`은 소스 버전 관리, `curl`은 REST API 확인, `python3-venv`는 프로젝트별 Python 의존성 격리에 사용한다.

## 7. IntelliJ IDEA 설치 및 첫 실행

기존 설치가 정상이라면 이 단계를 생략한다. Ubuntu에서 IntelliJ IDEA는 **JetBrains Toolbox(권장)** 또는 Snap 중 하나의 방식으로 설치한다.

**방법 A — JetBrains Toolbox:** [공식 설치 안내](https://www.jetbrains.com/help/idea/installation-guide.html)를 따라 Linux용 Toolbox를 설치하고, Toolbox에서 IntelliJ IDEA를 선택한다.

**방법 B — Snap:** Snap 사용 가능 환경에서 아래 명령으로 설치한다.

```bash
# IntelliJ IDEA 설치(기존 설치가 없다면)
sudo snap install intellij-idea --classic

# 실행
intellij-idea
```

**검증:** IntelliJ 시작 화면이 표시되어야 한다. 프로젝트의 JDK 설정에서 설치된 **JDK 21**을 선택할 수 있는지 확인한다. 무료 기능과 Ultimate 고급 Spring 지원 기능은 구분한다.

## 8. Ollama 설치 및 모델 준비

공식 안내: https://docs.ollama.com/linux

```bash
# 공식 설치 스크립트 다운로드
curl -fsSL https://ollama.com/install.sh -o /tmp/ollama-install.sh

# 스크립트를 열어 확인한 다음 실행
less /tmp/ollama-install.sh
sh /tmp/ollama-install.sh

# 서버 상태 및 버전 확인
ollama --version
systemctl status ollama --no-pager

# 임베딩 모델 다운로드
ollama pull bge-m3

# 대화형 모델 다운로드(예시: 장비 자원에 맞춰 변경 가능)
ollama pull qwen2.5:3b

# 설치된 모델 확인
ollama list
```

**검증:** Ollama API의 모델 목록을 조회한다.

```bash
curl -s http://localhost:11434/api/tags
```

**추가 검증:** 임베딩 API를 호출하여 숫자 배열이 반환되는지 확인한다.

```bash
curl -s http://localhost:11434/api/embed \
  -H 'Content-Type: application/json' \
  -d '{"model":"bge-m3","input":"Ubuntu에서 RAG를 학습한다."}'
```

실제 모델 선택은 사용 가능한 GPU 메모리·RAM에 맞추어 조정한다. 모델 다운로드에는 인터넷 연결과 저장 공간이 필요하다.

## 9. Docker Engine 및 Compose 설치

공식 Ubuntu 설치 가이드: https://docs.docker.com/engine/install/ubuntu/

**중요:** Docker는 기존 패키지 설치 상태와 충돌할 수 있으므로 먼저 `docker --version` 및 `dpkg -l | grep -E 'docker|containerd|podman'`으로 조사한다. 아래는 **새 설치**를 위한 공식 APT 저장소 방식이다.

```bash
# Docker 공식 저장소 서명키 등록
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Ubuntu 코드명과 아키텍처를 기준으로 저장소 등록
sudo tee /etc/apt/sources.list.d/docker.sources > /dev/null <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

# 패키지 설치
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin

# 설치 및 데몬 검증
sudo systemctl enable --now docker
sudo docker version
sudo docker compose version
sudo docker run --rm hello-world
```

**주의:** 기존 Docker/Podman 패키지 충돌이나 시스템 저장소 오류가 있으면 무리하게 제거하지 않고 먼저 원인을 조사한다. Docker 그룹 추가는 사실상 높은 권한을 부여하므로 이 학습안내서에서는 `sudo docker`를 기준으로 한다.

## 10. DB는 단계에 맞추어 구축

1장에서는 Docker 자체만 검증한다. PostgreSQL/pgvector 컨테이너 및 Elasticsearch 컨테이너는 각각 6장과 7장에서 버전·메모리 요구사항을 확인하고 Compose 파일로 실행한다.

**이유:** 두 서비스를 즉시 설치하면 초기 학습의 실패 원인을 분리하기 어렵고 시스템 자원을 불필요하게 점유한다.

## 11. IntelliJ에서 첫 Java 실행 확인

1. IntelliJ 실행 → **New Project** → **Java**.
2. JDK에서 **21** 선택.
3. 프로젝트명 `spring-ai-lab`, 빌드 시스템은 **Gradle** 선택.
4. `Main.java` 파일에 아래 코드 작성.

```java
// Java와 IntelliJ 개발환경 확인용 프로그램
public class Main {
    public static void main(String[] args) {
        System.out.println("Ubuntu Java 21 환경 확인 완료");
    }
}
```

5. 실행 버튼을 눌러 콘솔에 **Ubuntu Java 21 환경 확인 완료**가 출력되는지 확인한다.
6. GitHub에 업로드할 때 `.idea` 사용자별 설정·`.gradle` 캐시·`build` 출력물은 제외한다.

**주의:** 이 예제는 Java 실행 검증용이다. 실제 Spring Boot + Spring AI 프로젝트는 2장에서 구성한다.

## 12. 설치 결과 기록 및 다음 장 진입 기준

| 확인 항목 | 검증 명령/방법 | 기대 결과 | 실제 결과 |
|---|---|---|---|
| Ubuntu | `lsb_release -a` | Ubuntu 24.04 | 미검증 |
| Java | `java -version` | 21 | 미검증 |
| IntelliJ | GUI 실행 | 시작 화면 | 미검증 |
| Git | `git --version` | 버전 표시 | 미검증 |
| Docker | `sudo docker run --rm hello-world` | 성공 메시지 | 미검증 |
| Compose | `sudo docker compose version` | 버전 표시 | 미검증 |
| Ollama | `ollama --version` | 버전 표시 | 미검증 |
| 임베딩 | `/api/embed` 호출 | 벡터 배열 | 미검증 |
| 로컬 LLM | `ollama run qwen2.5:3b` | 텍스트 응답 | 미검증 |
| Java 프로젝트 | IntelliJ 실행 | 정해진 문자열 출력 | 미검증 |

**제1장 완료 조건:** 필수 항목이 모두 실제로 정상 동작하고, 검증 결과를 기록한 경우. 완료 이후 **제02장 — IntelliJ Spring Boot + Spring AI 프로젝트 시작**으로 진행한다.

## 13. 주요 오류 점검

| 증상 | 우선 확인 |
|---|---|
| `java: command not found` | JDK 설치 및 PATH |
| IntelliJ에서 JDK 21 미표시 | Project SDK 설정 |
| `docker: permission denied` | `sudo docker`로 재검증 |
| Docker 서비스 중지 | `sudo systemctl status docker` |
| `ollama` API 연결 거부 | `systemctl status ollama`, 11434 포트 |
| 모델 다운로드 실패 | 네트워크·디스크 공간·모델명 |
| 응답 속도 저하 | `nvidia-smi`, RAM, CPU/GPU 사용률 |

## 14. 핵심 정리

- IntelliJ IDEA와 Java 21은 Spring Boot 실습의 기본 개발환경이다.
- Ollama는 로컬 AI 모델 실행, Docker는 데이터베이스 실행을 담당한다.
- PostgreSQL/pgvector와 Elasticsearch는 동일한 벡터 DB가 아니라 각기 다른 검색 기능과 운영 특성을 비교할 대상이다.
- LlamaIndex는 Spring AI의 필수 라이브러리가 아니라 Python 비교·확장 실습 대상이다.
- **1장에서는 기존 설치 조사 → 필요한 항목 설치 → 실제 실행 검증을 완료한다.**
- 비밀번호, 토큰, 개인 경로, 내부 IP와 비공개 문서는 GitHub에 게시하지 않는다.
