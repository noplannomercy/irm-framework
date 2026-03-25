# Phase 2 — 구현 체크리스트

> 도메인: 제조기업 — 검사 절차 자동화
> 단계: Phase 2
> 상태: 완료
> 입력: Phase 1 전체
> 기준: IRM Framework v10
> 인프라: 온프레미스 (ALFA Phase 3, 기존 인프라 재활용)

---

## 탐색 구현 체크리스트

- [x] **§1~§8: 기존 RAG 인프라 재활용**
  - 유즈케이스 1에서 구축한 quality-docs 컬렉션 그대로 사용
  - 검사 기준서 100개 이미 인덱싱됨
  - 추가 구축 불필요

- [x] **GraphRAG: 불필요**
  - 승격 판단 0/4

---

## 접근 구현 체크리스트

- [ ] **1. CLI 대상 확정**
  - ★★★: 품질 DB (`quality`), MES (`mes`), ERP (`erp`)
  - ★★: LIMS (`lims`) — 유무 확인 후
  - 품질 DB CLI는 유즈케이스 1에서 이미 있음 — 쓰기 커맨드 추가
  - → 별첨: CLI 구성요소 생성 런북

- [ ] **2. CLI 경계**
  - 시스템 단위: `quality`, `mes`, `erp`, `lims`
  - JSON 출력
  - `quality record-result` — **쓰기 CLI 추가** (유즈케이스 1에는 없었음)

- [ ] **3. REST 게이트웨이**
  - 사내망 FastAPI (기존 확장)

- [ ] **4. CLI 구현**
  - 신규: `erp material-receipt`, `lims request-test`, `lims test-result`, `quality record-result`
  - 기존: `quality defect-list`, `mes process-status` (유즈케이스 1)

- [ ] **5. 통합 검증**
  - 수입검사 파이프라인: ERP → LIMS → 품질DB 순서 검증
  - 공정검사 파이프라인: MES → 품질DB 순서 검증
  - 출하검사 파이프라인: 품질DB → LIMS → 품질DB 기록 검증

---

## 실행(확정) 구현 체크리스트

- [ ] **1. CLI 파이프라인형 설계**
  - 수입검사: `erp material-receipt` → RAG 기준서 → `lims request-test` → 판정 → `quality record-result`
  - 공정검사: `mes process-data` → RAG 기준서 → 판정 → `quality record-result`
  - 출하검사: `quality inspect-result` → `lims test-result` → RAG 기준서 → 판정 → `quality record-result`
  - 검사 성적서: `quality get-results` → `lims get-results` → PDF 생성

- [ ] **2. CLI 파이프라인형 구현 및 검증**
  - 합격/불합격 판정 로직: 기준서 수치와 실측값 대조 (자동)
  - 단계별 성공/실패 분리
  - 실패 시 어디서 막혔는지 JSON으로 반환

---

## 실행(유연) — 해당 없음

---

## 인프라 배치 (기존 재활용)

```
A100 서버 (기존 — 유즈케이스 1과 공유)
├── Ollama (LLM — 파이프라인 제어용)
├── BGE-m3-ko (임베딩 — 기존)
├── PostgreSQL (기존)
│   ├── pgvector (벡터 검색 — 기존 컬렉션 재활용)
│   ├── Apache AGE (GraphRAG — 유즈케이스 1 전용)
│   └── 품질 DB (기존)
├── FastAPI (REST 게이트웨이 — 기존 확장)
└── 신규 CLI: erp, lims, quality record-result
```

> 신규 구축: CLI 3~4개. 나머지 전부 기존 인프라 재활용.

---

## 유즈케이스 1 대비 비교

| 항목 | 유즈케이스 1 (불량 분석) | 유즈케이스 2 (검사 절차) |
|------|----------------------|----------------------|
| RAG 구축 | 신규 (§1~§8) | **재활용** |
| GraphRAG | 승격 (§9~§12) | **불필요** |
| CLI | 2개 (quality, mes) | **4개 (+ erp, lims)** |
| 핵심 체크리스트 | 탐색 14항목 | **접근 5항목 + 실행(확정) 2항목** |
| 구축 시간 | 3개월 | **한 달 반** (인프라 있으니까) |

---

> **ALFA → IRM 유즈케이스 2 풀코스 완료.**
