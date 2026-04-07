<img src="https://capsule-render.vercel.app/api?type=waving&color=0:E34C26,10:DA5B0B,30:C6538C,75:3572A5,100:A371F7&height=100&section=header&text=&fontSize=0" width="100%"/>

## Kang Dong Hyeon · Backend Engineer

Spring Boot 기반의 **인증/인가 아키텍처 설계**와 **GitOps 기반 인프라 자동화**에 집중하고 있습니다.  
Keycloak, HashiCorp Vault, K3s를 활용한 프로덕션 수준의 보안 인프라를 직접 설계하고 운영합니다.

> *"동작하는 코드"를 넘어 **"운영 가능한 시스템"** 을 만드는 데 관심이 있습니다.*

---

## 🛠 Tech Stack

**Backend**&nbsp;&nbsp;&nbsp;&nbsp;
![Java](https://img.shields.io/badge/-Java-007396?style=flat-square&logo=java&logoColor=white)
![Spring Boot](https://img.shields.io/badge/-Spring%20Boot-6DB33F?style=flat-square&logo=spring-boot&logoColor=white)
![Spring Security](https://img.shields.io/badge/-Spring%20Security-6DB33F?style=flat-square&logo=spring-security&logoColor=white)
![JPA](https://img.shields.io/badge/-JPA%2FHibernate-59666C?style=flat-square&logo=hibernate&logoColor=white)
![Gradle](https://img.shields.io/badge/-Gradle-02303A?style=flat-square&logo=gradle&logoColor=white)

**Infrastructure**&nbsp;&nbsp;&nbsp;&nbsp;
![Kubernetes](https://img.shields.io/badge/-K3s%2FKubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![ArgoCD](https://img.shields.io/badge/-Argo%20CD-EF7B4D?style=flat-square&logo=argo&logoColor=white)
![Helm](https://img.shields.io/badge/-Helm-0F1689?style=flat-square&logo=helm&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white)

**Security**&nbsp;&nbsp;&nbsp;&nbsp;
![Keycloak](https://img.shields.io/badge/-Keycloak-4D4D4D?style=flat-square&logo=keycloak&logoColor=white)
![Vault](https://img.shields.io/badge/-HashiCorp%20Vault-FFEC6E?style=flat-square&logo=vault&logoColor=black)

**Database**&nbsp;&nbsp;&nbsp;&nbsp;
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/-Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

**Observability**&nbsp;&nbsp;&nbsp;&nbsp;
![Pinpoint](https://img.shields.io/badge/-Pinpoint-03C75A?style=flat-square&logoColor=white)
![Locust](https://img.shields.io/badge/-Locust-24B47E?style=flat-square&logoColor=white)

---

## 🚀 Featured Projects

### [Project-Auth-Server](https://github.com/DongHyeonka/Project-Auth-Server)
> **Keycloak 기반 OAuth2 인증/인가 서버** · 운영 인프라: [Project-Auth-GitOps](https://github.com/DongHyeonka/Project-Auth-GitOps)

- Keycloak을 IdP로 활용한 OAuth2 인증/인가 아키텍처 설계 (Spring Security 연동)
- IdP 응답 파싱을 Keycloak 단일 제공자로 통합 — 불필요한 provider 분기 제거
- Spring Security FilterChain과 Keycloak 간 인증 책임 경계 설계

`Spring Boot` `Spring Security` `Keycloak` `OAuth2` `PostgreSQL` `JPA`

---

### [Project-Auth-GitOps](https://github.com/DongHyeonka/Project-Auth-GitOps)
> **GitOps 기반 K3s 인프라 · CD 파이프라인 · Secret Lifecycle 관리** · 대상: [Project-Auth-Server](https://github.com/DongHyeonka/Project-Auth-Server)

- CD 중심 GitOps 저장소 — apps/infra 선언 분리, Kustomize base/overlay 구조로 환경별(dev/prod) 확장 설계
- 2-Vault 구조 Transit Auto-Unseal — vault-transit provider가 workload Vault를 자동 unseal, trust boundary 분리
- Vault Agent Injector + Kubernetes Auth 기반 secret 주입 — SealedSecret에서 Vault KV로 전환, 워크로드별 최소 권한 role 분리
- Bootstrap / Reconcile 분리 — 최초 1회 수동 runbook, 이후 self-hosted runner로 자동 reconcile (AppRole 기반)
- 앱 repo CI → `repository_dispatch` → GitOps repo 이미지 태그 자동 갱신 → Argo CD 배포 반영
- Cycle 기반 아키텍처 변경 이력 누적 — 구조/문제/개선/트러블슈팅을 README에 체계적으로 기록

`K3s` `Argo CD` `Vault` `Vault Agent Injector` `Terraform` `GitHub Actions` `Kustomize` `NetworkPolicy` `SealedSecrets`

---

### [feed-postgresql-query-tuning](https://github.com/DongHyeonka/feed-postgresql-query-tuning)
> **PostgreSQL 쿼리 성능 최적화 — 1.77초 → 7.66ms (231배 개선)**

- 4-table LEFT JOIN의 카테시안 곱 문제를 EXISTS 서브쿼리 + 쿼리 분리 전략으로 해결
- EXPLAIN ANALYZE 기반 쿼리 플랜 분석 — Seq Scan → Index Scan 전환, 외부 정렬 제거
- Redis 2단계 캐시 전략 도입 — visibility 기반 캐시 분할로 캐시 히트율 90%+ 달성
- Locust 부하 테스트로 검증 (900~1100 RPS), HikariCP 커넥션 풀 튜닝

`PostgreSQL` `Redis` `Spring Boot` `JPA` `Locust` `HikariCP`

---

### [Local-MicroService-Practice](https://github.com/DongHyeonka/Local-MicroService-Practice)
> **로컬 Kubernetes 환경에서의 MSA 운영 실습**

- 로컬 K8s 클러스터에서 마이크로서비스 배포 및 서비스 간 통신 구성
- Pinpoint APM 연동으로 분산 트레이싱 및 성능 모니터링
- 동시접속자 1,000명 기준 부하 테스트 수행

`Kubernetes` `Docker` `Pinpoint` `Spring Boot`

---

## 📝 Blog & Writing

[![Velog](https://img.shields.io/badge/Velog-20C997?style=flat-square&logo=velog&logoColor=white)](https://velog.io/@donghyeon2)
기술적 의사결정 과정과 트러블슈팅 경험을 기록합니다.

---

## 📊 GitHub Stats

<a href="https://github.com/anuraghazra/github-readme-stats">
<img src="https://github-readme-stats.vercel.app/api?username=DongHyeonka&show_icons=true&theme=material-palenight&hide_border=true&bg_color=20232a&icon_color=58A6FF&text_color=fff&title_color=58A6FF&count_private=true" width=53% />
</a>
<a href="https://github.com/anuraghazra/github-readme-stats">
<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=DongHyeonka&layout=donut&show_icons=true&theme=material-palenight&hide_border=true&bg_color=20232a&icon_color=58A6FF&text_color=fff&title_color=58A6FF&count_private=true&exclude_repo=Face-Transfer-Application" width=41% />
</a>

## 🏆 Algorithm

[![Solved.ac Profile](http://mazassumnida.wtf/api/v2/generate_badge?boj=ehdgus6435)](https://solved.ac/ehdgus6435/)

---

## 📫 Contact

[![Email](https://img.shields.io/badge/-donghyeon.kang.dev@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:donghyeon.kang.dev@gmail.com)
[![GitHub](https://img.shields.io/badge/-DongHyeonka-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/DongHyeonka)
[![Velog](https://img.shields.io/badge/-donghyeon2-20C997?style=flat-square&logo=velog&logoColor=white)](https://velog.io/@donghyeon2)

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:E34C26,10:DA5B0B,30:C6538C,75:3572A5,100:A371F7&height=40&section=footer&text=&fontSize=0" width="100%"/>
