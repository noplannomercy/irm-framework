# Phase 2 — 구현 체크리스트

> 도메인: 제조기업 — 품질 불량 원인 분석
> 단계: Phase 2
> 상태: 완료
> 입력: Phase 1 전체
> 기준: IRM Framework v10
> 인프라 제약: 온프레미스 필수 (ALFA Phase 3 결정)

---

## 탐색 구현 체크리스트

### RAG 구축 (기본)

- [x] **§1. 컬렉션 설계**
  - 3개 컬렉션: quality-docs, defect-history, equipment-docs
  - 벡터 저장소: PostgreSQL + pgvector (기존 인프라)
  - 인덱스: HNSW

- [x] **§2. 메타데이터 스키마**
  - 공통: `doc_type`, `product`, `date`
  - quality-docs: `inspection_type`, `process`, `equipment`
  - defect-history: `defect_type`, `root_cause`, `process`, `period`
  - equipment-docs: `equipment`, `equipment_type`

- [ ] **§3. 문서 파싱 및 청킹**
  - quality-docs: Docling + HybridChunker (PDF 검사 기준서)
  - equipment-docs: Docling (혼합 형태)
  - defect-history: 품질 DB에서 텍스트 추출 (1건 = 1청크, 메타데이터는 DB 필드 직접)

- [ ] **§4. 임베딩 모델**
  - **로컬 필수** (온프레미스)
  - BGE-m3-ko (한국어 특화, 로컬, A100에서 구동)

- [ ] **§5. 검색 전략**
  - 하이브리드 (벡터 + BM25), RRF 결합
  - 교차 컬렉션: defect-history + quality-docs (불량 관련 질의 시)
  - 메타데이터 필터: product, process, defect_type

- [ ] **§6. 리랭킹**
  - 적용 권장 — 표현 비통일 문제 ("버 발생" vs "성형 불량")
  - BGE-reranker-v2-m3 (로컬, A100)

- [x] **§7. 인덱싱 범위**
  - quality-docs: 최신형 (구버전 기준서 제거)
  - defect-history: 이력형 (3년+)
  - equipment-docs: 이력형

- [ ] **§8. 갱신 파이프라인**
  - quality-docs: 기준 변경 시 재인덱싱 (수동 트리거)
  - defect-history: 불량 발생 시 품질 DB → 자동 인덱싱 (배치)

### GraphRAG 승격 (3/4 충족 — 승격 대상)

- [x] **§9. GraphRAG 인프라** ← 파일럿 후 승격 실행 (2026-07)
  - Apache AGE (기존 PostgreSQL에 확장 설치)
  - pgvector + AGE 동일 인스턴스
  - 승격 근거: 파일럿에서 "버 발생" vs "성형 불량" 표현 비통일 문제 확인

- [x] **§10. 엔티티+트리플 추출** ← 승격 실행
  - 엔티티: 불량, 원인, 공정, 설비, 제품, 자재
  - 관계: 불량-[발생하다]→공정, 원인-[기인하다]→설비, 불량-[유사하다]→불량
  - LLM 자유 추출 (Ollama 로컬 LLM)

- [x] **§11. 그래프 검색 통합** ← 승격 실행
  - 멀티홉 2단계 (불량→원인→공정, 불량→공정→설비)
  - 컨텍스트 조립: [관련 문서] + [관련 관계 체인]

- [x] **§12. GraphRAG 갱신** ← 승격 실행
  - defect-history 갱신 시 벡터 + 그래프 동시 갱신

---

## 접근 구현 체크리스트

- [x] **1. CLI 대상**
  - ★★★ 즉시: 품질 DB (`quality defect-list --lot {lot}`)
  - ★★ 운영 중: MES (`mes process-status --process {id}`)

- [x] **2. CLI 경계**
  - 시스템 단위: `quality`, `mes`
  - JSON 출력

- [ ] **3. REST 게이트웨이**
  - 사내망 내부 FastAPI (온프레미스)

- [ ] **4. CLI 구현**
  - → 별첨: CLI 구성요소 생성 런북

- [ ] **5. 통합 검증**
  - 품질DB → 탐색 → LLM 판단 체이닝 검증

---

## 실행(확정) — 해당 없음

이 유즈케이스 범위 밖.

---

## 실행(유연) 구현 체크리스트

- [x] **1. 구조 설계**
  - 불량 원인 분석: 접근(품질DB+MES) → 탐색(이력+기준서+매뉴얼) → LLM 판단 → 사람 확정
  - human-in-the-loop: 원인 후보 제시까지 자동, 최종 확정은 사람

- [x] **2. 구현 방식**
  - 초기: 프롬프트 기반 (LLM이 판단)
  - 고도화: CLI 에이전트형 (판단은 LLM, 접근은 CLI)

- [ ] **3. 전환 모니터링**
  - 불량 원인 분석: 영구 유연 유지
  - 불량 요약 리포트: 정형화 시 확정 전환 가능

---

## 인프라 배치 (ALFA Phase 3 결정 반영)

```
A100 서버 (기존)
├── Ollama (LLM 서빙 — Llama/Mistral 70B급)
├── BGE-m3-ko (임베딩, 로컬)
├── BGE-reranker-v2-m3 (리랭킹, 로컬)
├── PostgreSQL (기존)
│   ├── pgvector (벡터 검색)
│   ├── Apache AGE (GraphRAG)
│   └── 품질 DB (기존 — 불량 이력)
└── FastAPI (사내망 REST 게이트웨이)
```

추가 구매: 없음. 소프트웨어 설치만.

---

> **ALFA → IRM 풀코스 완료.**
> ALFA Phase 4 유즈케이스 정의서 → IRM Phase 0~2 구현 체크리스트까지.
