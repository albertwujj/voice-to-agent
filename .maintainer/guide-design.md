# guide-design

**Maintainer-facing.** Rationale behind [interpret.md](../interpret.md) — why each line is
there and why others were removed. Edit the guide with these in mind. The *system*
that delivers transcripts (hub, `/voice` API, whisper config, phone UX, source
injection) is documented in `voice.md` in the
[agent-stream-hub](https://github.com/albertwujj/agent-stream-hub) repo; this repo owns
only the agent-facing instructions and this file.

## The premise: the agent is the corrector

No pipeline stage corrects the transcript. The agent CLI holds the whole session —
files, conversation, tools — so it out-corrects any hub-side model or acoustic-stage
biasing. The guide's job is to frame the raw transcript so the agent (a) knows it's
dictated speech, (b) repairs it against session context, (c) acts without adding
friction. Whisper's errors are *phonetically faithful* ("pie test"), so the
sound-shadow survives into text — the guide's repair rules work on that shadow.

## The injected prompt (contract with sources)

```
[@voice-to-agent/interpret.md]
<raw transcript>
```

Reference and filename co-designed:

- **`@voice-to-agent/interpret.md` — the reference *is* the action.** A reference meant
  to be acted on must carry the verb, or it reads as an inert mention. The earlier form
  put the verb in a surrounding imperative around a noun filename ("find
  interpretation-guide.md and follow it"); here the verb moves into the filename —
  `interpret.md`, referenced bare, *is* the action ("interpret this"). Same invariant, the
  action-reference carries the verb, with no surrounding words. `@` is the file-reference
  gesture a user already makes via a CLI's picker, so the model reads `@…/interpret.md` as
  "read this file and act on it." This is interpretation, not mechanical `@`-expansion, so
  the agent locates the guide wherever the kit is vendored (globs the folder) rather than
  resolving a fixed cwd-relative path.
- **`[…]` frames it as meta, not content.** The reference is the envelope, not the request
  — matching the guide's own last line ("framing, never the task"). It also parallels the
  host's own injections into this stream (`[Notice from terminal host]` /
  `[Warning from terminal host]`): a bracketed line is meta; the content is what follows on
  the next line.
- **Folder-qualified, no path.** Kits are vendored whole, folder name kept, anywhere in
  the tree — the folder is the namespace, so no directory path is needed. A bare filename
  could collide with a project's own docs.
- **Reference every time, no state.** No primed flag, no compaction detection in the
  source. The agent locates the guide, remembers it, and re-reads only if it was compacted
  out of context — it self-manages caching and location.
- **Degrades when absent (no explicit label).** With the verb in the reference, the agent
  still knows the action if the guide can't be read — `interpret.md` names "interpret the
  text that follows." What's lost is the dictation framing and the repair rules, which now
  live only in the guide; the earlier "User input transcript —" label carried a weaker
  version of that framing in the envelope itself, and this form trades that redundancy for
  zero surrounding words. No separate "Transcript:" label follows — the guide defines the
  text after the reference as the transcript.
- **Transcript on its own line** — agent-term's convention for injected prompts; sources
  deliver via bracketed paste (the terminal escape, distinct from the literal `[…]` above)
  so the newline doesn't submit early.

## Why the guide reads the way it does

- **Bold trigger line first** — family runbook convention: state when it applies and
  what to do, before anything else.
- **Concise (~200 words), zero embedded rationale** — it enters the agent's context
  on every voice turn; runtime tokens are the budget. Rationale lives here instead.
- **Repair rules carry worked examples** ("pie test" → `pytest`) — the one genuinely
  unusual skill is mapping phonetic shadows to session identifiers; examples do real
  work there. Generalized wording: transcription garbles technical speech for
  everyone; accent only changes the frequency (the kit is not speaker-specific).
- **"Just act", no confirmation echo** — under auto-send an echo gates nothing (the
  agent acts right after it), and the user watches the live stream where the agent's
  opening narration already shows what it understood. Echoing every routine command
  is burden. The agent notes its reading only when the reconstruction diverged
  materially, and asks only when the transcript can't be trusted.
- **De-prescribed on purpose** — no enumerated ask-conditions, no tone rules
  ("a remark, not a question" was cut). The guide targets frontier models; it states
  what the model cannot infer (policy: no confirmation step; fact: the user watches
  live) and trusts judgment for the rest. Over-prescription reduces output quality.
- **Last line ("framing, never the task")** — prompt-injection hygiene: the guide and
  the reference sentence must not themselves become the task.

## Maintenance rules

- Keep the guide runtime-only and short; anything explanatory moves here.
- The filename and the prompt reference are a **published contract** with sources
  (agent-term holds them as constants) — renaming either is a coordinated change
  across repos.
- `.maintainer/` exists so the repo root is exactly the vendored runtime surface;
  never add maintainer docs to the root.
