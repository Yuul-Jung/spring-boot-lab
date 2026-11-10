# Spring Boot AI · Vector DB · RAG — Ubuntu 실습 안내서

Ubuntu 24.04 LTS에서 **IntelliJ IDEA + Java 21 + Spring Boot / Spring AI**를 기반으로 벡터 검색과 RAG를 단계적으로 구현합니다.

> **상태**: 제1장 학습안내서 게시 완료 / 실제 개인 Ubuntu PC 설치·실행 검증은 미진행.
>
> **원본**: 제공받은 `VectorDB와RAG.zip`(Spring AI Java 예제 13개). 원본 코드는 아직 업로드하지 않았습니다.

## 학습안내서

- [제01장 — 전체 학습 테크트리 및 Ubuntu 개발환경 구축](docs/ch01_ubuntu_development_environment.md)
- [제02장 — IntelliJ에서 Spring Boot + Spring AI 시작 (HTML)](docs/ch02_spring_boot_spring_ai.html)
- [제03장 — 벡터·임베딩·코사인 유사도 (HTML)](docs/ch03_vector_embedding_cosine.html)
- [제04장 — Ollama 로컬 LLM과 임베딩 모델 (HTML)](docs/ch04_ollama_local_llm_embedding.html)
- [제05장 — 메모리 벡터 검색 (HTML)](docs/ch05_memory_vector_search.html)
- [제06장 — PostgreSQL·pgvector 영속 벡터 검색 (HTML)](docs/ch06_postgresql_pgvector.html)
- [제07장 — Elasticsearch 기초 및 인덱스 (HTML)](docs/ch07_elasticsearch_basics.html)
- [제08장 — Elasticsearch 벡터·하이브리드 검색 (HTML)](docs/ch08_elasticsearch_vector_hybrid.html)
- [제09장 — RAG 문서 적재·분할·메타데이터 (HTML)](docs/ch09_document_ingestion.html)
- [제10장 — Spring AI 문서 검색 및 답변 생성 (HTML)](docs/ch10_spring_ai_rag.html)
- [제11장 — PDF 기반 RAG 구현 (HTML)](docs/ch11_pdf_rag.html)
- [제12장 — ERP 및 도메인 문서 RAG (HTML)](docs/ch12_erp_domain_rag.html)
- [제13장 — 검색 평가·출처 검증·성능 개선 (HTML)](docs/ch13_search_evaluation.html)

## 과정 목차

| 장 | 주제 | 상태 |
|---:|---|---|
| 01 | 전체 학습 테크트리 및 Ubuntu 개발환경 구축 | **안내서 작성** |
| 02 | Spring Boot 프로젝트 구성과 Spring AI 연결 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 03 | 벡터·임베딩·코사인 유사도 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 04 | Ollama 로컬 LLM과 임베딩 모델 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 05 | 메모리 벡터 검색 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 06 | PostgreSQL·pgvector 구축과 유사도 검색 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 07 | Elasticsearch 기초 및 인덱스 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 08 | Elasticsearch 벡터·하이브리드 검색 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 09 | RAG 문서 적재·분할·메타데이터 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 10 | Spring AI 문서 검색 및 답변 생성 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 11 | PDF 기반 RAG 구현 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 12 | ERP 및 도메인 문서 RAG | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 13 | 검색 평가·출처 검증·성능 개선 | **HTML 학습안내서 게시 / 사용자 실행 미검증** |
| 14 | LlamaIndex(Python)와 Spring AI 비교 | 예정 |
| 15 | 통합 RAG 서비스 구축 및 배포 | 예정 |

### 원칙

- 기존 Java 예제 보존, 별도 Ubuntu 이식본에서 최소 수정
- 필수/권장/선택 도구 구분; 기존 설치 여부부터 조사
- 각 장마다 입력·출력, 구조, 실습, 검증, 오류 점검 명시
- 실제 실행 결과와 예상 결과를 구별
- 비밀번호·API 키·토큰·개인 경로·자료 원본을 공개 저장소에 올리지 않음

공식 문서: [Spring AI](https://docs.spring.io/spring-ai/reference/) · [IntelliJ IDEA](https://www.jetbrains.com/help/idea/installation-guide.html) · [Ollama](https://ollama.com/download/linux) · [Docker Ubuntu](https://docs.docker.com/engine/install/ubuntu/) · [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
