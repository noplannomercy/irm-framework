# IRM 시뮬레이션: 제조기업 — 검사 절차 자동화

> 도메인: 중견 제조기업 — 검사 절차 자동화 (유즈케이스 2)
> 시작일: 2026-07 (파일럿 후 횡확산)
> 입력: ALFA Phase 4 유즈케이스 정의서 #2
> 기준: IRM Framework v10

---

## 진행 상태

| 단계 | 파일 | 상태 |
|------|------|------|
| Phase 0 Step 1 | `phase0_step1_resource_inventory.md` | ✅ 완료 |
| Phase 0 Step 2 | `phase0_step2_classification.md` | ✅ 완료 |
| Phase 0 Step 3 | `phase0_step3_query_mapping.md` | ✅ 완료 |
| Phase 1 1-A | `phase1_1a_search.md` | ✅ 완료 — GraphRAG **불필요** (0/4) |
| Phase 1 1-B | `phase1_1b_access.md` | ✅ 완료 — 4개 시스템 |
| Phase 1 1-C | `phase1_1c_exec_fixed.md` | ✅ 완료 — **핵심** (검사 3종 + 성적서) |
| Phase 1 1-D | `phase1_1d_exec_flexible.md` | ✅ 해당 없음 |
| Phase 1 1-E | `phase1_1e_chaining.md` | ✅ 완료 |
| Phase 2 | `phase2_implementation.md` | ✅ 완료 |

---

## 유즈케이스 1 대비

| 항목 | 유즈케이스 1 (불량 분석) | 유즈케이스 2 (검사 절차) |
|------|----------------------|----------------------|
| 핵심 행위 | 탐색(내부) | **실행(확정)** |
| GraphRAG | 필수 (3/4) | **불필요 (0/4)** |
| 실행(유연) | 있음 | 없음 |
| 실행(확정) | 없음 | **있음 (4개)** |
| RAG | 신규 구축 | **재활용** |
| CLI | 2개 | **4개** |
| 구축 시간 | 3개월 | **한 달 반** |

> **같은 도메인, 같은 고객, 같은 인프라인데 IRM 결과가 완전히 다르다.**
