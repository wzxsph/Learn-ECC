# 배포与 DevOps 技能

## 개요

배포与 DevOps 技能用于컨테이너化、인프라即코드、持续통합和배포자동化。

## 코어技能

### docker-patterns

**용도**: Docker 컨테이너모범 사례

**코어개념**:
- 多단계빌드
- 最小化镜像크기
- 健康검사
- 로그管理
- 资源限制

---

### deployment-patterns

**용도**: 배포전략与패턴

**코어개념**:
- 蓝绿배포
- 金丝雀게시
- Rolling Update
- 롤백전략
- 零停机배포

---

### production-audit

**용도**: 生产环境审计

**체크 항목**:
- 보안설정
- 성능지표
- 可用性검사
- 모니터링알림검증

---

### production-scheduling

**용도**: 生产调度管理

**사용场景**:
- 定时태스크管理
- 批处理작업
- 维护窗口계획

---

### canary-watch

**용도**: 金丝雀게시모니터링

**기능**:
- 流量分割모니터링
- 오류率추적
- 성능对比
- 자동롤백触发

---

## 인프라

### network-config-validation

**용도**: 网络설정검증

**체크 항목**:
- 防火墙규칙
- 로드 밸런싱설정
- DNS 설정
- SSL/TLS 证书

---

### network-interface-health

**용도**: 网络인터페이스健康검사

**모니터링지표**:
- 带宽사용
- 지연
- 丢패키지率
- 연결数

---

## 관련スキル

- `docker-patterns` - Docker 패턴
- `deployment-patterns` - 배포패턴
- `production-audit` - 生产审计
- `production-scheduling` - 生产调度
- `canary-watch` - 金丝雀모니터링
- `network-config-validation` - 网络설정검증
- `network-interface-health` - 网络인터페이스健康
