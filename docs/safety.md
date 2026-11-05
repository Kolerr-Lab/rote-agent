# Safety

ROTE applies several layers of control before anything irreversible happens. They apply in both
discovery and replay.

## Allowlist

[`src/safety/allowlist.ts`](../src/safety/allowlist.ts) defines a `SafetyGate` built from a list of permitted
**routes** (globs) and **action types**. It is checked on every action. `**` matches across path segments and
`*` matches within one segment. Anything outside the allowlist is refused and surfaced as
`GUARDRAIL_BLOCKED`, never silently performed.

The default allowlist lives in [`config/allowlist.json`](../config/allowlist.json), and each artifact also
carries its own `guardrails.allowlist`.

## Risk policy

[`src/safety/risk.ts`](../src/safety/risk.ts) classifies each discovered step:

- `read`, `assert`, `waitFor` are `safe`.
- `navigate` and `click` are `irreversible` when the intent text matches verbs such as submit, confirm,
  transfer, delete, remove, create, post, approve or send; otherwise `safe`.
- `type` and `select` default to `safe`. Sensitive inputs are marked in the artifact's `inputs` and redacted.

Default policy: `irreversible` maps to `confirm` (routed to a human), `sensitive` and `safe` map to `auto`.
`block` refuses the step. The classification is a heuristic starting point; review artifacts for regulated
workflows.

## Redaction

[`src/safety/redaction.ts`](../src/safety/redaction.ts) runs inside the evidence sink, so values are masked
before anything is written to disk. It masks:

- values bound to `sensitive` inputs (for example a password), and
- patterns that look like secrets: card-like numbers, US SSNs, API keys (`sk-`, `pk-`, `rk-`), and bearer tokens.

The pattern list is conservative, not exhaustive. Secrets are read from the environment only and are not committed.

## Escalation

When replay reaches a `confirm` step or an unrecoverable state, it returns `escalation_required` with an
`InterventionRequest` (goal, stopped step, screenshot, what is needed). A `SessionController` holds the control
lease (`AGENT` or `HUMAN`):

`RUNNING -> PAUSED -> HUMAN_CONTROL -> RESUMING -> RUNNING`

A human operates the same live browser session, every human action is recorded in the trace, and replay
resumes from the correct step: after the step for a risky confirmation the human performed, or re-running it
after a re-authentication. The bundled operator panel (`npm run operator`) is intentionally minimal.
