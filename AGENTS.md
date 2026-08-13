# AGENTS.md

## Purpose

This repository is an Agora Voice AI onboarding guide and recipe index. It is not a replacement ConvoAI application scaffold.

## Working rules

1. Use the local Agora skill references as the source of truth for Agora integration details.
2. For ConvoAI implementation work, inspect the official quickstart before generating SDK code, `/join` payloads, routes, or a new app structure.
3. Preserve the official sample's architecture, environment names, lifecycle, and documented startup commands during the first successful run.
4. Prefer the Python quickstart when no stack is specified. Use the Next.js or Go quickstart when the user explicitly chooses that stack.
5. Keep secrets out of this repository. Do not commit `.env`, `.env.local`, App Certificates, Customer Secrets, provider keys, or tokens.
6. Treat `agora project doctor` as a control-plane check only. Do not claim an end-to-end baseline until the agent joins RTC and a voice round trip has been verified.
7. When editing documentation, link to official Agora repositories and docs, and state when a command or version is verified against the local skill references.

## Validation

For documentation-only changes:

```bash
git diff --check
rg -n "https?://|^#|^##|^###" README.md AGENTS.md recipes/README.md
```

For application changes, use the exact startup and verification commands documented by the selected official quickstart.
