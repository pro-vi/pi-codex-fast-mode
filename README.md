# Codex Fast Mode Extension

Project-local Pi extension for OpenAI Codex fast mode. The folder is also shaped as a releasable Pi package via `package.json`.

Codex fast mode is not a separate endpoint. In the OpenAI Codex repository it resolves to the Responses API request field:

```json
{ "service_tier": "priority" }
```

Pi's installed `openai-codex-responses` provider already has low-level support for `serviceTier`, but the normal `streamSimple` path does not expose a user-facing toggle. This extension wraps only the `openai-codex-responses` stream and injects `serviceTier: "priority"` when enabled.

## Install

Project-local development:

```bash
pi -e ./.pi/extensions/codex-fast-mode
```

Package install once published or hosted in git:

```bash
pi install npm:pi-codex-fast-mode
pi install git:github.com/pro-vi/pi-codex-fast-mode
```

## Usage

```text
/codex-fast status
/codex-fast on
/codex-fast force
/codex-fast off
/codex-fast help
```

Startup flag:

```bash
pi --codex-fast
```

Modes:

- `off`: default; no request changes.
- `auto`: sends `priority` only for the `openai-codex` provider when the model is found in the current Codex model catalog with a `priority` service tier (`gpt-5.4`, `gpt-5.5`).
- `force`: sends `priority` for any model using Pi's `openai-codex-responses` API.

The footer stays hidden unless fast mode is enabled and the active model is the `openai-codex` provider. In that case it shows a green `fast` marker; the underlying `priority` service tier is implied.

## Package checks

```bash
npm install
npm run typecheck
npm test
npm run smoke-test
npm run check
```

The unit tests exercise `off`, `auto`, and `force` request-option decisions without making network calls.

## Notes

- This changes routing/billing semantics. It is opt-in by default.
- The wrapper calls Pi's existing Codex Responses provider directly so provider-side service-tier cost accounting still sees the requested tier.
- The extension does not modify normal `openai-responses` models; it is scoped to Pi's Codex provider API.
- This extension registers an `openai-codex-responses` provider handler for the current Pi session. Another extension that overrides the same API can conflict; load only one such provider wrapper at a time.
