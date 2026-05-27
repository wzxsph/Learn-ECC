# 백엔드与 API 技能

## 개요

백엔드与 API 관련スキル用于빌드서버端应用、API 설계和데이터베이스操作。

## 코어技能

### backend-patterns

**용도**: 백엔드개발모범 사례

**코어개념**:
- 分层아키텍처（Controller → Service → Repository）
- 依赖注入
- 오류 처리전략
- 캐시패턴
- 消息큐통합

---

### api-design

**용도**: API 설계与RESTful 사양

**코어개념**:
- RESTful 라우팅설계
- 요청/응답형식
- 인증与권한 부여
- 버전控制전략
- 限流与캐시

---

### api-connector-builder

**용도**: API 연결器빌드도구

**사용场景**:
- 第三方 API 통합
- Webhook 处理
- OAuth 流程구현
- API 오류재시도机制

---

## 데이터베이스技能

### database-migrations

**용도**: 데이터베이스마이그레이션管理

**코어개념**:
- 마이그레이션파일组织
- 데이터롤백전략
- 种子데이터管理
- 零停机배포

---

### mysql-patterns

**용도**: MySQL 데이터베이스모범 사례

**코어개념**:
- 인덱스최적화
- 查询최적화
- 分表전략
- 主从복사

---

### postgres-patterns

**용도**: PostgreSQL 데이터베이스모범 사례

**코어개념**:
- JSONB 操作
- 全文搜索
- 分区表
- 高级인덱스전략

---

### redis-patterns

**용도**: Redis 캐시패턴

**코어개념**:
- 캐시전략（Cache-Aside、Write-Through）
- 分布式锁
- 세션存储
- 消息게시/구독

---

### clickhouse-io

**용도**: ClickHouse 列式存储

**사용场景**:
- 분석型查询
- 로그存储
- 실시간분석
- 大데이터管道

---

## 消息큐

### logistics-exception-management

**용도**: 物流异常管理

**사용场景**:
- 订单异常处理
- 退货流程
- 库存동기화

---

## 관련スキル

- `backend-patterns` - 백엔드패턴
- `api-design` - API 설계
- `database-migrations` - 데이터베이스마이그레이션
- `mysql-patterns` - MySQL 패턴
- `postgres-patterns` - PostgreSQL 패턴
- `redis-patterns` - Redis 패턴
- `clickhouse-io` - ClickHouse
