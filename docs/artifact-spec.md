# Artifact specification

A **capability artifact** is the typed, versioned record of a UI task. It is produced by discovery and
consumed by replay and the catalog. The Zod schema in [`src/artifact/schema.ts`](../src/artifact/schema.ts)
is the single source of truth; every other module imports its inferred types.

The exported JSON Schema is committed at [`schema/capability.schema.json`](../schema/capability.schema.json).
Regenerate it with `npm run schema:export`.

Artifacts are validated with Zod on load, and `provenance.checksum` covers the behavior-defining fields so a
tampered or drifted artifact is rejected.

## Capability

| Field | Type | Notes |
|---|---|---|
| `schemaVersion` | `"1.0.0"` | Literal. |
| `capabilityId` | string | Stable id, for example `member.read-savings-balance`. |
| `version` | string | Semver of this capability. |
| `name`, `description` | string | Human-readable. |
| `labels` | string[] | Default `[]`. |
| `target` | object | `appFamily`, `surfaceKind` (`web` \| `desktop`, default `web`), `entryPoint`, optional `compatibleVersions`. Keyed by app family, not a tenant URL. |
| `inputs` | `Param[]` | Typed inputs. |
| `outputs` | `OutputField[]` | Typed extraction contract. |
| `steps` | `Step[]` | Ordered, at least one. |
| `successCondition` | `Checkpoint` | Goal-level success. |
| `businessOutcomes` | `BusinessOutcome[]` | Declared expected non-success results. |
| `recovery` | `RecoveryRule[]` | Global rules evaluated at every step boundary. |
| `guardrails` | object | `allowlist: { routes, actions }`. |
| `provenance` | object | `discoveredBy {provider, model}`, `discoveredAt`, `sourceRunId`, `evidenceRef`, `checksum`. The raw transcript is not inlined. |

## Step

`id`, `intent`, `action`, optional `target` (`LocatorBundle`), optional `inputBinding { param }`, optional
`literalValue`, optional `precondition` and `postcondition` (`Checkpoint`), `risk` (default `safe`),
`policy` (default `auto`), and `recover` (`RecoveryRule[]`, step-specific).

**Actions:** `navigate` (with `to`), `click`, `type`, `select`, `read`, `waitFor` and `assert` (both with a `checkpoint`).

## Locators

A `LocatorBundle` is an ordered list of `strategies` (robust to brittle), an optional `framePath`, and
`invariants` (`role`, `name`, `editable`, `visible`) that must hold after resolution.

| Strategy (`by`) | Fields |
|---|---|
| `role` | `role`, `name`, optional `exact` (accessibility tree). |
| `label` | `label`, optional `scope`. |
| `rowAnchor` | `header`, `cell`, `targetCol`: "the target column cell of the row whose header cell reads X". |
| `attr` | `name`, `value`. |
| `text` | `text`, optional `exact`. |
| `structural` | `css`, `index`. Last resort; its use is logged as a drift signal. |

## Checkpoints

`urlMatches {pattern}`, `elementVisible {locator}`, `textPresent {text, scope?}`, `textAbsent {text}`,
`valueEquals {locator, value}`.

## Inputs and outputs

- `Param`: `name`, `type` (`string` \| `number` \| `boolean` \| `enum`), `required` (default true), `description`,
  optional `example` and `enumValues`, and `sensitive` (default false). Sensitive values are redacted before persistence.
- `OutputField`: `name`, `type`, `description`, and `source { fromStepId, extract (text | value | attribute), attribute? }`.

## Business outcomes and recovery

- `BusinessOutcome`: `code`, `detect` (`Checkpoint`), optional `returns`, `terminal` (default true).
- `RecoveryRule`: `when` (`Checkpoint`), `do` (`dismiss` \| `retry` \| `reauth`), optional `target` (for `dismiss`),
  optional `framePath` (for `retry`), and `maxAttempts` (default 2).

## Risk and policy

`risk`: `safe` \| `sensitive` \| `irreversible`. `policy`: `auto` \| `confirm` \| `block`. See [safety.md](./safety.md).

## Example

A full, real artifact is committed at [`catalog/member.read-savings-balance.json`](../catalog/member.read-savings-balance.json).
