# IRM 시뮬레이션: 재무/구매 승인

> 도메인: 사내 재무팀 — 구매 요청·승인·지급 처리
> 시작일: 2026-03-18
> 목적: IRM Framework v7→v8 단계별 시뮬레이션 및 검증
> 진행 규칙: 각 단계 완료 후 반드시 리뷰 → 승인 후 다음 단계 진행

---

## 진행 상태

| 단계 | 파일 | 상태 |
|------|------|------|
| Phase 0 Step 1 — 리소스 인벤토리 | `phase0_step1_resource_inventory.md` | ✅ 완료 |
| Phase 0 Step 2 — 리소스 성격 분류 | `phase0_step2_classification.md` | ✅ 완료 |
| Phase 0 Step 3 — 질의 매핑 (IRM 매핑표) | `phase0_step3_query_mapping.md` | ✅ 완료 |
| Phase 1 1-A — 탐색 배치 | `phase1_1a_search.md` | ✅ 완료 |
| Phase 1 1-B — 접근 배치 | `phase1_1b_access.md` | ✅ 완료 |
| Phase 1 1-C — 실행(확정) 배치 | `phase1_1c_exec_fixed.md` | ✅ 완료 |
| Phase 1 1-D — 실행(유연) 배치 | `phase1_1d_exec_flexible.md` | ✅ 완료 |
| Phase 1 1-E — 체이닝 관계 맵 | `phase1_1e_chaining.md` | ✅ 완료 |
| Phase 2 — 구현 체크리스트 | `phase2_implementation.md` | ✅ 완료 |

---

## 도메인 설명

**재무/구매 승인**

사내 구매 요청 발생 시 재무팀과 구매팀이 수행하는 일련의 프로세스.
구매 요청, 견적 비교, 결재 승인, 발주, 검수, 지급까지 전 과정을 포함한다.

**예상 행위 유형 분포**
- 탐색: 구매 정책, 벤더 이력, 계약 조건, 유사 구매 사례
- 접근: ERP, 전자결재, 법인카드 시스템, 회계 시스템, 벤더 DB
- 실행(확정): 구매 요청 접수, 발주서 발행, 지급 처리, 예산 차감
- 실행(유연): 벤더 선정, 견적 비교·협상, 예산 초과 대응, 감사 대응
