# ROTE documentation

| Document | What it covers |
|---|---|
| [architecture.md](./architecture.md) | Components, data flow, determinism, error handling, trade-offs. Start here. |
| [artifact-spec.md](./artifact-spec.md) | The capability artifact schema and where the exported JSON Schema lives. |
| [safety.md](./safety.md) | Allowlist, risk policy, redaction, and human escalation. |
| [design-notes.md](./design-notes.md) | Detailed design rationale and what was deliberately left out. |
| [governance/](./governance) | Engineering protocols: the blueprint (system specification), prompt, tracelog, context-window and tracking protocols. |

The blueprint at [`governance/blueprint-protocol.md`](./governance/blueprint-protocol.md) is the
source of truth. If code and blueprint disagree, the blueprint is amended or the code is fixed.
