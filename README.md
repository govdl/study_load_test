# 🚀 7일 완성: 백엔드 성능 테스트 및 데이터베이스 최적화

## 📌 프로젝트 개요

Spring Boot와 PostgreSQL을 활용하여 조회 API를 구현하고, 부하 테스트를 통해 서버의 성능을 측정한다.

이후 데이터베이스 실행 계획을 분석하여 병목 구간을 파악하고, 인덱스 및 쿼리 최적화를 통해 성능을 개선한다.

단순히 기능을 구현하는 것을 넘어 **성능 측정 → 병목 분석 → 개선 → 재측정**의 과정을 직접 경험하는 것을 목표로 한다.

### 학습 목표

- 부하 테스트의 개념과 주요 성능 지표 이해
- k6를 활용한 HTTP API 부하 테스트 수행
- PostgreSQL 인덱스의 구조와 동작 원리 이해
- EXPLAIN ANALYZE를 활용한 SQL 실행 계획 분석
- 단일 인덱스와 복합 인덱스의 차이 이해
- OFFSET 및 Keyset 페이징 방식 비교
- 성능 개선 전후의 측정 결과 비교 및 문서화

---

## 🛠️ 기술 스택

| 구분 | 기술 | 활용 목적 |
|---|---|---|
| Backend | Java, Spring Boot | REST API 구현 |
| Database | PostgreSQL | 데이터 저장 및 조회 |
| ORM | Spring Data JPA | 데이터 접근 |
| Load Testing | k6 | 부하 테스트 |
| Query Analysis | EXPLAIN ANALYZE | SQL 실행 계획 분석 |
| Version Control | Git, GitHub | 코드 및 학습 기록 관리 |

---

## 📅 7일 학습 계획

### DAY 1. 백엔드 성능 테스트 기초 이해

**학습 목표**

부하 테스트가 필요한 이유와 서버 성능을 평가하는 주요 지표를 이해한다.

**학습 내용**

1. 부하 테스트(Load Testing)란?
   - 부하 테스트와 스트레스 테스트의 차이
   - 동시 사용자와 요청량의 관계
   - 서버의 병목 현상이 발생하는 원인

2. 주요 성능 지표
   - VUs (Virtual Users): 가상 사용자 수
   - RPS (Requests Per Second): 초당 요청 수
   - Response Time: 응답 시간
   - Average Response Time: 평균 응답 시간
   - p95, p99: 백분위 응답 시간
   - Error Rate: 요청 실패율
   - Throughput: 처리량

3. 백엔드 성능 저하 원인
   - CPU 및 메모리 사용량 증가
   - 데이터베이스 쿼리 지연
   - 네트워크 지연
   - 커넥션 풀 부족
   - 불필요한 데이터 조회

**실습 과제**

- Spring Boot 프로젝트 생성
- PostgreSQL 연결
- 게시글 조회 API 구현
- 기본적인 SQL 조회 실행

**결과물**

- [ ] 부하 테스트 개념 정리
- [ ] 주요 성능 지표 정리
- [ ] Spring Boot 프로젝트 생성
- [ ] 게시글 조회 API 구현

---

### DAY 2. k6 설치 및 기본 부하 테스트

**학습 목표**

k6를 활용하여 HTTP API에 부하를 발생시키고 테스트 결과를 분석한다.

**학습 내용**

1. k6의 개념과 특징
2. k6 설치 및 실행 방법
3. 테스트 스크립트 작성
4. 가상 사용자(VUs) 설정
5. 테스트 시간 및 요청 횟수 설정
6. 테스트 결과 분석

**실습 과제**

다음과 같은 테스트 스크립트를 작성한다.

```javascript
import http from 'k6/http';
import { check } from 'k6';

export const options = {
  vus: 10,
  duration: '30s',
};

export default function () {
  const res = http.get(
    'http://localhost:8080/api/posts'
  );

  check(res, {
    'status is 200': (r) => r.status === 200,
  });
}
```

실행 명령어

```bash
k6 run load-test.js
```

다음 조건으로 테스트를 진행한다.

| 테스트 | 가상 사용자 | 테스트 시간 |
|---|---:|---:|
| 1차 | 10명 | 30초 |
| 2차 | 50명 | 30초 |
| 3차 | 100명 | 30초 |

**분석 항목**

- 사용자 증가에 따른 평균 응답 시간 변화
- p95 응답 시간 변화
- 초당 요청 처리량
- 요청 실패율

**결과물**

- [ ] k6 설치 및 실행
- [ ] 테스트 스크립트 작성
- [ ] 사용자 수별 부하 테스트
- [ ] 테스트 결과 기록 및 비교

---

### DAY 3. PostgreSQL 인덱스와 B-Tree 이해

**학습 목표**

데이터베이스 인덱스의 동작 원리를 이해하고 인덱스가 필요한 상황을 파악한다.

**학습 내용**

1. 인덱스(Index)의 개념
   - 인덱스를 사용하는 이유
   - 인덱스와 Full Table Scan
   - 인덱스의 장단점

2. B-Tree 구조
   - B-Tree와 B+Tree의 개념
   - 데이터 탐색 과정
   - 시간 복잡도 관점에서의 이해

3. 인덱스 종류
   - 기본 인덱스
   - 단일 인덱스
   - 복합 인덱스
   - UNIQUE 인덱스

4. 인덱스 설계 시 고려 사항
   - 카디널리티(Cardinality)
   - 선택도(Selectivity)
   - WHERE 조건과 인덱스
   - ORDER BY와 인덱스
   - INSERT, UPDATE, DELETE 시 인덱스 유지 비용

**실습 과제**

게시글 테이블을 생성하고 테스트 데이터를 준비한다.

```sql
CREATE TABLE posts (
    id BIGSERIAL PRIMARY KEY,
    author_id BIGINT NOT NULL,
    title VARCHAR(200),
    content TEXT,
    created_at TIMESTAMP NOT NULL
);
```

조회 조건에 따른 인덱스 적용을 실습한다.

```sql
CREATE INDEX idx_posts_author_id
ON posts(author_id);
```

**결과물**

- [ ] 인덱스 개념 및 특징 정리
- [ ] B-Tree 구조 정리
- [ ] 단일 인덱스와 복합 인덱스 비교
- [ ] 인덱스 적용 실습

---

### DAY 4. EXPLAIN ANALYZE를 활용한 쿼리 분석

**학습 목표**

SQL 실행 계획을 읽고 데이터베이스의 조회 성능이 저하되는 원인을 분석한다.

**학습 내용**

1. SQL 실행 계획이란?
2. EXPLAIN과 EXPLAIN ANALYZE 차이
3. 주요 실행 계획 노드
   - Seq Scan
   - Index Scan
   - Index Only Scan
   - Bitmap Index Scan
   - Nested Loop
   - Hash Join
   - Sort

4. 실행 계획에서 확인할 항목
   - Cost
   - Actual Time
   - Actual Rows
   - Loops
   - Rows Removed by Filter

**실습 과제**

인덱스가 없는 상태에서 조회 쿼리를 실행한다.

```sql
EXPLAIN ANALYZE
SELECT *
FROM posts
WHERE author_id = 100;
```

인덱스 생성 후 동일한 쿼리를 다시 실행한다.

```sql
CREATE INDEX idx_posts_author_id
ON posts(author_id);
```

```sql
EXPLAIN ANALYZE
SELECT *
FROM posts
WHERE author_id = 100;
```

두 실행 계획을 비교하고 다음 내용을 기록한다.

- Seq Scan 또는 Index Scan 여부
- 실제 실행 시간
- 탐색 및 반환된 행의 수
- 인덱스 적용 여부에 따른 차이

**결과물**

- [ ] EXPLAIN ANALYZE 사용법 정리
- [ ] 실행 계획 분석
- [ ] 인덱스 적용 전후 비교
- [ ] 실행 계획 분석 보고서 작성

---

### DAY 5. 인덱스 및 SQL 쿼리 최적화

**학습 목표**

조회 조건에 맞는 인덱스를 설계하고 쿼리 성능을 개선한다.

**학습 내용**

1. 단일 인덱스와 복합 인덱스
2. 복합 인덱스의 컬럼 순서
3. WHERE 및 ORDER BY 조건과 인덱스
4. 인덱스가 사용되지 않는 상황
5. 불필요한 데이터 조회 방지
6. SELECT * 대신 필요한 컬럼만 조회하기
7. 데이터베이스 쿼리 최적화 원칙

**실습 과제**

다음과 같은 조회 쿼리를 작성한다.

```sql
SELECT *
FROM posts
WHERE author_id = 100
ORDER BY created_at DESC;
```

복합 인덱스를 적용한다.

```sql
CREATE INDEX idx_posts_author_created
ON posts(author_id, created_at DESC);
```

적용 전후 실행 계획을 비교한다.

추가 실습:

- 단일 인덱스와 복합 인덱스 비교
- 인덱스 컬럼 순서 변경에 따른 차이 확인
- ORDER BY와 인덱스 활용 확인
- 인덱스가 불필요한 경우 확인

**결과물**

- [ ] 복합 인덱스 설계
- [ ] 인덱스 컬럼 순서에 따른 차이 분석
- [ ] 쿼리 최적화 실습
- [ ] 개선 과정 문서화

---

### DAY 6. 페이징 최적화와 추가 성능 개선

**학습 목표**

대량의 데이터를 조회할 때 발생하는 성능 문제를 이해하고 페이징 방식별 차이를 비교한다.

**학습 내용**

1. 페이징(Pagination)의 개념
2. OFFSET 기반 페이징
3. Keyset(Cursor) 기반 페이징
4. 대량 데이터 조회 시 OFFSET의 문제점
5. Keyset 페이징의 장점과 한계
6. JPA에서 페이징 처리하는 방법

**실습 과제**

OFFSET 기반 페이징을 구현한다.

```sql
SELECT *
FROM posts
ORDER BY id
LIMIT 20 OFFSET 100000;
```

Keyset 기반 페이징을 구현한다.

```sql
SELECT *
FROM posts
WHERE id > 100000
ORDER BY id
LIMIT 20;
```

두 쿼리의 실행 계획과 응답 시간을 비교한다.

추가로 Spring Data JPA에서 다음 기능을 구현한다.

- Pageable을 활용한 OFFSET 페이징
- 커서 값을 활용한 Keyset 페이징
- 다음 페이지 조회

**결과물**

- [ ] OFFSET 페이징 구현
- [ ] Keyset 페이징 구현
- [ ] 실행 계획 및 응답 시간 비교
- [ ] 페이징 방식별 장단점 정리

---

### DAY 7. 최종 부하 테스트 및 성능 개선 검증

**학습 목표**

앞서 학습한 내용을 종합하여 API 성능을 측정하고 개선 결과를 검증한다.

**학습 내용**

1. 성능 테스트 시나리오 작성
2. 개선 전후 동일한 조건으로 부하 테스트
3. 평균 응답 시간 비교
4. p95 응답 시간 비교
5. 처리량 및 에러율 비교
6. 성능 개선 결과 분석
7. GitHub README 작성

**실습 과제**

다음 조건으로 성능 테스트를 진행한다.

| 테스트 | 사용자 수 | 테스트 시간 |
|---|---:|---:|
| 1차 | 10명 | 30초 |
| 2차 | 50명 | 30초 |
| 3차 | 100명 | 30초 |

테스트 대상:

- 인덱스 미적용 조회 API
- 단일 인덱스 적용 조회 API
- 복합 인덱스 적용 조회 API
- 최적화된 페이징 API

테스트 환경, 데이터 크기, 요청 조건을 동일하게 유지한다.

**성능 비교 기록**

| 측정 지표 | 개선 전 | 개선 후 |
|---|---|---|
| 평균 응답 시간 | 측정값 | 측정값 |
| p95 응답 시간 | 측정값 | 측정값 |
| RPS | 측정값 | 측정값 |
| Error Rate | 측정값 | 측정값 |
| SQL 실행 시간 | 측정값 | 측정값 |

※ 실제 측정값을 기록한다. 인덱스 적용만으로 항상 성능이 개선되는 것은 아니므로 결과가 기대와 다르면 그 원인을 분석한다.

**최종 결과물**

- [ ] Spring Boot API 구현
- [ ] PostgreSQL 데이터 및 인덱스 구성
- [ ] k6 부하 테스트
- [ ] SQL 실행 계획 분석
- [ ] 인덱스 적용 전후 비교
- [ ] 페이징 성능 비교
- [ ] 최종 성능 개선 보고서
- [ ] GitHub README 작성

---

## 📁 프로젝트 디렉터리 구조

```text
performance-lab/
├── backend/
│   ├── src/
│   └── build.gradle
│
├── load-test/
│   ├── posts-load-test.js
│   ├── author-load-test.js
│   └── pagination-load-test.js
│
├── sql/
│   ├── schema.sql
│   ├── test-data.sql
│   └── index.sql
│
├── docs/
│   ├── day01-performance-basics.md
│   ├── day02-k6.md
│   ├── day03-index.md
│   ├── day04-explain-analyze.md
│   ├── day05-query-optimization.md
│   ├── day06-pagination.md
│   └── day07-performance-result.md
│
└── README.md
```

---

## 📚 추가로 학습할 개념

7일 동안의 핵심 학습이 끝난 후 다음 내용을 확장 학습한다.

### 데이터베이스
- 트랜잭션과 ACID
- MVCC
- 데이터베이스 락과 동시성 제어
- 커넥션 풀(Connection Pool)
- N+1 문제
- 캐싱과 Redis
- PostgreSQL VACUUM과 ANALYZE

### 백엔드 성능
- JVM 메모리 구조와 GC
- 스레드 풀과 동시성
- Spring Boot Actuator
- API 응답 시간 측정
- 캐시 적용 전후 성능 비교
- Docker 환경에서 리소스 제한과 성능 변화

### 테스트
- Smoke Test
- Load Test
- Stress Test
- Spike Test
- Soak Test

---

## 🎯 최종 학습 목표

이번 학습의 핵심은 도구 사용법을 익히는 것이 아니라, 성능 문제를 직접 발견하고 해결하는 경험을 쌓는 것이다.

최종적으로 다음 질문에 스스로 답할 수 있는 수준을 목표로 한다.

1. 서버에 부하가 발생하면 어떤 지표를 확인해야 하는가?
2. 평균 응답 시간과 p95 응답 시간은 어떤 차이가 있는가?
3. 데이터베이스에서 Full Table Scan은 왜 발생하는가?
4. 인덱스는 어떤 원리로 데이터를 빠르게 조회하는가?
5. EXPLAIN ANALYZE를 통해 무엇을 분석할 수 있는가?
6. 단일 인덱스와 복합 인덱스는 어떻게 다른가?
7. 인덱스가 오히려 성능을 저하시키는 경우는 언제인가?
8. OFFSET과 Keyset 페이징은 어떤 차이가 있는가?
9. 성능 개선 효과를 어떤 지표로 검증할 수 있는가?
10. 서버와 데이터베이스 중 병목 구간을 어떻게 구분할 수 있는가?

**최종 목표: 성능 테스트를 직접 수행하고, 병목 구간을 분석하며, 데이터베이스 최적화 결과를 측정값으로 설명할 수 있는 백엔드 개발 역량 확보.**