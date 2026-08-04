---
name: emile-handoff
description: Use when the current session's context is getting large (approaching roughly 200k tokens) and work needs to continue in a fresh session — creates a handoff document summarizing progress, decisions, and next steps so a new session can pick up where this one left off.
---

# Emile Handoff

## Overview

현재 세션에서 진행된 작업 내용과 결과를 정리해 `docs/handoffs/` 폴더에 마크다운 파일로 저장하는 스킬. 새 세션에서 이 파일을 읽으면 이전 작업 맥락을 빠르게 파악하고 이어서 작업할 수 있다.

## When to Use

- 현재 세션의 컨텍스트가 커져서 (대략 20만 토큰 근처) 압축/초기화가 임박했을 때
- 작업을 새 세션으로 넘겨야 할 때
- 사용자가 "핸드오프 만들어줘", "다음 세션에 이어서" 등을 요청할 때

## Steps

1. **폴더 확인/생성**: 현재 작업 디렉터리 기준 `docs/handoffs/` 폴더가 있는지 확인하고, 없으면 생성한다.
2. **파일명 생성**: `{YYYYMMDD}-{HHMM}-{간략한-제목}.md` 형식. 날짜/시간은 시스템 로컬 시각 기준(`date +%Y%m%d-%H%M`). 제목은 이번 세션 작업 내용을 3~6단어로 요약한 kebab-case (한글 가능).
   - 예: `20260724-1530-auth-refactor-handoff.md`
3. **내용 작성**: 아래 템플릿을 채워 넣는다. 이번 세션에서 실제로 있었던 작업만 기록하고, 추측하거나 지어내지 않는다.

## Template

```markdown
# Handoff: {제목}

**작성 시각**: {YYYY-MM-DD HH:MM}

## 다음 세션 시작 프롬프트

> 아래 블록을 복사해서 새 세션(/clear 후) 입력창에 그대로 붙여넣으세요.

```
{다음 세션 입력창에 그대로 붙여넣을 프롬프트. "docs/handoffs/{파일명}을 읽고 이어서 작업해줘" 같은 지시에 더해, 지금 상태와 다음 할 일을 2~4문장으로 요약해 새 세션이 파일을 열기 전에도 맥락을 잡을 수 있게 한다.}
```

## 작업 배경 / 목표
{왜 이 작업을 시작했는지, 무엇을 달성하려 했는지}

## 완료된 작업
{이번 세션에서 실제로 완료한 것들 — 구체적으로}

## 현재 상태
{코드/문서가 지금 어떤 상태인지, 진행 중이던 것}

## 남은 작업 / 다음 단계
{다음 세션에서 이어서 해야 할 일}

## 주요 결정 사항
{왜 이렇게 했는지에 대한 근거, 트레이드오프. 새 세션이 같은 고민을 반복하지 않도록}

## 참고 파일 / 경로
{이번 작업에서 건드린 주요 파일 경로들}

## 기타 참고사항
{막힌 부분, 주의할 점, 재현이 어려운 컨텍스트 등}
```

4. **저장 후 확인**: 생성된 파일의 전체 경로를 사용자에게 알려준다.
5. **다음 세션 프롬프트를 채팅에도 출력**: 템플릿에 작성한 "다음 세션 시작 프롬프트" 블록을, 파일 저장과는 별도로 현재 세션의 응답 메시지에도 코드 블록으로 그대로 출력한다. 사용자가 이 응답에서 바로 복사해 `/clear` 후 붙여넣을 수 있어야 한다.

## Notes

- 이 스킬은 문서 생성만 담당한다. git commit 여부는 별도로 사용자에게 확인한다 (자동 커밋하지 않음).
- `docs/handoffs/`가 이미 다른 용도로 존재하는 폴더라면 사용자에게 확인한다.
