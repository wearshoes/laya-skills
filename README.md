# Wearglass skills

Agent skills for Wearglass-hosted tools. Start with **laya-ai**: a System One
decision API for typed judgments (choice / score / noul).

## Install prompt (paste into an agent)

```text
Install the Laya skill. If you're in Claude Code, run `claude plugin marketplace add wearshoes/wearglass-skills`, then `claude plugin install laya-ai@wearshoes/wearglass-skills`. If you're in another agent, run `npx skills add wearshoes/wearglass-skills --skill laya-ai` and select your agent. Use one installation method. You can read the skill directly at https://github.com/wearshoes/wearglass-skills/blob/main/skills/laya-ai/SKILL.md (raw: https://raw.githubusercontent.com/wearshoes/wearglass-skills/main/skills/laya-ai/SKILL.md). Then use the Laya skill when the task needs fast typed decisions (routing, triage, guardrails, moderation) via https://laya.wearglass.work with LAYA_API_KEY.
```

Replace `wearshoes` with the GitHub user or org that owns this repository.

## Layout

```
skills/
  laya-ai/
    SKILL.md
```

## API

- Base: `https://laya.wearglass.work`
- Auth header for `/predict`: `X-API-Key: $LAYA_API_KEY`
