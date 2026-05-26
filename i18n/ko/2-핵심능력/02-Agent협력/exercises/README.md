# 연습: Agent 협력

## 연습 1: 코드 리뷰類 Agent 사용

### 목표
코드 리뷰 Agent를 호출하고 그 피드백 메커니즘을 이해한다.

### 시나리오
다음과 같은 Python 코드를 작성했다고 가정（실제 실행 불필요）:

```python
def get_user_data(user_id, include_password=False):
    user = db.query("SELECT * FROM users WHERE id = " + user_id)
    if include_password:
        return user
    return {"name": user.name, "email": user.email}
```

### 단계

1. **이 코드를 분석**
   잠재적인 문제를 생각:
   - SQL 인젝션 취약성
   - 비밀번호泄露 위험
   - 에러 처리 누락

2. **적절한 Agent 선택**
   이 코드에는 다음을 사용해야 함:
   - `security-reviewer` - 보안 취약성 체크
   - `python-reviewer` - Python 베스트 프랙티스 체크

3. **Agent 피드백 시뮬레이트**
   Agent가 제공할 것 같은 피드백을 기록:

   ```markdown
   ## 코드 리뷰 피드백

   ### 보안 문제 (CRITICAL)
   - SQL 인젝션: user_id를 SQL 문에 직접 연결
   - 비밀번호泄露: include_password 옵션이 비밀번호를 노출할可能性

   ### 품질 문제 (HIGH)
   - 에러 처리 누락
   - 입력 검증 누락

   ### 개선 제안
   - 파라미터화 쿼리 사용
   - 비밀번호 필드 반환 안 함
   - 예외 처리 추가
   ```

4. **수리 계획 수립**
   `/plan`의思路를 사용하여 수리 계획을 수립:

   ```markdown
   # 수리 계획

   1. SQL 인젝션 수리: 파라미터화 쿼리 사용
   2. 비밀번호 반환 제거: 절대 비밀번호 반환 안 함
   3. 입력 검증 추가: user_id 형식 검증
   4. 에러 처리 추가: 사용자가 존재하지 않는 상황 처리
   ```

### 완료 기준
- [ ] 코드에서 보안 문제 식별 가능
- [ ] 리뷰 Agent의 피드백 구조 이해
- [ ] 수리 계획 수립 가능

---

## 연습 2: 빌드 수리類 Agent 사용

### 목표
빌드 Agent를 사용하여 빌드 에러를 진단하고 수리하는 방법을 학습.

### 시나리오
Go 프로젝트에 다음과 같은 빌드 에러가 있다고 가정:

```
# github.com/myproject/pkg
./main.go:15: undefined: " fmt"
./main.go:23: undefined: calculateTotal
./main.go:31: too many arguments to function call
```

### 단계

1. **에러 타입 분석**
   에러를 분류:

   | 에러 | 타입 | 심각도 |
   |------|------|----------|
   | undefined: " fmt" | 임포트 에러 | HIGH |
   | undefined: calculateTotal | 함수 미정의 | HIGH |
   | too many arguments | 파라미터 에러 | MEDIUM |

2. **수리 플로 설계**
   Agent 협조 사용:

   ```markdown
   # 빌드 수리 플로

   1. /go-build - 빌드 실행하여 완전한 에러 가져오기
   2. /build-fix - 빌드 수리 Agent 호출
   3. 에러 우선순위 분석: 높은 우선순위 먼저 수리
   4. 순차적으로 수리하고 검증
   5. /verify - 빌드 성공 확인
   ```

3. **각 에러의 수리 방안 설계**

   **에러 1: undefined: " fmt"**
   ```go
   // 문제: 여분의 따옴표
   import "fmt"  // 올바른 작성

   // 수리: 따옴표 제거
   ```

   **에러 2: undefined: calculateTotal**
   ```go
   // 문제: 함수가 미정의 또는 이름 오류
   // 수리: 함수 정의 확인, 추가 또는 이름 변경 필요
   ```

   **에러 3: too many arguments**
   ```go
   // 문제: 함수 호출 파라미터 수 불일치
   // 수리: 함수 시그니처 확인, 호출 파라미터 수정
   ```

4. **수리 경험 기록**
   에러 패턴 요약 생성:

   ```markdown
   # Go 빌드 에러 패턴

   ## 일반적인 에러
   1. undefined: 임포트/함수 문제
   2. too many/few arguments: 파라미터 불일치
   3. undefined: " xxx": 임포트 구문 에러

   ## 수리 전략
   - 에러 수 먼저 확인, 그 다음 에러 위치 확인
   - 높은 우선순위 에러가 낮은 우선순위 에러를连锁 유발 가능
   - 수리 후 재빌드하여 검증
   ```

### 완료 기준
- [ ] 빌드 에러 타입과 우선순위 분석 가능
- [ ] 완전한 수리 플로 설계
- [ ] 일반적인 에러 패턴 기록

---

## 연습 3: Agent 조합 워크플로

### 목표
복잡한 태스크를 설계하고 여러 Agent의 협조로 완료.

### 시나리오
새로운 사용자 인증 모듈을 개발,以下の機能 포함:
- 사용자 등록（비밀번호 강도 검증 포함）
- 사용자 로그인（무차별 대입 공격 방지 포함）
- JWT 토큰 생성 및 검증

### 단계

1. **태스크 분해**
   태스크를 서브태스크로 분해:

   ```markdown
   # 인증 모듈 개발 태스크

   ## 서브태스크
   1. 데이터베이스 설계 (Schema)
   2. 사용자 등록 API
   3. 사용자 로그인 API
   4. JWT 토큰 처리
   5. 보안 메커니즘（비밀번호 강도, 무차별 대입 공격 방지）
   ```

2. **Agent 할당**
   각 서브태스크에 Agent 할당:

   | 서브태스크 | Agent | 이유 |
   |--------|-------|------|
   | 데이터베이스 설계 | architect | 아키텍처 관점 필요 |
   | 등록 API | tdd-guide + security-reviewer | TDD + 보안 |
   | 로그인 API | tdd-guide + security-reviewer | TDD + 보안 |
   | JWT 처리 | security-reviewer | 보안 민감 |
   | 보안 메커니즘 | security-reviewer (우선) | 보안 우선 |

3. **워크플로 설계**

   ```
   [시작]
      ↓
   /plan (전체 계획)
      ↓
   ┌─────────────────────────────────────┐
   │  병행 실행:                            │
   │  - architect: 데이터베이스 설계         │
   │  - tdd-guide: 테스트 전략 설계        │
   └─────────────────────────────────────┘
      ↓
   [실시 단계]
      ↓
   ┌─────────────────────────────────────┐
   │  직렬 + 병행 혼합:                      │
   │  1. tdd-guide: 등록 테스트 작성        │
   │  2. 등록 기능 구현                     │
   │  3. security-reviewer: 보안 리뷰        │
   │  ──────────────────────────────────   │
   │  1. tdd-guide: 로그인 테스트 작성        │
   │  2. 로그인 기능 구현                   │
   │  3. security-reviewer: 보안 리뷰        │
   └─────────────────────────────────────┘
      ↓
   [통합 단계]
      ↓
   ┌─────────────────────────────────────┐
   │  - architect: JWT 아키텍처 설계         │
   │  - security-reviewer: 전체 보안 리뷰     │
   │  - code-reviewer: 코드 품질 리뷰       │
   │  - /verify: 전체 검증                  │
   └─────────────────────────────────────┘
      ↓
   [완료]
   ```

4. **워크플로 문서 작성**

   ```markdown
   # 사용자 인증 모듈 개발 워크플로

   ## 단계 1: 계획
   - /plan을 사용하여 전체 계획 수립
   - architect를 사용하여 데이터베이스와 JWT 아키텍처 설계

   ## 단계 2: TDD 개발
   - tdd-guide를 사용하여 테스트 작성을 가이드
   - 먼저 테스트 작성, 그 다음 기능 구현
   - 각 서브 기능 개별적으로 검증

   ## 단계 3: 보안 리뷰
   - security-reviewer를 사용하여 각 API 리뷰
   - 특히 관심: 비밀번호 처리, 토큰 보안, 인젝션 방어

   ## 단계 4: 통합 검증
   - code-reviewer를 사용하여 전체 리뷰
   - /verify를 사용하여 기능 완전성 검증

   ## Agent 협조 요약
   - architect: 아키텍처 설계 (1회)
   - tdd-guide: 테스트 가이드 (2회)
   - security-reviewer: 보안 리뷰 (3회)
   - code-reviewer: 품질 리뷰 (1회)
   ```

### 완료 기준
- [ ] 태스크 분해가 합리적이고 5+개의 서브태스크 포함
- [ ] Agent 할당 전략 명확
- [ ] 워크플로 설계 명확
- [ ] 문서화된 워크플로가 재사용하기 좋음

---

## 심층 학습

- 완전한 Agent 문서를 읽으세요: [../../../참조문서/agents/1.%20코드검토类.md](../../../참조문서/agents/1.%20코드검토类.md)
- 모든 사용 가능한 Agent 목록 보기
- 커스텀 Agent 동작 방법 학습