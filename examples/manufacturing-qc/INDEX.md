# IRM 시뮬레이션: 제조/품질관리

> 도메인: 중견 제조기업 — 품질관리팀 (QC/QA)
> 시작일: 2026-03-24
> 목적: IRM Framework v10 단계별 시뮬레이션 및 검증
> 진행 규칙: 각 단계 완료 후 반드시 리뷰 → 승인 후 다음 단계 진행

---

## 진행 상태

| 단계 | 파일 | 상태 |
|------|------|------|
| Phase 0 Step 1 — 리소스 인벤토리 | `phase0_step1_resource_inventory.md` | ✅ 완료 |
| Phase 0 Step 2 — 리소스 성격 분류 | `phase0_step2_classification.md` | ✅ 완료 |
| Phase 0 Step 3 — 질의 매핑 (IRM 매핑표) | `phase0_step3_query_mapping.md` | ✅ 완료 |
| Phase 1 1-A — 탐색 배치 | `phase1_1a_search.md` | ✅ 완료 — **GraphRAG 승격 대상** (3/4 충족) |
| Phase 1 1-B — 접근 배치 | `phase1_1b_access.md` | ✅ 완료 |
| Phase 1 1-C — 실행(확정) 배치 | `phase1_1c_exec_fixed.md` | ✅ 완료 |
| Phase 1 1-D — 실행(유연) 배치 | `phase1_1d_exec_flexible.md` | ✅ 완료 |
| Phase 1 1-E — 체이닝 관계 맵 | `phase1_1e_chaining.md` | ✅ 완료 |
| Phase 2 — 구현 체크리스트 | `phase2_implementation.md` | ✅ 완료 |

---

## 도메인 설명

**제조/품질관리**

중견 제조기업의 품질관리팀이 수행하는 일련의 프로세스.
수입검사, 공정검사, 출하검사, 불량 대응, 품질 개선, 설비 관리까지 전 과정을 포함한다.

**예상 행위 유형 분포**
- 탐색: 검사 기준서, 불량 이력, 공정 매뉴얼, SPC 데이터, 규격서
- 접근: MES, 품질 DB, 설비 모니터링, ERP, LIMS
- 실행(확정): 수입검사 절차, 출하 판정, 검사 성적서 발행
- 실행(유연): 불량 원인 분석, 시정조치(CAPA), 공정 이상 대응

**핵심 특성**
- GraphRAG 승격 대상 (3/4 조건 충족) — "불량" 다의어 + 인과 체인 탐색
- 접근+탐색+실행(유연) 3단 체이닝이 핵심 패턴
- 제조 환경 폐쇄망 가능성 → 로컬 LLM/임베딩 우선
