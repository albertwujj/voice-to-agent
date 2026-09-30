# voice-to-agent

Agent instructions for interpreting voice input, supported by [AgentTerm](https://github.com/albertwujj/agent-term). The agent uses your conversation and project context to repair transcription errors before acting: “pie test” can become `pytest`.

<a name="install"></a>

## Adding it

Ask your agent:

```text
Clone the repository below into ai/ in this project, and leave ai/ out
of .gitignore.
https://github.com/albertwujj/voice-to-agent
```

Clone it on the machine running your agent, inside WSL on Windows. Keep the folder name `voice-to-agent`; other locations work too ([placement](https://github.com/albertwujj/agent-term/blob/main/docs/conventions.md#placement)).

<a name="how-its-used"></a>

## Using it

AgentTerm automatically references [`interpret.md`](interpret.md) with each voice transcript from its [phone viewer](https://github.com/albertwujj/agent-term/blob/main/docs/phone.md). The guide tells the agent to repair misheard terms, follow spoken corrections, and ask when the transcript is too garbled to interpret reliably.

Recording and transcription are provided by [agent-stream-hub's voice setup](https://github.com/albertwujj/agent-stream-hub/blob/main/docs/setup.md#optional-voice-input).

<a name="files"></a>

## The mechanics

For the transcript format, integration with other sources, and the reasoning behind the guide, see [guide design](.maintainer/guide-design.md).

## License

[MIT](LICENSE).
