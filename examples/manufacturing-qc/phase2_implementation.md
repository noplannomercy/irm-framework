# Phase 2 — 구현 체크리스트

> 도메인: 제조/품질관리
> 단계: Phase 2
> 상태: 완료
> 입력: Phase 1 전체 (1-A ~ 1-E)
> 기준: IRM Framework v10

---

## 탐색 구현 체크리스트

### RAG 구축 (기본)

- [x] **§1. 컬렉션 설계**
  - 4개 컬렉션: quality-standards, defect-history, vendor-quality, runbook
  - 벡터 저장소: pgvector (PostgreSQL 16)
  - 인덱스: HNSW
  - → 별첨: RAG 설계 방법론 §1

- [x] **§2. 컬렉션별 메타데이터 스키마**
  - 공통: `doc_type`, `product`, `date`, `source`
  - quality-standards: `inspection_type`, `process`, `spec_version`
  - defect-history: `defect_type`, `root_cause`, `severity`, `period`
  - vendor-quality: `vendor_name`, `eval_type`, `period`
  - runbook: `procedure_name`, `trigger_condition`, `defect_type`
  - → 별첨: RAG 설계 방법론 §2

- [ ] **§3. 문서 파싱 및 청킹**
  - quality-standards: Docling + HybridChunker (PDF 규격서, 테이블 구조 인식)
  - defect-history: 커스텀 전처리 (1 클레임 = 1 청크, CRM 필드 직접 추출)
  - vendor-quality: Docling (평가 리포트 PDF)
  - runbook: Docling (Word/Wiki → 섹션 단위)
  - → 별첨: RAG 설계 방법론 §3

- [ ] **§4. 임베딩 모델 선정**
  - 한국어 제조 도메인 → BGE-m3-ko 또는 KURE-v1 (로컬)
  - PoC: OpenAI text-embedding-3-small
  - → 별첨: RAG 설계 방법론 §4

- [ ] **§5. 검색 전략**
  - 하이브리드 검색 (벡터 + BM25), RRF 결합
  - 교차 컬렉션: quality-standards + defect-history (불량 관련 질의 시)
  - 메타데이터 필터링: product, process, defect_type 활용
  - → 별첨: RAG 설계 방법론 §5

- [ ] **§6. 리랭킹**
  - 적용 권장 — 교차 컬렉션 검색 + 불량 유형 다의어로 노이즈 예상
  - 로컬: BGE-reranker-v2-m3
  - → 별첨: RAG 설계 방법론 §6

- [x] **§7. 인덱싱 범위**
  - quality-standards: 최신형 (구버전 규격으로 검사하면 위험)
  - defect-history: 이력형 (최소 3년, severity=critical은 기간 무관)
  - vendor-quality: 이력형 (최소 2년)
  - runbook: 최신형
  - → 별첨: RAG 설계 방법론 §7

- [ ] **§8. 갱신 파이프라인**
  - quality-standards: 즉시 갱신 (규격 변경 → Webhook)
  - defect-history: 허용 갱신 (클레임 종결 → 일배치)
  - vendor-quality: 느린 갱신 (평가 완료 → 수동)
  - runbook: 즉시 갱신 (절차 변경 → Webhook)
  - → 별첨: RAG 설계 방법론 §8

### GraphRAG 승격 추가 항목

- [ ] **§9. GraphRAG 인프라 구성**
  - Apache AGE (pgvector와 동일 PostgreSQL 인스턴스)
  - → 별첨: GraphRAG 설계 방법론 §1

- [ ] **§10. 엔티티+트리플 추출 파이프라인**
  - 예상 엔티티: 불량, 원인, 공정, 설비, 자재, 제품, 검사, 규격
  - 예상 관계: 불량-[발생하다]→공정, 원인-[기인하다]→설비, 자재-[투입되다]→공정
  - LLM 자유 추출 방식 (gpt-4o-mini)
  - → 별첨: GraphRAG 설계 방법론 §2

- [ ] **§11. 그래프 검색 통합**
  - 멀티홉 2단계 (불량→원인→공정, 불량→공정→설비)
  - 컨텍스트 조립: [관련 문서] + [관련 관계 체인]
  - → 별첨: GraphRAG 설계 방법론 §3

- [ ] **§12. GraphRAG 갱신 파이프라인**
  - defect-history 갱신 시 벡터 + 그래프 동시 갱신
  - → 별첨: GraphRAG 설계 방법론 §4

### 탐색(외부)

- [ ] **§13~14. 외부 수집**
  - ISO/IATF 규격: 온디맨드 웹 검색
  - 품질 규제 동향: 온디맨드 웹 검색

---

## 접근 구현 체크리스트

- [x] **1. CLI 대상 확정**
  - ★★★ 즉시: MES, 품질DB, 설비 모니터링
  - ★★ 운영 중: ERP, LIMS, SPC
  - ★ 필요 시: DMS
  - → 별첨: CLI 구성요소 생성 런북

- [x] **2. CLI 경계 결정**
  - 시스템 단위 분리: `mes`, `quality`, `erp`, `equip`, `lims`, `spc`, `dms`
  - JSON 출력 표준화
  - 권한: 각 시스템 기존 인증 위임

- [ ] **3. REST 게이트웨이 설계**
  - FastAPI로 CLI 노출
  - 인증: 기존 사내 SSO 연동

- [ ] **4. CLI 구현**
  - → 별첨: CLI 구성요소 생성 런북

- [ ] **5. 통합 검증**
  - 검사 파이프라인 체이닝: ERP→LIMS→품질DB 순서 검증
  - 불량 대응 체이닝: 품질DB→MES→설비 모니터링 순서 검증

---

## 실행(확정) 구현 체크리스트

- [ ] **1. CLI 파이프라인형 설계**
  - 수입검사: `erp material-receipt` → `lims request-test` → `quality record-result`
  - 공정검사: `mes process-data` → `spc check-control` → `quality record-result`
  - 출하검사: `quality inspect-result` → `lims test-result` → `dms issue-coa`
  - 검사 성적서: `quality get-results` → `lims get-results` → `dms generate-coa`
  - → 별첨: CLI 구성요소 생성 런북

- [ ] **2. CLI 파이프라인형 구현 및 검증**
  - 단계별 성공/실패 분리
  - 합격/불합격 판정 로직 CLI 내장

---

## 실행(유연) 구현 체크리스트

- [x] **1. 구조 설계**
  - 불량 처리 (MRB): 접근(품질DB) → 탐색(유사 이력) → 판단(처분) → 실행(기록)
  - 불량 원인 분석: 접근(품질DB+MES+설비) → 탐색(유사 이력) → 판단(4M 분류)
  - CAPA: 접근(품질DB) → 탐색(가이드+사례) → 판단(시정책) → 실행(기록)
  - 공정 이상 대응: 접근(설비+MES) → 탐색(매뉴얼+이력) → 판단(조치) → 실행
  - human-in-the-loop: 처분 결정, 라인 정지, 특채 승인

- [x] **2. 구현 방식 결정**
  - 초기: 프롬프트 기반 (LLM 판단+실행)
  - 고도화: CLI 에이전트형 (판단은 LLM, 실행은 CLI)

- [ ] **3. 실행(확정) 전환 모니터링**
  - MRB: 동일 불량 유형 처분 패턴 3회 반복 → 실행(확정) 전환 검토
  - 공정 이상: 특정 이상 패턴 3회 반복 → 실행(확정) 전환 검토
  - 원인 분석/CAPA: 영구 유연 유지

---

## 라우팅 프롬프트 설계

- [ ] **1. 판단 트리 프롬프트화**
  - Q1/Q2/Q3 기반 분기
  - "불량" 질의 → 탐색+접근+실행(유연) 체이닝 자동 트리거

- [ ] **2. 체이닝 순서**
  - 1-E 맵 기반
  - 검사 절차: 순차 (접근→탐색→실행)
  - 불량 대응: 순차+분기 (판단 결과에 따라 경로 다름)

---

## 인프라 배치

- [ ] **1. 인프라 유형**
  - Type 1: 직접 구성 — PostgreSQL 16 (pgvector + AGE) + Ollama (로컬 임베딩)
  - 제조 환경 특성: 폐쇄망 가능성 → 로컬 LLM/임베딩 우선 검토

---

> **시뮬레이션 완료**
