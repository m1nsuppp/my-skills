---
name: verify-code
description: 코드 변경 후 lint, tsc, test를 실행하고 에러가 있으면 자동 수정한다. 코드 구현이 끝난 뒤 검증 단계에서 사용한다. Use PROACTIVELY after completing code changes - when implementation tasks finish, or when user says 검증, verify, lint 돌려, tsc 확인, 테스트 돌려.
---

# Verify Code

코드 변경 후 lint, tsc, test를 통과할 때까지 반복 수정한다.

## 금지 패턴

수정 시 다음을 사용하지 않는다:

- `as` type assertion → zod `.parse()`로 대체
- `any` 타입
- `eslint-disable`, `eslint-disable-next-line`, `@ts-ignore`, `@ts-expect-error`

코드베이스 내 기존 코드를 레퍼런스 삼아 동일 패턴으로 수정한다.

## 실행 순서

### 1. 변경 파일 식별

```bash
git diff --name-only HEAD
```

변경된 파일 목록을 기준으로 해당 패키지/앱을 특정한다.

### 2. TypeScript 타입 체크

변경된 파일이 속한 프로젝트의 `tsconfig.json`을 찾아 실행:

```bash
npx tsc --noEmit --pretty --project <tsconfig-path>
```

에러가 있으면 수정 후 재실행, 통과할 때까지 반복한다.

### 3. Lint

변경된 파일이 속한 패키지의 lint 명령어를 실행:

```bash
pnpm --filter <package-name> lint
```

- Error만 수정 대상 (Warning 무시)
- 변경한 파일에서 발생한 에러만 수정

### 4. Test (해당 시)

변경된 파일에 대응하는 `.spec.ts`/`.test.ts`가 있으면 실행:

```bash
pnpm --filter <package-name> test -- --run <test-file-pattern>
```

테스트 파일이 없으면 건너뛴다.

### 5. 완료 보고

모든 검증을 통과하면 결과를 요약:

- tsc: 통과
- lint: 통과 (수정사항 간략히 나열)
- test: 통과 / 해당 없음
