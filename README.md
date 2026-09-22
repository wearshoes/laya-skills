# Laya skills

Agent skills for the Wearglass-hosted **Laya** decision API (System One style typed judgments).

## Install prompt (paste into an agent)

```text
Install the Laya skill. If you're in Claude Code, run `claude plugin marketplace add wearshoes/laya-skills`, then `claude plugin install laya-ai@wearshoes/laya-skills`. If you're in another agent, run `npx skills add wearshoes/laya-skills --skill laya-ai` and select your agent. Use one installation method. You can read the skill directly at https://github.com/wearshoes/laya-skills/blob/main/skills/laya-ai/SKILL.md (raw: https://raw.githubusercontent.com/wearshoes/laya-skills/main/skills/laya-ai/SKILL.md). Then use the Laya skill when the task needs fast typed decisions (routing, triage, guardrails, moderation) via https://laya.wearglass.work with LAYA_API_KEY.
```

## Layout

```
skills/
  laya-ai/
    SKILL.md
```

## API

- Base: `https://laya.wearglass.work`
- Auth header for `/predict`: `X-API-Key: $LAYA_API_KEY`