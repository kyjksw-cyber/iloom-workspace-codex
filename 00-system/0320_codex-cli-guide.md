# Codex CLI 완전 가이드

> 일룸 AX 1.5차 강의 — AI 코딩 에이전트로 실무하기

---

## 1. Codex CLI란?

터미널(명령줄)에서 실행하는 **OpenAI의 AI 코딩 에이전트**입니다.
ChatGPT가 "이렇게 하세요"라고 조언만 한다면, Codex CLI는 **직접 파일을 만들고, 수정하고, 실행**합니다.

| 구분 | ChatGPT (웹) | Codex CLI |
|------|-------------|-----------|
| 실행 위치 | 브라우저 | 내 컴퓨터 터미널 |
| 하는 일 | 대화, 조언 | **파일 생성/수정/삭제, 코드 실행** |
| 결과물 | 텍스트 답변 | 실제 파일이 내 폴더에 생김 |
| 맥락 | 대화창 안에서만 | **내 폴더 전체를 읽고 작업** |

---

## 2. 설치

### 2-1. 사전 준비

| 필요한 것 | 확인 방법 |
|----------|----------|
| Node.js (v22+) | `node --version` |
| Git | `git --version` |
| OpenAI API 키 | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |

### 2-2. Node.js 설치 (없는 경우)

**Windows**: [nodejs.org](https://nodejs.org) 에서 LTS 버전 다운로드 → 설치

**Mac**:
```bash
brew install node
```

### 2-3. Codex CLI 설치

```bash
npm install -g @openai/codex
```

### 2-4. API 키 설정

**Windows (PowerShell)**:
```powershell
$env:OPENAI_API_KEY = "sk-proj-xxxxxxxxxxxxx"
```

**Windows (영구 설정)**: 시스템 환경변수에 `OPENAI_API_KEY` 추가

**Mac/Linux**:
```bash
export OPENAI_API_KEY="sk-proj-xxxxxxxxxxxxx"
```

영구 설정: `~/.zshrc` 또는 `~/.bashrc`에 위 줄 추가

### 2-5. 워크스페이스 클론

```bash
git clone https://github.com/kyjksw-cyber/iloom-workspace-codex.git
cd iloom-workspace-codex
```

### 2-6. 첫 실행

```bash
codex
```

프롬프트가 뜨면 성공! 이제 자연어로 요청하면 됩니다.

```
> 데일리 노트 만들어줘
```

---

## 3. Claude Code vs Codex CLI — 핵심 차이

### 3-1. 기능 비교

| 기능 | Claude Code | Codex CLI |
|------|-----------|-----------|
| **핵심 설정 파일** | `CLAUDE.md` | `AGENTS.md` |
| **설정 디렉토리** | `.claude/` | `.codex/` |
| **슬래시 커맨드** | `/daily-note`, `/sns-draft` 등 30개+ | **없음** — 자연어로 요청 |
| **스킬 (자동 트리거)** | 키워드 감지 → 자동 실행 | **없음** — AGENTS.md 워크플로우로 대체 |
| **서브에이전트** | 복잡한 작업 자동 분배 | **없음** |
| **메모리** | 세션 간 자동 기억 | **없음** — progress.md로 수동 관리 |
| **MCP 서버** | 구글캘린더, Notion 등 연동 | **없음** |
| **AI 모델** | Claude (Anthropic) | GPT-4o / o4-mini (OpenAI) |
| **승인 모드** | 자동/수동 선택 | suggest / auto-edit / full-auto |

### 3-2. 같은 작업, 다른 방식

**데일리 노트 만들기**

| Claude Code | Codex CLI |
|------------|-----------|
| `/daily-note` | "데일리 노트 만들어줘" |
| 슬래시 커맨드가 자동 실행 | AGENTS.md의 워크플로우를 따라 실행 |

**SNS 콘텐츠 초안**

| Claude Code | Codex CLI |
|------------|-----------|
| `/sns-draft --channel instagram` | "인스타 포스트 초안 만들어줘" |
| 옵션 플래그로 세부 지정 | 자연어로 설명 |

**경쟁사 리뷰 분석**

| Claude Code | Codex CLI |
|------------|-----------|
| "경쟁사 분석해줘" → 스킬 자동 트리거 | "경쟁사 분석해줘" → AGENTS.md 보고 스크립트 실행 |

### 3-3. 한줄 요약

> **Claude Code** = 슬래시 커맨드 + 스킬 + 메모리 + MCP → **자동화 극대화**
> **Codex CLI** = AGENTS.md 하나 + 자연어 → **심플하지만 강력**

---

## 4. AGENTS.md — Codex의 두뇌

### 4-1. AGENTS.md가 뭔가요?

Codex CLI가 **매 세션마다 자동으로 읽는 설정 파일**입니다.
여기에 적힌 규칙, 워크플로우, 브랜드 정보를 Codex가 따릅니다.

Claude Code의 `CLAUDE.md`와 같은 역할이지만, Codex에서는 **이게 유일한 설정 수단**입니다.

### 4-2. 계층 구조

Codex는 AGENTS.md를 **루트 → 하위 폴더 순서**로 읽습니다.

```
iloom-workspace-codex/
├── AGENTS.md                          ← 1순위: 전체 규칙
├── 10-projects/
│   ├── 15-봄캠페인/
│   │   ├── AGENTS.md                  ← 2순위: 이 프로젝트 전용
│   │   └── progress.md
```

즉, 프로젝트 폴더에 AGENTS.md를 넣으면 **그 프로젝트 작업 시 자동으로 맥락이 추가**됩니다.

### 4-3. 수정 방법

워크스페이스 루트의 `AGENTS.md` 상단에서 본인 정보를 수정하세요:

```markdown
## 사용자 프로필
- **이름**: 홍길동
- **소속**: 영업효율개선팀
- **주요 업무**: SNS 콘텐츠 제작, 캠페인 기획
```

---

## 5. 맥락 있이 일하는 법

### 5-1. 문제: Codex는 기억을 못 한다

Claude Code는 메모리 시스템이 있어서 "지난번에 한 작업" 을 기억합니다.
Codex CLI는 **매 세션이 백지 상태**입니다.

### 5-2. 해결: progress.md + 프로젝트 AGENTS.md

**핵심 원리**: Codex가 기억 못 하면, **파일에 적어두고 매번 읽게 하면 된다.**

```
10-projects/15-봄캠페인/
├── AGENTS.md        ← "이 프로젝트는 이런 거야" (Codex 자동 인식)
├── progress.md      ← "여기까지 했어" (매 작업 후 업데이트)
└── memo.md          ← 작업 메모
```

### 5-3. 새 프로젝트 시작하기

Codex에게 요청:
```
새 프로젝트 만들어줘. 이름: 봄 SNS 캠페인
```

자동으로 생성되는 구조:
```
10-projects/[번호]-봄-sns-캠페인/
├── AGENTS.md
├── progress.md
└── memo.md
```

### 5-4. 프로젝트 AGENTS.md 작성법

```markdown
# 봄 SNS 캠페인

## 현재 상태
- 기획 완료, 콘텐츠 제작 중
- 인스타 3건 완료, 블로그 1건 남음

## 핵심 맥락
- 목표: 봄 신제품 인지도 확보
- 타겟: 2030 신혼부부
- 기한: 2026-04-15
- 채널: 인스타그램, 블로그

## 작업할 때 읽어야 할 파일
- progress.md — 진행 현황
- memo.md — 회의 메모, 피드백
```

### 5-5. progress.md 업데이트 규칙

**매 작업이 끝나면** Codex에게 요청:
```
progress.md 업데이트해줘
```

progress.md 형식:
```markdown
# 봄 SNS 캠페인 Progress

## 최근 업데이트
- 2026-03-20: 인스타 3건 초안 완성
- 2026-03-18: 캠페인 기획서 확정

## 완료
- [x] 캠페인 기획서 작성
- [x] 인스타 포스트 3건 초안

## 진행 중
- [ ] 블로그 포스트 1건

## 다음 할 일
- [ ] 콘텐츠 팀 리뷰
- [ ] 최종 발행
```

### 5-6. 이어서 작업하기 (핵심!)

다음 세션에서 프로젝트 작업을 이어갈 때:

```
봄 SNS 캠페인 프로젝트 이어서 할게.
10-projects/15-봄-sns-캠페인/progress.md 먼저 읽어줘.
```

또는 루트 AGENTS.md에 활성 프로젝트로 등록해두면, 매번 말 안 해도 됩니다:

```markdown
## 활성 프로젝트

| 프로젝트 | 맥락 파일 |
|---------|----------|
| 봄 SNS 캠페인 | `10-projects/15-봄-sns-캠페인/progress.md` |
```

### 5-7. 맥락 관리 요약

```
┌─────────────────────────────────────────────┐
│  AGENTS.md (루트)                            │
│  "프로젝트 작업 전 progress.md 읽어"          │
│  "활성 프로젝트 목록: ..."                    │
├─────────────────────────────────────────────┤
│  프로젝트/AGENTS.md                          │
│  "이 프로젝트는 뭐고, 목표는 뭐야"            │
├─────────────────────────────────────────────┤
│  프로젝트/progress.md                        │
│  "여기까지 했고, 다음은 이거야"                │
└─────────────────────────────────────────────┘

Codex가 이 3개를 읽으면 → 맥락 있는 작업 가능!
```

---

## 6. 승인 모드

Codex CLI는 파일을 직접 수정하기 때문에 **안전 장치**가 있습니다.

| 모드 | 설명 | 추천 대상 |
|------|------|----------|
| `suggest` | 모든 작업 승인 필요 (기본값) | **초보자 (추천)** |
| `auto-edit` | 파일 수정은 자동, 명령어는 승인 | 중급자 |
| `full-auto` | 모든 작업 자동 승인 | 숙련자 |

설정 방법 (`.codex/config.toml`):
```toml
model = "o4-mini"
approval_mode = "suggest"
```

또는 실행 시 지정:
```bash
codex --approval-mode auto-edit
```

---

## 7. 실전 워크플로우 예시

### 예시 1: 오늘의 데일리 노트

```
> 데일리 노트 만들어줘
```
→ `40-personal/41-daily/2026-03-20.md` 자동 생성

### 예시 2: SNS 콘텐츠 만들기

```
> 헤이븐 매트리스 인스타 포스트 초안 만들어줘.
> 타겟은 30대 신혼부부, 톤은 따뜻하게.
```
→ 캡션 + 해시태그 + 이미지 가이드 생성

### 예시 3: 데이터 분석

```
> 50-resources/sample-data/sample_sales_data.csv 분석해줘.
> 채널별 매출 트렌드 뽑아줘.
```
→ CSV 읽기 → 분석 → 인사이트 정리

### 예시 4: 프로젝트 맥락 유지하며 작업

```
> 봄 캠페인 프로젝트 이어서 할게.
> progress.md 읽고, 블로그 포스트 초안 만들어줘.
```
→ progress.md 확인 → 맥락 파악 → 블로그 초안 작성 → progress.md 업데이트

### 예시 5: 생각 정리

```
> 생각 정리 좀 도와줘.
> 온라인 채널 매출을 높이려면 어떤 전략이 필요할지 고민 중이야.
```
→ 질문으로 시작 → 아이디어 탐색 → 인사이트 정리

---

## 8. 자주 묻는 질문

### Q: Claude Code랑 뭐가 더 좋아요?

기능만 보면 Claude Code가 더 많습니다 (메모리, 스킬, MCP 등).
하지만 Codex CLI는 **심플하고 배우기 쉽습니다.**
AI 에이전트를 처음 써본다면 Codex부터 시작하는 것도 좋은 선택입니다.

### Q: API 비용이 얼마나 드나요?

o4-mini 모델 기준으로 일반적인 작업은 건당 몇 원~수십 원 수준입니다.
하루 활발하게 써도 보통 천 원 미만입니다.

### Q: 인터넷 검색도 되나요?

Codex CLI 자체에는 웹 검색 기능이 없습니다.
검색이 필요한 작업은 ChatGPT 웹에서 하거나, 별도 도구를 활용하세요.

### Q: 기존 파일을 망가뜨리면 어떡하죠?

1. `suggest` 모드를 쓰면 모든 수정을 미리 확인할 수 있습니다
2. Git으로 커밋해두면 언제든 되돌릴 수 있습니다
3. 중요한 작업 전에는 반드시 `git commit` 먼저!

### Q: Codex가 맥락을 까먹어요

progress.md를 업데이트하고, 다음 세션에서 "progress.md 먼저 읽어줘"라고 요청하세요.
루트 AGENTS.md에 활성 프로젝트로 등록하면 더 확실합니다.

---

## 9. 트러블슈팅

| 증상 | 해결 |
|------|------|
| `codex: command not found` | `npm install -g @openai/codex` 재설치 |
| `API key not found` | 환경변수 `OPENAI_API_KEY` 설정 확인 |
| Codex가 AGENTS.md를 안 읽는 것 같음 | 워크스페이스 루트에서 `codex` 실행했는지 확인 |
| 파일 수정이 안 됨 | 승인 모드 확인 — `suggest`면 직접 승인 필요 |
| 한글 파일명 깨짐 | Git 설정: `git config --global core.quotepath false` |

---

## 10. 다음 단계

1. **AGENTS.md 커스터마이징** — 본인 프로필, 자주 하는 작업 추가
2. **첫 프로젝트 만들기** — "새 프로젝트 만들어줘"
3. **데일리 노트 습관** — 매일 "데일리 노트 만들어줘"
4. **progress.md 습관** — 작업 끝나면 "progress 업데이트해줘"

---

*일룸 AX 1.5차 강의 | iloom 영업효율개선팀 AX*
