# Phase 2 — 구현 체크리스트

> 도메인: 이커머스 — CS 상담사 보조
> 단계: Phase 2
> 상태: 완료
> 기준: IRM Framework v10

---

## 탐색 구현 체크리스트

- [x] **§1. 컬렉션 설계**
  - 단일 컬렉션: cs-knowledge (FAQ + 정책 + 배송 안내)
  - 데이터 소량이라 분리 불필요
  - 벡터DB: OpenSearch Serverless 또는 RDS PostgreSQL 신규 (pgvector)
  - 인덱스: HNSW

- [x] **§2. 메타데이터 스키마**
  - `doc_type` (faq / policy / shipping)
  - `faq_category` (교환/환불/배송/적립금 등)
  - `policy_type` (교환/환불/반품)
  - `condition` (개봉/미개봉, 할인/정가, 기간 내/외)
  - `shipping_type` (국내/해외)

- [ ] **§3. 문서 파싱 및 청킹**
  - FAQ: 1문항 = 1청크
  - 정책 문서: Docling (Word/PDF → 조항 단위)
  - 배송 안내: 텍스트 그대로

- [ ] **§4. 임베딩 모델**
  - Bedrock Titan Embeddings 또는 OpenAI text-embedding-3-small
  - 소량 데이터라 API 비용 미미

- [ ] **§5. 검색 전략**
  - 벡터 검색 단독으로 충분 (소량)
  - 메타데이터 필터: doc_type, condition
  - top-k: 3~5

- [x] **§6. 리랭킹**
  - 미적용 — 소량 데이터, 단일 컬렉션, 벡터 검색으로 충분

- [x] **§7. 인덱싱 범위**
  - 전부 최신형 — FAQ, 정책, 배송 안내 모두 최신 버전만

- [ ] **§8. 갱신 파이프라인**
  - 정책 변경 시 수동 재인덱싱 (소량이라 배치 불필요)
  - FAQ 추가 시 1건 추가 인덱싱

- [x] **GraphRAG: 불필요 (0/4)**

---

## 접근 구현 체크리스트

- [ ] **1. CLI 대상**
  - `shop` — 상품 정보, 재고, 입고 예정 (어드민 DB)
  - `delivery` — 배송 조회 (택배사 API)

- [ ] **2. CLI 경계**
  - 시스템 단위: `shop`, `delivery`
  - JSON 출력

- [ ] **3. REST 게이트웨이**
  - FastAPI (AWS 내부)

- [ ] **4. CLI 구현**
  - `shop product-info --sku`, `shop stock --sku`, `shop restock-date --sku`
  - `delivery track --tracking`
  - → 별첨: CLI 구성요소 생성 런북

- [ ] **5. 통합 검증**
  - "교환 되나요? + 재고 있어요?" 체이닝 검증
  - 탐색(정책) → 접근(어드민) 순서

---

## 실행(확정) — 해당 없음
## 실행(유연) — 해당 없음

---

## 인프라 배치

```
AWS (기존)
├── Bedrock (LLM — Claude/Titan)
├── 벡터DB (신규 — OpenSearch Serverless 또는 RDS PostgreSQL+pgvector)
├── RDS MySQL (기존 — 쇼핑몰 어드민 DB)
├── FastAPI (REST 게이트웨이)
└── CLI: shop, delivery
```

신규 구축: 벡터DB 1개 + CLI 2개. 나머지 기존 활용.

---

## 전체 케이스 비교

| 항목 | 불량분석 (제조1) | 검사절차 (제조2) | **CS보조 (이커머스)** |
|------|---------------|---------------|-------------------|
| 핵심 행위 | 탐색+실행(유연) | 실행(확정) | **탐색+접근** |
| GraphRAG | 필수 | 불필요 | **불필요** |
| 컬렉션 | 3개 | 재활용 | **1개** |
| CLI | 2개 | 4개 | **2개** |
| 실행 | 유연 | 확정 4개 | **없음** |
| 복잡도 | 높음 | 중간 | **낮음** |

> **가장 단순한 IRM 케이스.** 탐색+접근만, 실행 없음, 단일 컬렉션, GraphRAG 불필요.
> 고객 대면 전환 시 복잡도가 올라감 — 그때 IRM 재돌리기.

---

> **ALFA → IRM 풀코스 완료.**
