# 프로젝트 1: CLI 도구

ECC를 사용하여命令行 도구를 구축합니다.

## 프로젝트 목표

**예상 시간**: 2-3시간

ECC 프로젝트 구성요소（Agent, Skill, Hook）를 생성하는 `ecc-gen` 도구를 만듭니다.

## 기능 요구사항

### 1. 핵심 기능
- `ecc-gen agent <name>` - Agent 파일 생성
- `ecc-gen skill <name>` - Skill 파일 생성
- `ecc-gen hook <name>` - Hook 설정 생성

### 2. 인터랙티브 기능
- 인터랙티브 질문 생성
- 템플릿 선택
- 출력 경로 지정

### 3. 고급 기능
- 배치 생성
- 사용자 정의 템플릿
- 설정 파일 지원

## 기술方案

### 디렉토리 구조

```
ecc-gen/
├── src/
│   ├── commands/
│   │   ├── agent.js
│   │   ├── skill.js
│   │   └── hook.js
│   ├── generators/
│   │   └── templates/
│   └── index.js
├── package.json
└── README.md
```

### 사용 라이브러리
- `commander` - CLI 매개변수 파싱
- `inquirer` - 인터랙티브 질문
- `ejs` - 템플릿 엔진

## 수락 기준

- [ ] 세 하위 명령 모두 정상 작동
- [ ] 생성된 템플릿 형식 올바름
- [ ] `--output`로 출력 경로 지정 지원
- [ ] 단위 테스트 포함
- [ ] 문서 완비

## 학습 포인트

- CLI 도구 개발
- 템플릿 시스템 설계
- 인터랙티브 입력 처리
- 모듈화 아키텍처

> 참고 명령: [/build-fix](../참조문서/commands/04-构建修复.md) | [/test](../참조문서/commands/02-测试相关.md)

---

[실전 프로젝트 디렉토리로 돌아가기](./README.md)