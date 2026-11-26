# 🚀 부하테스트 (study)

## 📌 프로젝트 소개

Spring Boot와 PostgreSQL을 활용하여 백엔드 서비스의 성능을 측정하고 개선하는 실습 프로젝트입니다.

k6를 활용한 부하 테스트로 API 성능을 측정하고, PostgreSQL의 실행 계획을 분석하여 데이터베이스 병목 구간을 파악합니다.

이후 인덱스 및 쿼리 최적화를 적용하고, 개선 전후의 성능을 비교하여 실제 개선 효과를 검증합니다.

**핵심 목표: 성능 측정 → 병목 분석 → 최적화 → 재측정**

## 🎯 학습 목표

- k6를 활용한 API 부하 테스트 수행
- 주요 성능 지표(RPS, Response Time, p95, Error Rate) 분석
- PostgreSQL 인덱스 구조 및 동작 원리 이해
- EXPLAIN ANALYZE를 활용한 SQL 실행 계획 분석
- 단일 인덱스와 복합 인덱스 비교 및 설계
- OFFSET과 Keyset 페이징 성능 비교
- 성능 개선 전후의 측정 결과 비교 및 문서화

## 🛠️ 기술 스택

| 구분 | 기술 | 활용 목적 |
|---|---|---|
| Backend | Java, Spring Boot | REST API 구현 |
| Database | PostgreSQL | 데이터 저장 및 조회 |
| ORM | Spring Data JPA | 데이터 접근 |
| Load Testing | k6 | 부하 테스트 |
| Query Analysis | EXPLAIN ANALYZE | SQL 실행 계획 분석 |
| Version Control | Git, GitHub | 형상 관리 및 협업 |

## 📅 학습 로드맵

| 일정 | 학습 주제 | 주요 목표 |
|---|---|---|
| DAY 1 | 성능 테스트 기초 | 부하 테스트 및 주요 지표 이해 |
| DAY 2 | k6 부하 테스트 | API 부하 테스트 수행 |
| DAY 3 | PostgreSQL 인덱스 | B-Tree 및 인덱스 이해 |
| DAY 4 | SQL 실행 계획 분석 | EXPLAIN ANALYZE 활용 |
| DAY 5 | 쿼리 최적화 | 단일 및 복합 인덱스 비교 |
| DAY 6 | 페이징 최적화 | OFFSET 및 Keyset 비교 |
| DAY 7 | 성능 개선 검증 | 개선 전후 성능 측정 및 분석 |

## 📁 프로젝트 구조

```text
performance-lab/
├── .github/
│   └── ISSUE_TEMPLATE/
├── backend/
├── load-test/
├── sql/
├── docs/
└── README.md
```

## 📊 최종 결과물

- Spring Boot 기반 조회 API
- PostgreSQL 테스트 데이터 및 인덱스 구성
- k6 부하 테스트 스크립트
- SQL 실행 계획 분석 자료
- 인덱스 및 쿼리 최적화 실습
- OFFSET 및 Keyset 페이징 비교
- 성능 개선 전후 비교 보고서

## 📚 학습 및 작업 관리

세부 학습 내용과 실습 과제는 GitHub Issues를 통해 관리합니다.

각 학습 단계에서 수행한 작업, 테스트 결과, 분석 내용은 GitHub에 기록합니다.

**단순히 기능을 구현하는 것에 그치지 않고, 성능 문제를 발견하고 개선하는 과정을 직접 경험하는 것을 목표로 합니다.**