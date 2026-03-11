---
name: commit
description: Conventional commit 형식으로 커밋 생성. Use when user asks to commit changes, create commits, or mentions 커밋, 커밋해줘, commit.
allowed-tools: Bash, Read, Glob, Grep
arguments:
  - name: push
    description: 커밋 후 원격에 push할지 여부
    default: "false"
---

# Commit 생성

git 변경사항을 분석하여 Conventional Commit 형식으로 커밋을 생성합니다.

## Commit Convention

**형식:** `type(scope): 메시지`

**Types:**
- `feat`: 새로운 기능
- `fix`: 버그 수정
- `refactor`: 리팩토링 (기능 변경 없음)
- `docs`: 문서 변경
- `test`: 테스트 추가/수정
- `chore`: 빌드, 설정 등 기타 변경

## Process

1. `git status`와 `git diff --staged`로 staged 변경사항 확인
2. `git log --oneline -5`로 최근 커밋 스타일 참고
3. **변경사항을 의미론적 단위로 분석** (아래 "커밋 분리 전략" 참고)
4. 변경된 파일들의 경로에서 scope 추론 (apps/thetle-v2 → thetle-v2, packages/core → core)
5. 변경 내용을 분석하여 적절한 type 선택
6. 커밋 계획을 출력한 후 즉시 커밋 실행 (확인 불필요)
7. `--push`가 `true`이면 커밋 후 `git push` 실행

## 커밋 분리 전략

**코드리뷰를 위한 논리적 흐름을 고려하여 커밋을 분리합니다.**

분리 기준:
- **기능 단위**: 하나의 기능 추가/수정은 하나의 커밋
- **리팩토링 vs 기능**: 리팩토링과 기능 변경은 별도 커밋으로 분리
- **의존성 순서**: 다른 변경의 기반이 되는 변경을 먼저 커밋
- **테스트**: 테스트 추가는 관련 기능과 함께 또는 별도 커밋

예시 시나리오:
```
# 변경사항이 섞여있는 경우
1. refactor: 헬퍼 함수 추출
2. feat: 새 기능 추가 (헬퍼 함수 사용)
3. test: 새 기능 테스트 추가
```

**단, 사용자가 단일 커밋을 원하면 분리하지 않습니다.**

## Rules

- 여러 scope에 걸친 변경은 주요 변경 scope 사용 또는 복수 표기
- 메시지는 한글 또는 영어로 간결하게 작성
- "무엇을 왜" 변경했는지 명확히 전달
- `.env`, credentials 등 민감한 파일은 커밋하지 않도록 경고

## User Hint

$ARGUMENTS
