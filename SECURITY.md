# Security Policy

## Reporting a vulnerability

Please **do not** open a public issue for security problems.

Email **security@blazle.io** or **ricky@kolerr.com** with:

- a description of the issue and its impact,
- steps to reproduce (a minimal artifact or command if possible),
- the version or commit you tested.

We aim to acknowledge reports within a few business days. Please give us reasonable time to investigate and
fix before any public disclosure.

## Scope

In scope: the code in this repository, including the allowlist (`src/safety/allowlist.ts`), redaction
(`src/safety/redaction.ts`), artifact validation, and the escalation/handoff layer.

Reports about leaked credentials in committed files or in generated artifacts and traces are especially welcome.

## Supported versions

ROTE is early (0.x). Only the latest commit on `main` receives security fixes.
