# Codex Fast Mode for Pi

Tiny Pi extension that puts OpenAI Codex on the fast lane.

Codex's Fast button is just this request field:

```json
{ "service_tier": "priority" }
```

This wraps Pi's `openai-codex-responses` provider and adds that field when you ask for it. The footer stays quiet unless fast is actually armed:

![Pi footer showing the fast marker](docs/images/codex-fast-mode.png)

This is purely vibecoded. I read Codex source, made the smallest shim, then added tests so the vibe has guardrails. It can change routing and billing. Don't turn it on if you don't want priority-tier behavior.

## Install

```bash
pi install git:github.com/pro-vi/pi-codex-fast-mode
```

## Use

```text
/codex-fast on      auto mode for known fast Codex models
/codex-fast force   priority for any openai-codex-responses model
/codex-fast off
/codex-fast status
```

Startup:

```bash
pi --codex-fast
```

## Modes

`off` is default.

`auto` only targets `openai-codex` models Codex currently marks fast-capable: `gpt-5.4`, `gpt-5.5`.

`force` sends priority for any `openai-codex-responses` model.

## Dev

```bash
npm install
npm run check
```

It does not touch normal `openai-responses` models. It does override Pi's `openai-codex-responses` handler for the session, so don't load another wrapper for that same API unless you like adapter cage fights.
