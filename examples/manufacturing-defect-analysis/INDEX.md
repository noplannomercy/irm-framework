# IRM 시뮬레이션: 제조기업 — 품질 불량 원인 분석

> 도메인: 중견 제조기업 — 품질 불량 원인 분석 (유즈케이스 한정)
> 시작일: 2026-03-25
> 목적: ALFA → IRM 풀코스 연결 검증
> 입력: ALFA Phase 4 유즈케이스 정의서 (docs/ALFA/hist/manufacturing-company/)
> 기준: IRM Framework v10

---

## 진행 상태

| 단계 | 파일 | 상태 |
|------|------|------|
| Phase 0 Step 1 — 리소스 인벤토리 | `phase0_step1_resource_inventory.md` | ✅ 완료 |
| Phase 0 Step 2 — 리소스 성격 분류 | `phase0_step2_classification.md` | ✅ 완료 |
| Phase 0 Step 3 — 질의 매핑 | `phase0_step3_query_mapping.md` | ✅ 완료 |
| Phase 1 1-A — 탐색 배치 | `phase1_1a_search.md` | ✅ 완료 — **GraphRAG 승격 대상** (3/4) |
| Phase 1 1-B — 접근 배치 | `phase1_1b_access.md` | ✅ 완료 |
| Phase 1 1-C — 실행(확정) | `phase1_1c_exec_fixed.md` | ✅ 해당 없음 |
| Phase 1 1-D — 실행(유연) | `phase1_1d_exec_flexible.md` | ✅ 완료 |
| Phase 1 1-E — 체이닝 관계 맵 | `phase1_1e_chaining.md` | ✅ 완료 |
| Phase 2 — 구현 체크리스트 | `phase2_implementation.md` | ✅ 완료 |

---

## manufacturing-qc 대비 차이

| 항목 | manufacturing-qc (이전) | manufacturing-defect-analysis (이번) |
|------|----------------------|-------------------------------------|
| 범위 | 품질관리 전체 | **불량 원인 분석에 한정** |
| 입력 | IRM 직접 시뮬레이션 | **ALFA 유즈케이스 정의서** |
| 리소스 | 22개 | **7개** |
| 시스템 | 7개 (MES, ERP, LIMS...) | **2개 (품질DB, MES)** |
| 실행(확정) | 4개 (검사 3종 + 성적서) | **없음** |
| 실행(유연) | 5개 | **2개** |
| 질의 | 25개 | **13개** |

> ALFA가 유즈케이스를 좁혀서 넘기니까 IRM 결과도 집중된다. 이게 ALFA→IRM 핸드오프의 효과.

---

## 핵심 특성

- 탐색(내부)이 압도적 (13개 질의 중 8개)
- GraphRAG 승격 대상 (3/4) — 동음이의어 + 인과 체인
- 실행(확정) 없음 — "분석" 유즈케이스라서
- 온프레미스 전용 — 로컬 LLM + pgvector + AGE
- 추가 비용 0 — 기존 A100 + PostgreSQL
