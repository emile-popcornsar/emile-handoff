# emile-handoff

[Claude Code](https://claude.com/claude-code) skill that writes a session handoff document so work can continue cleanly in a fresh session.

세션 컨텍스트가 커져서(대략 20만 토큰 근처) 곧 압축/초기화될 것 같을 때, 지금까지의 진행 상황·결정 사항·다음 할 일을 `docs/handoffs/`에 마크다운으로 정리해두는 Claude Code 스킬입니다. 새 세션에서 이 파일을 읽으면 이전 맥락을 빠르게 복원할 수 있습니다.

## When it triggers

- 현재 세션의 컨텍스트가 커져서 압축/초기화가 임박했을 때
- 작업을 새 세션으로 넘겨야 할 때
- 사용자가 "핸드오프 만들어줘", "다음 세션에 이어서" 등을 요청할 때

## What it produces

`docs/handoffs/{YYYYMMDD}-{HHMM}-{간략한-제목}.md` 파일 하나에 아래 항목을 채워 넣습니다.

- 다음 세션 시작 프롬프트 (그대로 복사해서 붙여넣을 수 있는 블록)
- 작업 배경 / 목표
- 완료된 작업
- 현재 상태
- Git 상태 (브랜치, 이번 핸드오프의 커밋 범위, 커밋 안 된 변경사항)
- 남은 작업 / 다음 단계
- 주요 결정 사항
- 참고 파일 / 경로
- 기타 참고사항

파일을 저장한 뒤에는 "다음 세션 시작 프롬프트" 블록을 채팅 응답에도 그대로 출력해, 사용자가 바로 복사해 `/clear` 후 붙여넣을 수 있게 합니다.

## Commit

커밋 여부는 **문서를 쓰기 전에 한 번만** 묻습니다. 핸드오프 파일만 커밋할지, 이번 세션 변경사항까지 함께 커밋할지, 아니면 커밋하지 않을지를 먼저 정하고, 그 결정을 문서의 `Git 상태` 항목에 기록한 뒤 작성이 끝나면 그대로 실행합니다. 문서가 완성된 뒤에 커밋 여부를 되묻지 않기 때문에, 세션을 넘기기 직전에 대화가 한 번 더 끊기지 않습니다.

git 저장소가 아니거나 사용자가 지시에서 이미 의사를 밝힌 경우에는 묻지 않습니다. 어느 경우에도 사용자가 정해주지 않은 커밋을 임의로 만들지는 않습니다.

## Install

Claude Code는 `~/.claude/skills/<name>/`(전역) 또는 `<repo>/.claude/skills/<name>/`(프로젝트 한정) 아래의 `SKILL.md`를 스킬로 인식합니다.

```bash
git clone https://github.com/emile-popcornsar/emile-handoff.git ~/.claude/skills/emile-handoff
```

또는 특정 프로젝트에서만 쓰려면 해당 리포의 `.claude/skills/emile-handoff/`에 두면 됩니다.

## Usage

Claude Code 세션에서:

```
/emile-handoff
```

또는 자연어로 "핸드오프 만들어줘"라고 요청하면 스킬 설명(description)을 보고 자동으로 매칭됩니다.

## Related

- [emile-masterplan](https://github.com/emile-popcornsar/emile-masterplan) — 여러 세션·여러 plan에 걸치는 대형 작업 묶음을 위한 상위 진행 상황 문서 스킬. 슬라이스 **중간**에 세션이 끊길 때는 emile-masterplan이 아니라 이 스킬(emile-handoff)을 사용합니다.

## License

MIT
