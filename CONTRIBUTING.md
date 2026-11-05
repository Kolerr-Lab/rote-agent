# Contributing to ROTE

Thanks for your interest. ROTE is early; APIs and the artifact schema may change, so please open an issue
to discuss larger changes before investing in a pull request.

## Setup

Requires Node 20+.

```bash
npm install
npx playwright install chromium
npm run typecheck
npm test
```

Tests run fully offline against the bundled mock bank and the `MockLlmProvider`. No API key is needed.

## Ground rules

- **The blueprint is the source of truth.** See [`docs/governance/blueprint-protocol.md`](./docs/governance/blueprint-protocol.md).
  If your change alters behavior it describes, amend the blueprint in the same PR (with a changelog line).
- **Keep the seams.** `replay` must never import `llm`. Only `WebSurface` talks to a browser, and only
  the LLM providers talk to a model.
- **Schema changes** go in `src/artifact/schema.ts` first. Run `npm run schema:export` and commit the result.
- **Tests travel with code.** New behavior needs a deterministic, offline test.
- **No secrets.** Never commit keys or real credentials. Use environment variables (see `.env.example`).
- **Small, focused PRs** with clear commit messages (for example `fix: ...`, `feat: ...`, `docs: ...`).

## Before you open a PR

- [ ] `npm run typecheck` passes
- [ ] `npm test` passes
- [ ] Docs and `CHANGELOG.md` are updated if behavior or the schema changed

## Reporting bugs and security issues

Use the issue templates for bugs and feature requests. For vulnerabilities, follow [SECURITY.md](./SECURITY.md)
and do not open a public issue.

By contributing you agree that your contributions are licensed under the [Apache-2.0 License](./LICENSE) and
that you will follow the [Code of Conduct](./CODE_OF_CONDUCT.md).
