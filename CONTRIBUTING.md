# play597 팀 워크플로우 가이드

GitHub Issue 기반의 **Spec-Driven 개발** 워크플로우입니다.
사람과 AI 모두 이 문서를 참고하여 작업합니다.

---

## 워크플로우 개요

```
1. 논의      →  기능/작업 결정 (대화, AI 협업)
2. Issue     →  Spec 작성 (GitHub Issue)
3. 개발      →  Branch 생성 & 구현
4. PR        →  코드 리뷰 & 머지
5. Done      →  Issue 자동 Close
```

### 상태 흐름 (GitHub Project)

```
📋 Todo → 🚧 In Progress → ✅ Done
```

---

## 1. Issue 작성법

### Issue = Spec 문서

**Issue 본문이 곧 기능 명세서입니다.** 별도 문서 없이 Issue로 모든 맥락을 관리합니다.

AI가 이해하고 구현할 수 있도록 명확하게 작성하세요.

### 좋은 Issue 예시

```markdown
## 요약
사용자가 프로필 이미지를 변경할 수 있다.

## 배경
현재는 프로필 이미지 변경이 불가능해서 CS로 요청이 많음.

## 상세 스펙

### 동작 흐름
1. 프로필 페이지에서 이미지 클릭
2. 파일 선택 다이얼로그 표시
3. 이미지 선택 후 미리보기 표시
4. 저장 버튼 클릭 시 업로드

### 상세 내용
- 지원 포맷: jpg, png, webp
- 최대 크기: 5MB
- 이미지 리사이즈: 200x200

## 입력 / 출력

**입력**: 이미지 파일
**출력**:
- 성공 → 업로드된 이미지 URL
- 실패 → 에러 메시지

## 예시
- 정상: 1MB jpg 파일 → 업로드 성공
- 에러: 10MB 파일 → "파일 크기 초과" 메시지

## 완료 조건 (Acceptance Criteria)
- [ ] 이미지 업로드 가능
- [ ] 미리보기 표시
- [ ] 에러 처리 (크기, 포맷)
- [ ] 테스트 코드 작성
```

### Issue 작성 팁

| DO ✅ | DON'T ❌ |
|-------|----------|
| 구체적인 동작 흐름 | 모호한 설명만 |
| 입력/출력 명시 | "알아서 해줘" |
| Acceptance Criteria 체크리스트 | 완료 기준 없음 |
| 예시 케이스 포함 | 맥락 없이 단독 |

---

## 2. AI 이슈 생성 플로우

### 템플릿 파일

| Type | 템플릿 파일 | Label |
|------|-------------|-------|
| Spec | `.github/ISSUE_TEMPLATE/spec.yml` | `spec` |
| Bug | `.github/ISSUE_TEMPLATE/bug_report.yml` | `bug` |
| Task | `.github/ISSUE_TEMPLATE/task.yml` | `task` |

### 생성 플로우

```
1. 이슈 유형 파악 (Spec/Bug/Task)
2. 해당 템플릿 파일 읽기 → body 필드 구조 확인
3. 템플릿 형식에 맞게 body 작성
4. 이슈 생성
```

### 이슈 생성 명령어

```bash
gh issue create \
  --title "[Type]: 제목" \
  --label "{label}" \
  --project "play597" \
  --body "{템플릿 필드 구조에 맞춘 내용}"
```

### 프로젝트 필드 설정 (CLI)

이슈 생성 후 프로젝트 필드를 CLI로 설정합니다.

**1. 아이템 ID 조회**

```bash
gh project item-list 5 --owner play597 --format json | jq -r '.items[] | select(.content.number == {이슈번호}) | .id'
```

**2. 필드 설정**

```bash
PROJECT_ID="PVT_kwDOCnQtDs4BOEu3"
ITEM_ID="{조회한 아이템 ID}"

# Status 설정
gh project item-edit --project-id $PROJECT_ID --id $ITEM_ID \
  --field-id PVTSSF_lADOCnQtDs4BOEu3zg84eP4 \
  --single-select-option-id {option_id}

# Priority 설정
gh project item-edit --project-id $PROJECT_ID --id $ITEM_ID \
  --field-id PVTSSF_lADOCnQtDs4BOEu3zg9AMZA \
  --single-select-option-id {option_id}

# Iteration 설정
gh project item-edit --project-id $PROJECT_ID --id $ITEM_ID \
  --field-id PVTIF_lADOCnQtDs4BOEu3zg88_CE \
  --iteration-id {iteration_id}

# Target Date 설정
gh project item-edit --project-id $PROJECT_ID --id $ITEM_ID \
  --field-id PVTF_lADOCnQtDs4BOEu3zg88_Ho \
  --date YYYY-MM-DD
```

**필드 옵션 ID 참조**

| Status | Option ID |
|--------|-----------|
| Todo | `f75ad846` |
| In-Progress | `47fc9ee4` |
| Done | `98236657` |

| Priority | Option ID |
|----------|-----------|
| high | `f206906c` |
| medium | `5d71a576` |
| low | `d2751d36` |

| Iteration | Option ID | 시작일 |
|-----------|-----------|--------|
| Iteration 1 | `c6894909` | 2026-02-03 |
| Iteration 2 | `22f73190` | 2026-02-10 |
| Iteration 3 | `72f9c5cc` | 2026-02-17 |

> **Note:** 이슈 생성 시 Status는 기본값 없음. `Todo`로 설정 필요.

---

## 3. AI와 협업하기

### Issue 생성 요청

```
"프로필 이미지 변경 기능 이슈 만들어줘"

→ AI가 스펙 논의 후 이슈 생성
→ 프로젝트 필드 자동 설정
```

### 스펙 논의 요청

```
"이 기능 어떻게 설계하면 좋을까?"

→ 대화로 스펙 결정 → Issue 생성
```

### 구현 요청

```
좋은 예:
"Issue #123 구현해줘"
"이 스펙대로 만들어줘: [스펙 내용]"

나쁜 예:
"결제 기능 만들어줘" (스펙 없음)
"알아서 해줘" (맥락 없음)
```

### AI가 참조하는 문서

| 문서 | 용도 |
|------|------|
| `CLAUDE.md` | 코드 작성 규칙, 프로젝트 맥락 |
| `AGENTS.md` | 디렉토리별 상세 문서 |
| `CONTRIBUTING.md` | 워크플로우 (이 문서) |

---

## 3. Labels 체계

### Type (필수)

| Label | 용도 | 템플릿 |
|-------|------|--------|
| `spec` | 기능 명세 | spec.yml |
| `bug` | 버그 수정 | bug_report.yml |
| `task` | 일반 작업 | task.yml |
| `chore` | 설정, 문서 등 | - |

> **Note:** Priority와 Status는 라벨이 아닌 **GitHub Project 필드**에서 관리합니다.

---

## 4. Branch & PR 규칙

### 브랜치 생성

```bash
# Issue에서 바로 브랜치 생성 (추천)
gh issue develop 123 --checkout
```

### 브랜치 네이밍

```
feature/issue-{번호}-{설명}
bugfix/issue-{번호}-{설명}
chore/{설명}

예시:
feature/issue-123-profile-image
bugfix/issue-45-login-error
```

### PR 생성

```bash
gh pr create --title "feat: 프로필 이미지 변경" --body "Closes #123"
```

### PR 규칙

- **반드시 Issue 번호 연결**: `Closes #123`
- PR 템플릿의 체크리스트 확인
- 머지 시 Issue 자동 Close

### 커밋 메시지 규칙

| Prefix | 용도 |
|--------|------|
| `feat:` | 새로운 기능 |
| `fix:` | 버그 수정 |
| `docs:` | 문서 수정 |
| `refactor:` | 리팩토링 |
| `test:` | 테스트 추가/수정 |
| `chore:` | 빌드, 설정 등 |

---

## 5. GitHub Project 사용

Org-level Project에서 모든 레포의 Issue를 통합 관리합니다.

### 뷰 (Views)

| 뷰 | 용도 |
|----|------|
| **Board** | 칸반 보드 (상태별) |
| **Table** | 전체 목록 (필터링, Repository별) |

### 상태 변경

- Issue 생성 시 → `Backlog`
- 작업 시작 → `In Progress`
- PR 머지 시 → `Done` (자동)

---

## 6. gh CLI 명령어

### 설치 & 로그인

```bash
brew install gh
gh auth login
```

### Issue

```bash
# 목록
gh issue list

# 생성 (대화형)
gh issue create

# 생성 (한 줄)
gh issue create -t "[Spec]: 제목" -b "내용" -l "spec" --project "play597"

# 보기
gh issue view 123

# 브랜치 생성
gh issue develop 123 --checkout
```

### PR

```bash
# 생성
gh pr create --title "feat: 기능" --body "Closes #123"

# 목록
gh pr list

# 보기
gh pr view 456
```

### Project

```bash
# 목록
gh project list --owner play597

# 보기
gh project view [NUMBER] --owner play597
```

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
gh issue create -t "[Spec]: 제목" -b "내용" -l "spec" --project "play597"
```

---

## Questions?

궁금한 점이 있으면 Issue로 문의해주세요.
