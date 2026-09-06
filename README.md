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
[<absolute path to interpret.md>]
<raw transcript>
```

The reference names the guide as the action to take; the agent reads it, repairs the
transcript against session context (no pipeline stage corrects it — the agent has the most
context), and acts. A source that cannot resolve the path sends
`[@voice-to-agent/interpret.md]` instead and the agent locates the guide itself; see
[.maintainer/guide-design.md](./.maintainer/guide-design.md) for why the resolved form is
preferred.

## Files

- **[interpret.md](./interpret.md)** — *runtime, agent-facing.* Kept concise; it enters the
  agent's context on every voice turn.
- **[.maintainer/guide-design.md](./.maintainer/guide-design.md)** —
  *maintainer-facing.* Why the guide and the injected prompt read the way they do;
  edit the guide with it open. Maintainer docs live under `.maintainer/` so the repo
  root is exactly the vendored runtime surface.

## Install

Clone this repo where a source will look for it. Lookup is by folder name, nearest
first, so one clone can serve one project or every project on the machine:

- **`~/voice-to-agent`** — serves every project. The usual choice, and the one to make
  if you are unsure.
- **`<repo>/ai/voice-to-agent`** — serves that project and wins over a home clone there.
  `ai/` holds the whole vendored set behind one line in `git status`.
- **`<repo>/voice-to-agent`**, or any directory above the repo — also found.

Two things to keep right. The folder name is the namespace the reference uses, so keep
it. And the lookup walks directories rather than searching, so a clone tucked inside a
folder of your own (`tools/`, `third_party/`) is not on that path and will not be found.

agent-term's [placement guide](https://github.com/albertwujj/agent-term/blob/main/docs/conventions.md)
covers the same ladder for every kit in the suite, including why a clone inside a repo
overrides one under home.
