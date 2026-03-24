# Phase 0 Step 1 — 리소스 인벤토리

> 도메인: SaaS 고객 성공팀
> 단계: Phase 0 Step 1
> 상태: 검토 대기
> v2: 자체 평가 후 수정 (갭 5개 추가, 중복 항목 설명 명확화)

---

## 파일/문서

| # | 리소스명 | 유형 | 위치/접근방법 | 비고 |
|---|---------|------|-------------|------|
| 1 | 제품 사용 가이드 | 파일/문서 | Notion /product/guide | |
| 2 | 알려진 버그 및 이슈 이력 | 파일/문서 | Notion /cs/known-issues | 제품팀 관리 기술적 버그 목록 |
| 3 | 릴리즈 노트 | 파일/문서 | Notion /product/release-notes | |
| 4 | CS 대응 사례 모음 | 파일/문서 | Notion /cs/case-library | CS팀이 실제 처리한 티켓 해결 사례 |
| 5 | 고객 온보딩 가이드 | 파일/문서 | Notion /cs/onboarding | **고객에게 전달하는 자료** (↔ #16 내부 절차와 구분) |
| 6 | SLA 기준 문서 | 파일/문서 | Google Drive /contracts/sla | |
| 7 | 에스컬레이션 정책 | 파일/문서 | Notion /cs/escalation-policy | |
| 8 | 가격 정책·플랜별 기능 비교표 | 파일/문서 | Notion /product/pricing | 갱신 협상·업셀 시 필수 참조 |

## 시스템

| # | 리소스명 | 유형 | 위치/접근방법 | 비고 |
|---|---------|------|-------------|------|
| 9 | CRM (HubSpot) | 시스템 | HubSpot REST API | 고객 정보, 계약 현황, 커뮤니케이션 이력 |
| 10 | 구독 관리 (Chargebee) | 시스템 | Chargebee REST API | 구독 상태, 결제, 인보이스 |
| 11 | 사용량 모니터링 (자체 대시보드) | 시스템 | 내부 Analytics API | 기능별 사용 빈도, 로그인 이력 |
| 12 | 티켓 시스템 (Zendesk) | 시스템 | Zendesk REST API | |
| 13 | 이메일 시스템 (Gmail) | 시스템 | Gmail API | 고객과의 주요 소통 채널 |
| 14 | Slack | 시스템 | Slack API | 내부 알림 및 에스컬레이션 채널 |
| 15 | NPS/CSAT 조사 도구 (Typeform) | 시스템 | Typeform REST API | 고객 만족도 측정 결과 |
| 16 | 계정 관리 콘솔 (Admin API) | 시스템 | 내부 Admin REST API | 계정 정지·해제·권한 변경 |

## 절차/매뉴얼

| # | 리소스명 | 유형 | 위치/접근방법 | 비고 |
|---|---------|------|-------------|------|
| 17 | 신규 고객 온보딩 절차 | 절차/매뉴얼 | Notion /cs/runbook/onboarding | **CS팀 내부 실행 순서** (↔ #5 고객 전달 자료와 구분) |
| 18 | 구독 갱신/해지 처리 절차 | 절차/매뉴얼 | Notion /cs/runbook/subscription | |
| 19 | 계정 정지·해제 절차 | 절차/매뉴얼 | Notion /cs/runbook/account | |
| 20 | 이탈 위기 고객 대응 플레이북 | 절차/매뉴얼 | Notion /cs/playbook/churn | |
| 21 | 장애 에스컬레이션 절차 | 절차/매뉴얼 | Notion /cs/runbook/escalation | |
| 22 | 인보이스/청구 오류 처리 절차 | 절차/매뉴얼 | Notion /cs/runbook/billing | 빈번한 CS 케이스 |

## 외부 소스

| # | 리소스명 | 유형 | 위치/접근방법 | 비고 |
|---|---------|------|-------------|------|
| 23 | 고객사 IR/공시 자료 | 외부 소스 | 각사 IR 페이지, 공시 사이트 | 비즈니스 리뷰 준비 시 활용 |
| 24 | 업계 CS 벤치마크 리포트 | 외부 소스 | Gartner, Forrester 등 | |
| 25 | 경쟁사 제품 비교 정보 | 외부 소스 | 경쟁사 공개 사이트, G2/Capterra | 이탈 방어 시 필요 |
