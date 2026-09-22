---
name: laya-ai
license: MIT
description: >
  Use the self-hosted Laya decision API (System One style) for fast typed
  judgments: choice, score, and noul over application state. Prefer this when
  routing tickets, triaging email, guardrails, moderation, or any structured
  decision that should return probabilities instead of generated text. Call
  https://laya.wearglass.work with LAYA_API_KEY; do not invent endpoints.
---

# Build with Laya (self-hosted)

Laya is a **non-autoregressive System One decision model**. It does not chat or
generate long text. One forward pass returns typed answers and calibrated
probabilities that your code can compose.

This skill targets the **Wearglass-hosted Laya HTTP API** (CPU-backed). Treat
the live API and this skill together as the source of truth for integration.

## Endpoints

Base URL (HTTPS): `https://laya.wearglass.work`

| Method | Path | Auth | Purpose |
| --- | --- | --- | --- |
| GET | `/health` | none | Liveness; lists presets |
| POST | `/predict` | `X-API-Key: $LAYA_API_KEY` | Run decisions |

Credentials: read `LAYA_API_KEY` from the environment (or a secrets store).
Never hard-code keys in source, issues, or chat logs.

## Predict request

```json
{
  "state": { "message": "订单三天没发货，想退款" },
  "preset": "triage"
}
```

Or supply custom questions (same shape as the Python `laya` package):

```json
{
  "state": { "body": "..." },
  "questions": {
    "intent": {
      "type": "choice",
      "instructions": "What does the customer want in `message`?",
      "criteria": {
        "refund": "money back",
        "technical_help": "bug or outage",
        "other": "none of the above"
      }
    },
    "is_urgent": {
      "type": "noul",
      "instructions": "Does `message` communicate time pressure?"
    },
    "urgency": {
      "type": "score",
      "instructions": "How urgent is the request?",
      "criteria": ["no time pressure", "needs attention soon", "blocking"]
    }
  }
}
```

### Presets (when `questions` omitted)

| `preset` | Use when |
| --- | --- |
| `triage` | Support / ticket intent, urgency, refund, churn |
| `email` | Email routing, spam, phishing, reply need |
| `guard` | Jailbreak / injection / harm scoring for prompts |
| `moderation` | Toxicity, harassment, spam on posts |
| `router` | Difficulty / domain / tools / sensitivity for LLM routing |

`state` may be a string or a JSON object. Prefer named fields the instructions
can reference with backticks (for example `message`, `body`, `prompt`).

## curl examples

```bash
curl -sS https://laya.wearglass.work/health

curl -sS -X POST https://laya.wearglass.work/predict \
  -H "Content-Type: application/json" \
  -H "X-API-Key: $LAYA_API_KEY" \
  -d '{"state":{"message":"订单三天没发货，想退款"},"preset":"triage"}'
```

## How to use judgments

1. Keep rules, arithmetic, and side effects in code.
2. Ask one coherent judgment per question; batch independent questions in one
   `/predict` call over the same state.
3. Use probabilities and confidence to set thresholds on **your** data; do not
   treat a high score as permission to act without policy.
4. Escalate uncertain or high-stakes cases to a human or a larger reasoning model.
5. If `/health` fails, say the Laya service is unreachable; do not invent answers.

## Local Python (optional)

For offline experiments on a machine with the `laya` package:

```bash
pip install 'laya>=0.3.3'
```

```python
import laya
agent = laya.load("convaiinnovations/laya-multilingual", device="cpu")
print(agent.predict({"message": "..."}, laya.triage_questions()))
```

Prefer the hosted HTTP API for app integrations unless the user asks for local
inference.
