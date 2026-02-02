# Contributing to play597 Projects

play597 프로젝트에 기여해 주셔서 감사합니다!

## 워크플로우 개요

```
1. Issue 확인/생성  →  스펙 작성 (GitHub Issue)
2. Branch 생성      →  Issue에서 브랜치 생성
3. 개발             →  구현 & 테스트
4. PR 생성          →  코드 리뷰
5. 머지             →  Issue 자동 Close
```

자세한 내용은 [WORKFLOW.md](./WORKFLOW.md)를 참고하세요.

---

## 1. Issue 확인

- **작업 전 반드시 관련 Issue가 있는지 확인**
- 새로운 기능은 `spec` 템플릿으로 Issue 생성
- Issue = Spec 문서입니다

```bash
# Issue에서 바로 브랜치 생성 (추천)
gh issue develop 123 --checkout
```

---

## 2. Branch 생성

### 브랜치 네이밍 규칙

```
feature/issue-{번호}-{설명}
bugfix/issue-{번호}-{설명}
chore/{설명}

# 예시
feature/issue-123-billing-refund
bugfix/issue-45-login-error
chore/update-deps
```

### 직접 생성 시

```bash
git checkout -b feature/issue-123-billing-refund
```

---

## 3. 커밋

```bash
git add .
git commit -m "feat: 환불 기능 추가"
```

### 커밋 메시지 규칙

| Prefix | 용도 |
|--------|------|
| `feat:` | 새로운 기능 |
| `fix:` | 버그 수정 |
| `docs:` | 문서 수정 |
| `style:` | 코드 스타일 변경 |
| `refactor:` | 리팩토링 |
| `test:` | 테스트 추가/수정 |
| `chore:` | 빌드, 설정 등 |

---

## 4. Pull Request

```bash
gh pr create --title "feat: 환불 기능 추가" --body "Closes #123"
```

### PR 규칙

- **반드시 Issue 번호 연결**: `Closes #123`
- PR 템플릿의 체크리스트 확인
- 머지 시 Issue 자동 Close

### Issue 연결 키워드

```
Closes #123
Fixes #123
Resolves #123
```

---

## 5. 체크리스트

### 작업 시작 전
- [ ] 관련 Issue가 있는가?
- [ ] Issue에 스펙이 명확한가?

### PR 생성 전
- [ ] 테스트 코드 작성했는가?
- [ ] lint/test 통과하는가?
- [ ] Issue 번호 연결했는가?

### 머지 전
- [ ] 리뷰 받았는가?
- [ ] CI 통과했는가?
- [ ] Acceptance Criteria 모두 체크했는가?

---

## Labels

| Label | 용도 |
|-------|------|
| `spec` | 기능 명세 |
| `bug` | 버그 수정 |
| `task` | 일반 작업 |
| `priority:high` | 긴급 |
| `priority:medium` | 이번 스프린트 |
| `priority:low` | 나중에 |

---

## Questions?

궁금한 점이 있으면 Issue로 문의해주세요.
