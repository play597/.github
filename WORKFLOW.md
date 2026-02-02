# 팀 워크플로우 가이드

GitHub Issue 기반의 Spec-Driven 개발 워크플로우입니다.

## 워크플로우 개요

```
1. 논의     →  기능/작업 결정 (대화, 미팅)
2. Issue    →  Spec 작성 (GitHub Issue)
3. 개발     →  Branch 생성 & 구현
4. PR       →  코드 리뷰 & 머지
5. Done     →  Issue 자동 Close
```

### 상태 흐름

```
📋 Backlog → 📝 Spec Review → 🚧 In Progress → 🧪 Testing → ✅ Done
```

---

## 1. GitHub Issue 작성법

### Issue = Spec 문서

Issue 본문이 곧 기능 명세서입니다. 별도 문서 없이 Issue로 모든 맥락을 관리합니다.

### 좋은 Issue 예시

```markdown
## 요약
사용자가 정기결제를 환불 요청할 수 있다.

## API 스펙

### Endpoint
`POST /api/seller/billing/{billingId}/refund`

### Request
```json
{
  "reason": "고객 요청",
  "amount": 10000
}
```

### Response
```json
{
  "success": true,
  "message": "환불 처리되었습니다",
  "data": {
    "refundId": "ref_xxx",
    "status": "REFUNDED"
  }
}
```

## 완료 조건 (Acceptance Criteria)
- [ ] 부분 환불 가능
- [ ] 전체 환불 가능
- [ ] 환불 후 billingHistory 상태 변경
- [ ] 7일 이내만 환불 가능
- [ ] 테스트 코드 작성
```

### Issue 작성 팁

| DO | DON'T |
|----|-------|
| 구체적인 API 스펙 포함 | 모호한 설명만 |
| Acceptance Criteria 체크리스트 | 완료 기준 없음 |
| 관련 Issue 링크 | 맥락 없이 단독 |
| 적절한 Label 부착 | Label 없음 |

---

## 2. Labels 체계

### Type (필수)

| Label | 용도 |
|-------|------|
| `spec` | 기능 명세 |
| `bug` | 버그 수정 |
| `task` | 일반 작업 |
| `chore` | 설정, 문서 등 |

### Priority (필수)

| Label | 용도 |
|-------|------|
| `priority:high` | 긴급, 블로커 |
| `priority:medium` | 이번 스프린트 |
| `priority:low` | 나중에 |

### Domain (선택)

| Label | 용도 |
|-------|------|
| `domain:billing` | 결제 관련 |
| `domain:store` | 스토어 관련 |
| `domain:product` | 상품 관련 |
| `domain:user` | 사용자 관련 |
| `domain:chat` | 채팅 관련 |

---

## 3. GitHub Project 사용법

Org-level Project에서 모든 레포의 Issue를 통합 관리합니다.

### 뷰 (Views)

| 뷰 | 용도 |
|----|------|
| **Board** | 칸반 보드 (상태별) |
| **Table** | 전체 목록 (필터링) |
| **Roadmap** | 일정 기반 뷰 |

### 상태 변경

- Issue 생성 시 → `Backlog`
- 스펙 확정 후 → `In Progress`
- PR 생성 시 → 자동 연결
- PR 머지 시 → `Done` (자동)

---

## 4. gh CLI 명령어

### 설치 & 로그인

```bash
# 설치 (macOS)
brew install gh

# 로그인
gh auth login
```

### Issue 관련

```bash
# Issue 목록 보기
gh issue list

# Issue 생성
gh issue create --title "환불 기능" --body "스펙 내용..."

# Issue 생성 (대화형)
gh issue create

# Issue 보기
gh issue view 123

# Issue에서 브랜치 생성
gh issue develop 123 --checkout
```

### PR 관련

```bash
# PR 생성
gh pr create --title "feat: 환불 기능" --body "Closes #123"

# PR 목록
gh pr list

# PR 보기
gh pr view 456
```

### Project 관련

```bash
# Project 목록
gh project list --owner play597

# Project 보기
gh project view [PROJECT_NUMBER] --owner play597

# Issue를 Project에 추가
gh project item-add [PROJECT_NUMBER] --owner play597 --url [ISSUE_URL]
```

---

## 5. AI(Claude)와 협업

### AI에게 Issue 생성 요청

```
"정기결제 환불 기능 이슈 만들어줘"

→ AI가 gh issue create로 생성
→ 스펙 논의 후 수정
```

### AI에게 스펙 논의 요청

```
"이 기능 어떻게 설계하면 좋을까?"

→ 대화로 스펙 결정
→ 결정 내용으로 Issue 생성
```

### AI가 참조하는 문서

| 문서 | 용도 |
|------|------|
| `CLAUDE.md` | 코드 작성 규칙 |
| `AGENTS.md` | 디렉토리별 상세 문서 |
| `WORKFLOW.md` | 작업 프로세스 (이 문서) |

### AI에게 작업 요청 시 팁

```
좋은 예:
"Issue #123 구현해줘"
"이 스펙대로 API 만들어줘: [스펙 내용]"

나쁜 예:
"결제 기능 만들어줘" (스펙 없음)
"알아서 해줘" (맥락 없음)
```

---

## 6. 브랜치 & PR 규칙

### 브랜치 네이밍

```
feature/issue-{번호}-{설명}
bugfix/issue-{번호}-{설명}
chore/{설명}

예시:
feature/issue-123-billing-refund
bugfix/issue-45-login-error
chore/update-deps
```

### PR 규칙

```markdown
## PR 제목
feat: 환불 기능 추가

## PR 본문
Closes #123

## 변경 내용
- 환불 API 추가
- 환불 상태 처리

## 테스트
- [ ] 부분 환불 테스트
- [ ] 전체 환불 테스트
```

### Issue 연결

PR 본문에 다음 키워드로 Issue 자동 연결:

```
Closes #123
Fixes #123
Resolves #123
```

머지 시 Issue가 자동으로 Close 됩니다.

---

## 7. 체크리스트

### 작업 시작 전

- [ ] 관련 Issue가 있는가?
- [ ] Issue에 스펙이 명확한가?
- [ ] Acceptance Criteria가 있는가?

### PR 생성 전

- [ ] 테스트 코드 작성했는가?
- [ ] lint/test 통과하는가?
- [ ] Issue 번호 연결했는가?

### 머지 전

- [ ] 리뷰 받았는가?
- [ ] CI 통과했는가?
- [ ] Acceptance Criteria 모두 체크했는가?

---

## Quick Reference

```bash
# 작업 시작
gh issue develop 123 --checkout

# 작업 완료
git add . && git commit -m "feat: 설명"
gh pr create --title "feat: 설명" --body "Closes #123"

# Issue 빠른 생성
gh issue create -t "제목" -b "내용" -l "spec,priority:high"
```
