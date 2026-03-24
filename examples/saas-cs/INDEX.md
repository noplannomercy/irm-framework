# IRM 시뮬레이션: SaaS 고객 성공팀 (Customer Success)

> 도메인: SaaS 회사 — 고객 성공팀 (CS / Customer Success)
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

**SaaS 고객 성공팀**

SaaS 제품을 사용하는 고객의 온보딩, 사용량 모니터링, 갱신/해지, 문제 대응을 담당하는 팀.
고객 이탈 방지와 확장(upsell/renewal)이 핵심 목표.

**예상 행위 유형 분포**
- 탐색: 제품 매뉴얼, 버그 이력, 릴리즈 노트, 유사 사례
- 접근: CRM(고객 정보), 구독 관리 시스템, 사용량 모니터링
- 실행(확정): 구독 갱신/해지 처리, 인보이스 발행, 계정 정지·해제
- 실행(유연): 이탈 위기 고객 대응, VIP 온보딩, 복잡한 장애 에스컬레이션
