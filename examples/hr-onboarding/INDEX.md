# IRM 시뮬레이션: 신규 직원 온보딩 (HR)

> 도메인: 사내 HR — 신규 직원 온보딩
> 시작일: 2026-03-17
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

**신규 직원 온보딩**

입사자가 발생했을 때 HR, IT, 각 부서가 수행하는 일련의 프로세스.
계정 생성, 장비 배포, 교육 이수, 부서 배정, 정책 안내 등을 포함한다.

**예상 행위 유형 분포**
- 탐색: 온보딩 정책 문서, 부서별 교육 자료
- 접근: HR 시스템, IAM/계정 관리, 장비 자산 DB
- 실행(확정): 계정 생성 절차, 표준 장비 배포, 입사 안내 메일 발송
- 실행(유연): 부서별 맞춤 온보딩, 특수 직군(개발자/디자이너) 환경 세팅
