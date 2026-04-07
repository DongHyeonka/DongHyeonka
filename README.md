<img src="https://capsule-render.vercel.app/api?type=waving&color=0:E34C26,10:DA5B0B,30:C6538C,75:3572A5,100:A371F7&height=100&section=header&text=&fontSize=0" width="100%"/>

## 강동현 · 백엔드 엔지니어

**인증/인가 설계부터 배포 자동화까지 한 사이클을 직접 구축**해 본 백엔드 엔지니어입니다.  
Keycloak · Vault · K3s로 인증 플랫폼을 만들고, 쿼리 튜닝으로 API 응답 시간을 231배 개선한 경험이 있습니다.

---

## 기술 스택

![Java](https://img.shields.io/badge/-Java-007396?style=flat-square&logo=java&logoColor=white)
![Spring Boot](https://img.shields.io/badge/-Spring%20Boot-6DB33F?style=flat-square&logo=spring-boot&logoColor=white)
![Spring Security](https://img.shields.io/badge/-Spring%20Security-6DB33F?style=flat-square&logo=spring-security&logoColor=white)
![JPA](https://img.shields.io/badge/-JPA%2FHibernate-59666C?style=flat-square&logo=hibernate&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/-Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-K3s-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![ArgoCD](https://img.shields.io/badge/-Argo%20CD-EF7B4D?style=flat-square&logo=argo&logoColor=white)
![Vault](https://img.shields.io/badge/-Vault-FFEC6E?style=flat-square&logo=vault&logoColor=black)
![Keycloak](https://img.shields.io/badge/-Keycloak-4D4D4D?style=flat-square&logo=keycloak&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white)

주력: **Java / Spring Boot / Spring Security** — 인증 설계, API 개발, 쿼리 최적화  
확장: **K3s / Argo CD / Vault** — GitOps 배포 자동화, 시크릿 관리

---

## 주요 프로젝트

### Keycloak 기반 인증 플랫폼 — 인증 서버 + GitOps 운영 인프라

> 개인 프로젝트 · 2025.11 ~ 현재 · 설계/구현/운영 전담  
> [인증 서버](https://github.com/DongHyeonka/Project-Auth-Server) · [운영 인프라](https://github.com/DongHyeonka/Project-Auth-GitOps)

인증 구현에서 끝내지 않고, **인증 위임 → 배포 자동화 → 시크릿 관리까지 한 사이클을 직접 경험**하기 위해 시작했습니다.

**인증 서버**

- 로그인을 직접 구현하지 않고 Keycloak에 인증을 위임하는 구조로 설계 (Spring Security + OAuth2)
- 여러 소셜 로그인 분기 코드를 Keycloak 단일 연동으로 통합 → 인증 관련 코드 **50% 이상 감소** (provider별 파싱 클래스 제거 기준)
- "어디까지가 Spring Security 책임이고, 어디부터가 Keycloak 책임인지" 경계를 명확히 분리

**운영 인프라**

- 코드 push만 하면 자동 배포되는 GitOps 파이프라인 구축 → 배포 과정을 **수동 5단계 → 자동 1단계**로 축소 ([변경 이력](https://github.com/DongHyeonka/Project-Auth-GitOps#readme))
- 앱 비밀번호(DB, API 키 등)를 코드에 넣지 않고 Vault가 실행 시점에 자동 주입하는 구조로 전환 → **Git에 시크릿을 두지 않는 구조** 확보
- Vault를 2대로 분리하여 "잠금 해제용"과 "비밀번호 저장용" 보안 경계를 나눔
- 반복되는 수동 세팅을 스크립트화하여 **초기 구축 30분 → 자동 5분**으로 단축 (runbook + bootstrap 스크립트 기준)
- 아키텍처 변경마다 "이전 구조 → 문제 → 개선"을 [README에 누적 기록](https://github.com/DongHyeonka/Project-Auth-GitOps#readme) (현재 10+ cycle)

`Spring Boot` `Keycloak` `K3s` `Argo CD` `Vault` `GitHub Actions` `Kustomize`

---

### PostgreSQL 쿼리 성능 최적화 — 1.77초 → 7.66ms (231배 개선)

> 개인 프로젝트 · 1인 · 2025.03 ~ 2025.04 · 쿼리 분석/최적화/검증 담당  
> [feed-postgresql-query-tuning](https://github.com/DongHyeonka/feed-postgresql-query-tuning)

피드 서비스의 목록 조회가 **1.77초**로 느린 원인을 분석하고 단계적으로 개선한 프로젝트입니다.

- **원인 분석**: 4-table LEFT JOIN으로 인한 데이터 폭증 + 인덱스 미사용 + 외부 정렬 발생 확인 (EXPLAIN ANALYZE)
- **1차 개선**: 불필요한 JOIN을 EXISTS 서브쿼리로 교체 + 쿼리 분리 → **1.77초 → 0.216초** (8배)
- **2차 개선**: 복합 인덱스 + 커버링 인덱스로 디스크 접근 최소화 → **0.216초 → 7.66ms** (28배)
- **캐시 전략**: Redis 2단계 캐시 (공개 피드만 캐시) 도입 → 캐시 히트율 **90%+** 달성
- **부하 검증**: Locust로 동시 1,000명 부하 테스트 수행 → **900~1,100 RPS** 달성

`PostgreSQL` `Redis` `Spring Boot` `JPA` `Locust` `HikariCP`

---

## 블로그 · 기술 글

[![Velog](https://img.shields.io/badge/Velog-20C997?style=flat-square&logo=velog&logoColor=white)](https://velog.io/@donghyeon2)
기술적 의사결정 과정과 트러블슈팅 경험을 기록합니다.

<!-- 대표 글이 정해지면 아래에 추가
- [글 제목](링크) — 한 줄 요약
- [글 제목](링크) — 한 줄 요약
-->

---

## 연락처

[![Email](https://img.shields.io/badge/-donghyeon.kang.dev@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:donghyeon.kang.dev@gmail.com)
[![GitHub](https://img.shields.io/badge/-DongHyeonka-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/DongHyeonka)
[![Velog](https://img.shields.io/badge/-donghyeon2-20C997?style=flat-square&logo=velog&logoColor=white)](https://velog.io/@donghyeon2)

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:E34C26,10:DA5B0B,30:C6538C,75:3572A5,100:A371F7&height=40&section=footer&text=&fontSize=0" width="100%"/>
