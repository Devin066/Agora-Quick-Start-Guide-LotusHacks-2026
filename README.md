# Agora Voice AI Starter Kit

Build and ship a real-time voice AI agent with Agora Conversational AI, RTC, and RTM.

This repository is an onboarding guide and recipe index. It intentionally keeps application code in the official Agora quickstarts so your first working baseline follows the maintained sample architecture, commands, environment names, and token flow.

## Start here

Choose the path that matches your goal:

| Goal | Recommended path |
| --- | --- |
| Fastest browser voice agent | [Python quickstart](https://github.com/AgoraIO-Conversational-AI/agent-quickstart-python) |
| Next.js / TypeScript app | [Next.js quickstart](https://github.com/AgoraIO-Conversational-AI/agent-quickstart-nextjs) |
| Go backend | Run `agora init <name> --template go` with the [Agora CLI](https://github.com/AgoraIO/cli) |
| Full sample collection | [Conversational AI agent samples](https://github.com/AgoraIO-Conversational-AI/agent-samples) |
| No-code or low-code agent configuration | [Agora Agent Studio](https://console.agora.io/studio/) |
| Human-to-human voice/video | [Agora RTC voice quickstart](https://docs-md.agora.io/en/voice-calling/get-started/get-started-sdk.md) |
| Chat, presence, or signaling | [Agora RTM quickstart](https://docs-md.agora.io/en/signaling/get-started/sdk-quickstart.md) |

For ready-to-copy workflows, see the [recipe index](recipes/README.md).

## One-command onboarding

The official Agora CLI can create or bind a quickstart, write the template-specific environment file, and persist local project context.

First check the installed CLI before running a mutating command:

```bash
agora version
which -a agora
agora doctor --json
```

The bundled Agora workflow is verified against CLI `0.2.1` and supports CLI versions `>=0.1.7`. The `doctor --json` command requires CLI `0.2.0+`. If the command is missing or the binary is older than the minimum, follow the [CLI install and upgrade guide](https://github.com/AgoraIO/cli#installation) before continuing.

Use an existing Agora project:

```bash
agora login
agora init my-voice-agent --template python --project <your-project> --json
cd my-voice-agent
agora project doctor --feature convoai --json
```

Or create a new project as part of setup:

```bash
agora login
agora init my-voice-agent --template python --new-project --json
cd my-voice-agent
agora project doctor --feature convoai --json
```

The CLI writes the expected keys for each official template:

| Template | Environment file | App ID key | App certificate key |
| --- | --- | --- | --- |
| Python | `server/.env` | `APP_ID` | `APP_CERTIFICATE` |
| Next.js | `.env.local` | `NEXT_PUBLIC_AGORA_APP_ID` | `NEXT_AGORA_APP_CERTIFICATE` |
| Go | `server-go/.env` | `APP_ID` | `APP_CERTIFICATE` |

Use [`agora quickstart env write`](https://github.com/AgoraIO/cli) when binding an already-cloned official quickstart. Do not use the generic project env writer for these templates; it uses different variable names.

## Manual baseline path

If you are not using the CLI, start from an official quickstart and follow its README exactly.

```bash
git clone https://github.com/AgoraIO-Conversational-AI/agent-quickstart-python.git
cd agent-quickstart-python
```

Before adding custom routes or SDK code, inspect the quickstart source and its `README.md`. The baseline is complete only when the app opens, the agent starts, the agent joins the same RTC channel, and you can speak and hear TTS back.

For a larger multi-client sample with voice, video avatar, simple HTML clients, profiles, and production notes, use [agent-samples](https://github.com/AgoraIO-Conversational-AI/agent-samples) and read its [coding-agent guide](https://github.com/AgoraIO-Conversational-AI/agent-samples/blob/main/AGENT.md).

## What you are building

```text
Browser client
  ├─ RTC: microphone audio and agent audio
  └─ RTM: transcripts, state, metrics, and errors
        │
        ▼
Your app server ── POST /join ──► Agora Conversational AI
                                      │
                                      ▼
                                ASR → LLM → TTS
```

The app server starts and stops the cloud agent. The browser joins the same RTC channel and listens for agent audio. When RTM events are enabled, the client also subscribes to the RTM channel with the same name as the RTC channel.

### ConvoAI integration rules

- Keep App ID and App Certificate on the server. Never expose the App Certificate or REST credentials in browser code.
- Use the official quickstart's documented auth and environment names for the first successful run.
- For direct REST integrations, prefer scoped Agora token auth; use Basic Auth only when the integration explicitly requires it.
- A successful `/join` request means the agent is starting, not that it is already in the channel. Wait for the RTC `user-joined` event before expecting audio.
- To receive agent events over RTM, set both `advanced_features.enable_rtm: true` and `parameters.data_channel: "rtm"`.
- Use the same channel name for RTC and RTM. RTM user IDs are strings, and the token subject must match the ID used at RTM login.
- Stop the agent from the server before the client leaves the RTC channel.

## Credentials

Create an Agora project in the [Agora Console](https://console.agora.io/), enable the App Certificate, and keep credentials in the server-side environment file.

At minimum, the official quickstarts need:

```dotenv
APP_ID=your_agora_app_id
APP_CERTIFICATE=your_agora_app_certificate
```

The exact file and variable names are template-specific; use the table above or let `agora quickstart env write` populate them. Never commit `.env`, `.env.local`, App Certificates, Customer Secrets, provider API keys, or generated tokens.

## Recipes

The [recipe index](recipes/README.md) collects focused next steps:

- [Start a Python voice agent](recipes/README.md#recipe-1-python-voice-agent)
- [Start a Next.js voice agent](recipes/README.md#recipe-2-nextjs-voice-agent)
- [Use the full agent-samples workspace](recipes/README.md#recipe-3-full-agent-samples-workspace)
- [Reuse an Agent Studio configuration](recipes/README.md#recipe-4-studio-managed-agent)
- [Add live transcripts and agent events](recipes/README.md#recipe-5-transcripts-and-agent-events)
- [Build a custom LLM bridge](recipes/README.md#recipe-6-custom-llm)
- [Move from a baseline into an existing app](recipes/README.md#recipe-7-existing-app-integration)
- [Use the Agora CLI as an onboarding helper](recipes/README.md#recipe-8-cli-diagnostics-and-readiness)

## Further reading

| Topic | Official resource |
| --- | --- |
| Conversational AI overview | [Agora Voice AI quickstart](https://docs.agora.io/en/ai/get-started/quickstart) |
| Agent Studio | [Create your first Studio agent](https://docs.agora.io/en/ai/studio/quickstart) |
| Start and stop an agent | [ConvoAI start/stop guide](https://docs.agora.io/en/ai/build/start-stop-agent) |
| ASR providers | [ASR provider examples](https://docs.agora.io/en/ai/models/asr/deepgram) |
| LLM providers | [LLM provider examples](https://docs.agora.io/en/ai/models/llm/openai) |
| TTS providers | [TTS provider examples](https://docs.agora.io/en/ai/models/tts/openai) |
| Custom LLM | [Custom LLM server sample](https://github.com/AgoraIO-Conversational-AI/server-custom-llm) |
| MCP tools | [MCP server sample](https://github.com/AgoraIO-Conversational-AI/server-mcp) |
| Agora CLI | [CLI repository and commands](https://github.com/AgoraIO/cli) |
| Agora coding skills | [Agora skills repository](https://github.com/agoraio/skills) |
| RTC tokens | [Token server guide](https://docs-md.agora.io/en/video-calling/token-authentication/deploy-token-server.md) |

## Troubleshooting checklist

1. Run `agora version`, `which -a agora`, and `agora doctor --json`.
2. Run `agora project doctor --feature convoai --json` for project readiness. A healthy doctor result is control-plane readiness, not proof of an end-to-end call.
3. Confirm the quickstart's expected environment file and variable names.
4. Confirm RTC and RTM use the same channel name and that the RTM token subject matches the RTM login user ID.
5. If RTM was just enabled, allow a bounded wait/retry; control-plane enablement can arrive before the runtime service is usable.
6. Inspect the agent's RTC join event before debugging missing audio.

## Repository conventions

The repository-level [AGENTS.md](AGENTS.md) contains the source-of-truth workflow for coding agents. Keep this repository focused on onboarding and recipes; application implementation should be copied or adapted from an official quickstart after its source has been inspected.
