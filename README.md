<img src="https://capsule-render.vercel.app/api?type=waving&color=0:E34C26,10:DA5B0B,30:C6538C,75:3572A5,100:A371F7&height=100&section=header&text=&fontSize=0" width="100%"/>

## 강동현 · 백엔드 신입 엔지니어

Spring Boot 인증 서버와 Kubernetes 기반 실행 환경을 함께 구성하며, **인증 위임 · 시크릿 관리 · 쿼리 튜닝** 을 중심으로 백엔드 운영 흐름을 학습해 온 신입 백엔드 엔지니어입니다.  
Keycloak · Vault · K3s 기반 인증 / 시크릿 관리 구조를 구성하고, PostgreSQL 쿼리 튜닝 프로젝트에서 로컬 실험 기준 조회 시간을 **1.77 초 → 7.66 ms** 로 개선한 경험이 있습니다.

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
![Traefik](https://img.shields.io/badge/-Traefik-24A1C1?style=flat-square&logo=traefikproxy&logoColor=white)
![Vault](https://img.shields.io/badge/-Vault-FFEC6E?style=flat-square&logo=vault&logoColor=black)
![Keycloak](https://img.shields.io/badge/-Keycloak-4D4D4D?style=flat-square&logo=keycloak&logoColor=white)

주력: **Java / Spring Boot / Spring Security** — 인증 설계, API 개발, 쿼리 최적화  
확장: **K3s / Traefik / Vault / Keycloak** — Ingress 단 인증 차단, 시크릿 파이프라인

---

## 주요 프로젝트

### Keycloak 기반 인증 플랫폼 — 인증 서버 + Kubernetes 인프라 구성

> 개인 프로젝트 · 2025.11 ~ 현재 · 설계 / 구현 / 로컬 검증  
> [인증 서버](https://github.com/donghyeon-ka/project-auth-server) · [Kubernetes 인프라 구성](https://github.com/donghyeon-ka/project-infra)

인증 구현에서 끝내지 않고, **인증 위임 → Ingress 단 차단 → 시크릿 파이프라인까지 한 사이클을 직접 학습 / 구성**하기 위해 시작했습니다.

**인증 서버**

- 로그인을 직접 구현하지 않고 Keycloak에 인증을 위임하는 구조로 설계 (Spring Security ResourceServer + OAuth2)
- 여러 소셜 로그인 분기 코드를 Keycloak 단일 연동으로 통합 → 인증 관련 코드 **50% 이상 감소** (provider별 파싱 클래스 제거 기준)
- "어디까지가 Spring Security 책임이고, 어디부터가 Keycloak 책임인지" 경계를 명확히 분리

**인프라 구성**

- **Ingress 단 인증 차단 구조** — Traefik ForwardAuth + oauth2-proxy + Keycloak 조합으로 미인증 요청을 게이트에서 차단. 백엔드는 로그인 플로우를 직접 처리하지 않고, Spring Security Resource Server 가 Keycloak JWT 의 서명 / 만료 / issuer 를 다시 검증하는 구조로 분리
- **Vault + VSO 시크릿 파이프라인** — KV-v2 값을 K8s Secret 으로 자동 동기화. VSO 의 ServiceAccount 1 개를 Vault role 2 개 (`auth-platform` / `storage`) 로 분리해 도메인별 최소 권한 보장 → auth 토큰이 유출돼도 storage secret 은 보호
- **default-deny NetworkPolicy 베이스라인** — 단일 namespace 안에서도 모든 Pod 간 트래픽 차단 후 컴포넌트별로 명시 허용. 새 워크로드 추가 시 NetworkPolicy 도 같이 작성하지 않으면 통신 자체가 안 됨 (구조적 강제)
- **K3s packaged Traefik 무수정 정책** — K3s 가 재부팅마다 덮어쓰는 packaged manifest 에 손대지 않고 `HelmChartConfig` overlay 로만 운영 설정 관리 → 운영 정책이 Git 에서 사라지지 않음
- **kustomize build / kubeconform / kube-linter 3 단 검증 스크립트 구성** — 주요 overlay 에 대해 빌드 / 스키마 / 안티패턴을 단일 스크립트로 점검

`Spring Boot` `Spring Security` `Keycloak` `K3s` `Traefik` `oauth2-proxy` `Vault` `VSO` `Kustomize`

---

### PostgreSQL 쿼리 성능 최적화 — 1.77초 → 7.66ms (231배 개선)

> 개인 프로젝트 · 1인 · 2025.03 ~ 2025.04 · 쿼리 분석/최적화/검증 담당  
> [feed-postgresql-query-tuning](https://github.com/donghyeon-ka/feed-postgresql-query-tuning)

피드 서비스의 목록 조회 쿼리 응답 시간이 **1.77초**로 느린 원인을 분석하고 단계적으로 개선한 프로젝트입니다 (모든 수치는 EXPLAIN ANALYZE 기준 쿼리 실행 시간).

- **원인 분석**: 4-table LEFT JOIN으로 인한 데이터 폭증 + 인덱스 미사용 + 외부 정렬 발생 확인 (EXPLAIN ANALYZE)
- **1차 개선**: 불필요한 JOIN을 EXISTS 서브쿼리로 교체 + 쿼리 분리 → **1.77초 → 0.216초** (8배)
- **2차 개선**: 복합 인덱스 + 커버링 인덱스로 디스크 접근 최소화 → **0.216초 → 7.66ms** (28배)
- **캐시 전략**: Redis 2단계 캐시 (공개 피드만 캐시) 도입 → 캐시 히트율 **90%+** 달성
- **부하 검증**: 단일 인스턴스 환경에서 Locust로 동시 1,000명 부하 테스트 수행 → **900~1,100 RPS** 달성

`PostgreSQL` `Redis` `Spring Boot` `JPA` `Locust` `HikariCP`

---

## 블로그 · 기술 글

기술적 의사결정 과정과 트러블슈팅 경험을 기록합니다 — [Velog @donghyeon2](https://velog.io/@donghyeon2)

---

## 연락처

[![Email](https://img.shields.io/badge/-donghyeon.kang.dev@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:donghyeon.kang.dev@gmail.com)
[![GitHub](https://img.shields.io/badge/-donghyeon-ka-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/donghyeon-ka)
[![Velog](https://img.shields.io/badge/-donghyeon2-20C997?style=flat-square&logo=velog&logoColor=white)](https://velog.io/@donghyeon2)

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:E34C26,10:DA5B0B,30:C6538C,75:3572A5,100:A371F7&height=40&section=footer&text=&fontSize=0" width="100%"/>
