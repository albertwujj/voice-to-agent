# voice-to-agent

The agent-facing instructions for voice input: a vendored guide that tells an AI
coding CLI how to reconstruct and act on a raw speech-to-text transcript. This repo
owns the guide and its maintenance — nothing else. The system that records,
transcribes, and delivers transcripts lives in
[agent-stream-hub](https://github.com/albertwujj/agent-stream-hub) (see its `voice.md`)
and [agent-term](https://github.com/albertwujj/agent-term).

## How it's used

A source injects each spoken command as:

```
[@voice-to-agent/interpret.md]
<raw transcript>
```

The `@`-reference names the guide as the action to take; the agent reads it, repairs the
transcript against session context (no pipeline stage corrects it — the agent has the most
context), and acts.

## Files

- **[interpret.md](./interpret.md)** — *runtime, agent-facing.* Kept concise; it enters the
  agent's context on every voice turn.
- **[.maintainer/guide-design.md](./.maintainer/guide-design.md)** —
  *maintainer-facing.* Why the guide and the injected prompt read the way they do;
  edit the guide with it open. Maintainer docs live under `.maintainer/` so the repo
  root is exactly the vendored runtime surface.

## Install

Vendor this repo into any project you want to drive by voice — anywhere in the tree
(`tools/`, `third_party/`, the root), keeping the folder name `voice-to-agent`.
