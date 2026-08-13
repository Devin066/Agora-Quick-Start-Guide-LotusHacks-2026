# Agora Voice AI Recipes

Short, source-linked workflows for the common Agora Voice AI paths. Each recipe starts from an official sample or an official CLI command so the implementation stays aligned with the maintained baseline.

## Recipe 1: Python voice agent

Best default for a first working baseline: Python server plus a browser client.

1. Complete the [CLI readiness check](https://github.com/AgoraIO/cli#installation).
2. Run `agora init my-voice-agent --template python --project <your-project> --json`, or use `--new-project` when a new Agora project is explicitly intended.
3. Run `agora project doctor --feature convoai --json`.
4. Follow the generated quickstart's documented install and start commands.
5. Verify the browser joins RTC and completes a voice round trip.

Official source: [agent-quickstart-python](https://github.com/AgoraIO-Conversational-AI/agent-quickstart-python)

## Recipe 2: Next.js voice agent

Use this when the client and server workflow should live in a TypeScript/Next.js project.

```bash
agora init my-nextjs-voice-agent --template nextjs --project <your-project> --json
cd my-nextjs-voice-agent
agora project doctor --feature convoai --json
```

The official Next.js baseline uses `.env.local`, `NEXT_PUBLIC_AGORA_APP_ID`, and `NEXT_AGORA_APP_CERTIFICATE`. Follow its README for the exact package-manager and startup commands.

Official source: [agent-quickstart-nextjs](https://github.com/AgoraIO-Conversational-AI/agent-quickstart-nextjs)

## Recipe 3: Full agent-samples workspace

Use this when you need voice, video avatar, simple HTML clients, profile-based configuration, or deployment notes.

Official source: [agent-samples](https://github.com/AgoraIO-Conversational-AI/agent-samples)

Useful entry points:

- [Coding-agent guide](https://github.com/AgoraIO-Conversational-AI/agent-samples/blob/main/AGENT.md)
- [Voice client](https://github.com/AgoraIO-Conversational-AI/agent-samples/tree/main/react-voice-client)
- [Video avatar client](https://github.com/AgoraIO-Conversational-AI/agent-samples/tree/main/react-video-client-avatar)
- [Simple backend](https://github.com/AgoraIO-Conversational-AI/agent-samples/tree/main/simple-backend)

## Recipe 4: Studio-managed agent

Use this when an agent has already been configured in [Agora Agent Studio](https://console.agora.io/studio/agents).

1. Keep the Studio Agent ID separate from the runtime `agent_id` returned after a session starts.
2. Start from an official quickstart.
3. Follow the Studio-managed configuration path in the [ConvoAI Studio guide](https://docs.agora.io/en/ai/studio/quickstart).
4. Verify the client and Studio agent use the same RTC channel.

## Recipe 5: Transcripts and agent events

Use RTC for media and RTM for transcripts, agent state, metrics, and errors.

The ConvoAI join configuration must include both flags:

```json
{
  "advanced_features": {
    "enable_rtm": true
  },
  "parameters": {
    "data_channel": "rtm"
  }
}
```

Subscribe the RTM client to the same channel name used by RTC. Parse the event payload as JSON and handle agent state, transcript, metric, and error events according to the selected toolkit or official quickstart.

Reference: [RTC + RTM + ConvoAI integration pattern](https://github.com/agoraio/skills/blob/main/skills/agora/references/integration-patterns.md)

## Recipe 6: Custom LLM

Start with the baseline, then use the official custom LLM sample for the server-side bridge.

Official source: [server-custom-llm](https://github.com/AgoraIO-Conversational-AI/server-custom-llm)

Keep provider-specific fields in the server environment. For MLLM configurations, use `location` where the sample expects it; do not rename it to `region`.

## Recipe 7: Existing-app integration

Do not drop a new ConvoAI scaffold into an existing app before source alignment.

1. Inspect the official Python or Next.js quickstart.
2. Create a copy map from quickstart files to the target app.
3. Adapt only the session lifecycle, auth/token flow, RTC/RTM wiring, and required UI.
4. Preserve the target app's architecture and test start, stop, and voice round trip separately.

Reference: [integration-from-quickstart](https://github.com/agoraio/skills/blob/main/skills/agora/references/conversational-ai/integration-from-quickstart.md)

## Recipe 8: CLI diagnostics and readiness

Use this sequence when setup is unclear:

```bash
agora version
which -a agora
agora doctor --json
agora auth status --json
agora project show --json
agora project doctor --feature convoai --json
```

If the CLI is below `0.1.7`, upgrade before using mutating commands. If `doctor --json` is unavailable, upgrade to `0.2.0+`; the local skill references are verified against `0.2.1`. A healthy `project doctor` result confirms control-plane readiness only; still run the official sample and verify the voice round trip.

Reference: [Agora CLI doctor guide](https://github.com/AgoraIO/cli)

## More recipes and products

- [Agora CLI quickstarts](https://github.com/AgoraIO/cli)
- [Agora coding skills](https://github.com/agoraio/skills)
- [ConvoAI start/stop guide](https://docs.agora.io/en/ai/build/start-stop-agent)
- [ASR provider examples](https://docs.agora.io/en/ai/models/asr/deepgram)
- [LLM provider examples](https://docs.agora.io/en/ai/models/llm/openai)
- [TTS provider examples](https://docs.agora.io/en/ai/models/tts/openai)
- [RTC voice quickstart](https://docs-md.agora.io/en/voice-calling/get-started/get-started-sdk.md)
- [RTM quickstart](https://docs-md.agora.io/en/signaling/get-started/sdk-quickstart.md)
