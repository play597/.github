# play597 팀 워크플로우 가이드

Linear + GitHub 기반의 **Spec-Driven 개발** 워크플로우입니다.
사람과 AI 모두 이 문서를 참고하여 작업합니다.

> **역할 분담**: Linear = 업무 관리 (이슈, 스프린트, 문서) / GitHub = 코드 관리 (브랜치, PR, 리뷰)

---

## 워크플로우 개요

### 계층 구조

```
Initiative (서비스명)            예: 신고포상제
└── Project (서비스명 - 기능)    예: 신고포상제 - 대용량 업로드
    └── Milestone (단계)        예: 설계, 구현, 테스트 & 배포
        └── Issue (작업)        예: R2 멀티파트 업로드 API 설계
```

### 작업 흐름

```
1. Initiative →  서비스 단위 목표 설정
2. Project    →  기능 단위 정의 + Milestone + Initiative 지정
3. Issue      →  실제 구현할 작업 쪼개기 (기능, 버그, 개선, 문서, 설정)
4. 개발       →  Branch 생성 & 구현 (GitHub)
5. PR         →  코드 리뷰 & 머지 (GitHub)
6. Done       →  Linear 이슈 자동 완료
```

### 상태 흐름 (Linear Issue)

```
📥 Backlog → 📋 Todo → 🚧 In Progress → 👀 In Review → ✅ Done
```

---

## 1. Project 생성

### Project = 기획 단위

**규모 있는 작업은 Project로 만듭니다.** 논의, 기획, 목표를 Project에 정리하고, 그 안에서 프론트/백엔드 이슈를 생성합니다.

`기본` 템플릿을 선택하면 설명 구조가 세팅됩니다. Milestone은 프로젝트 생성 시 함께 추가합니다.

- **제목 규칙**: `서비스명 - 기능` (예: `신고포상제 - 대용량 업로드`)
- **Initiative 지정**: 프로젝트 생성 시 해당 서비스의 Initiative 선택
- **템플릿 관리**: Linear → Settings → Team (Play597) → Templates → Project

### 구조 예시

```
Initiative: 신고포상제
└── Project: 신고포상제 - 대용량 업로드
    │
    ├── Milestone: 설계
    │   ├── PLA-200 R2 멀티파트 업로드 API 설계              (백엔드)
    │   └── PLA-201 업로드 PoC 테스트                        (백엔드)
    │
    ├── Milestone: 구현
    │   ├── PLA-202 청크 분할 업로드 API 구현                (백엔드)
    │   ├── PLA-203 업로드 프로그레스바 UI                    (프론트)
    │   └── PLA-204 대용량 파일 에러 핸들링 UI                (프론트)
    │
    └── Milestone: 테스트 & 배포
        └── PLA-205 통합 테스트 & 배포                       (공통)
```

### 언제 Project를 만드는가?

| Project 생성 | Issue만 생성 |
|-------------|-------------|
| 여러 이슈가 필요한 기능 개발 | 단일 버그 수정 |
| 프론트 + 백엔드 협업 필요 | 간단한 설정 변경 |
| 마일스톤 단계가 있는 작업 | 문서 수정, chore |
| 기획/논의가 필요한 주제 | 명확한 단건 작업 |

---

## 2. Issue 생성

### Issue = Spec 문서

**Issue 본문이 곧 기능 명세서입니다.** Project 하위에 생성하거나, 단독으로 생성합니다.

### 이슈 템플릿

`Option/Alt + C`로 템플릿을 선택하여 생성합니다.

| 템플릿 | 라벨 | 용도 |
|--------|------|------|
| 기능 | `기능` | 기능 명세 (상세 스펙 + 구현 이슈) |
| 버그 | `버그` | 버그 수정 (재현 조건 + 기대 동작) |

### Workspace 라벨

| 라벨 | 용도 |
|------|------|
| `기능` | 새로운 기능 |
| `버그` | 버그 수정 |
| `개선` | 기존 기능 개선 |
| `문서` | 기술 문서 |
| `설정` | 설정, 빌드, 환경 등 |

> 템플릿 관리: Linear → Settings → Team (Play597) → Templates

### 이슈 생성 후

- Linear이 자동으로 **이슈 ID** (PLA-200) 부여
- Linear이 자동으로 **브랜치명** 생성 (gitBranchName)
- Cycle에 배치하면 스프린트 보드에 표시

---

## 3. Branch & PR 규칙

### 브랜치 생성

Linear 이슈의 `gitBranchName`을 그대로 사용합니다. (이슈 상세에서 복사 가능)

```bash
git checkout -b makeisss777/pla-200-프로필-이미지-변경
```

형태: `{username}/pla-{번호}-{이슈제목}`

### PR 생성

```bash
gh pr create --title "feat: 프로필 이미지 변경" --body "Fixes PLA-200"
```

### PR 규칙

- **반드시 Magic Word + Linear 이슈 ID 포함** (PR 본문에 작성)
- PR 템플릿의 체크리스트 확인

### Magic Words (이슈 연동 키워드)

PR **본문(description)** 에 magic word + 이슈 ID를 적어야 Linear가 인식합니다.

**Closing keywords** — 머지 시 이슈 자동 Done:

| 키워드 그룹 | 변형 |
|-------------|------|
| close | close, closes, closed, closing |
| fix | fix, fixes, fixed, fixing |
| resolve | resolve, resolves, resolved, resolving |
| complete | complete, completes, completed, completing |

```
예시: Fixes PLA-200
      Fixes PLA-200, PLA-201 and PLA-202
```

**Contributing keywords** — 연결만, 머지해도 이슈 상태 안 바뀜:

| 키워드 |
|--------|
| ref, references |
| part of |
| related to |
| contributes to |
| towards |

```
예시: Part of PLA-200
```

> **주의사항**
> - magic word 없이 이슈 ID만 적으면 인식 안 됨
> - 브랜치명에 이슈 ID가 있으면 해당 이슈 1개는 자동 연결
> - 커밋 메시지에는 적용 안 됨, PR description에만 동작

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

## 4. Linear 설정 참조

### Team: Play597

모든 이슈는 Play597 팀 하위에서 관리합니다.

### 우선순위 (Priority)

| 값 | 레벨 | 기준 |
|----|------|------|
| 1 | Urgent | 즉시 처리 (장애, 핫픽스) |
| 2 | High | 이번 Cycle 필수 |
| 3 | Normal | 이번 Cycle 목표 |
| 4 | Low | 여유 있을 때 |

### Cycle (스프린트)

- 2주 단위 자동 반복
- Cycle 뷰에서 이슈를 드래그하여 배치
- 번다운 차트로 진행률 자동 추적

---

## 5. AI와 협업하기

### Project 생성 요청

```
"신고포상제에 대용량 업로드 프로젝트 만들어줘"

→ AI가 논의 후 Project 생성 (Initiative 지정 + Milestone 추가)
→ 하위 이슈 구조 제안
```

### Issue 생성 요청

```
"신고포상제 - 대용량 업로드 프로젝트에 업로드 API 이슈 만들어줘"

→ AI가 템플릿 구조에 맞춰 Issue 생성
→ 우선순위·Cycle 설정
```

### 구현 요청

```
좋은 예:
"PLA-200 구현해줘"
"이 스펙대로 만들어줘: [스펙 내용]"

나쁜 예:
"결제 기능 만들어줘" (스펙 없음)
```

### AI가 참조하는 문서

| 문서 | 위치 | 용도 |
|------|------|------|
| `CLAUDE.md` | 각 레포 | 코드 작성 규칙, 프로젝트 맥락 |
| `CONTRIBUTING.md` | org `.github` | 워크플로우 (이 문서) |
| Linear Documents | Linear 앱 | 프로젝트별 기획 문서 |

---

## 6. 체크리스트

### 작업 시작 전
- [ ] Linear에 이슈가 있는가?
- [ ] 이슈에 스펙이 명확한가?
- [ ] Acceptance Criteria가 있는가?

### PR 생성 전
- [ ] lint/test 통과하는가?
- [ ] Magic Word + Linear 이슈 ID 포함했는가? (예: `Fixes PLA-200`)

### 머지 전
- [ ] CI 통과했는가?
- [ ] Acceptance Criteria 모두 체크했는가?

---

## Quick Reference

```bash
# 작업 시작
git checkout -b makeisss777/pla-200-기능명

# 작업 완료
git add . && git commit -m "feat: 설명"
gh pr create --title "feat: 설명" --body "Fixes PLA-200"
```
