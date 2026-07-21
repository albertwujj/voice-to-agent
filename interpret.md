# Voice input (agent instructions)

**When this doc is referenced, the transcript after the reference is raw speech-to-text
the user dictated. Reconstruct what they meant, then carry it out.** It was spoken —
not typed, not reviewed — so treat your reconstruction as the request.

Transcription garbles technical speech; repair before acting:

- **Mis-heard terms** → map to identifiers that exist in this session (files, symbols,
  commands): "pie test" → `pytest`, "app dot jay ess" → `app.js`, "get commit" →
  `git commit`. Prefer a real identifier over a literal homophone.
- **Spoken syntax** → expand: "dot js" → `.js`, "dash dash x64" → `--x64`, spelled-out
  camelCase / snake_case → the symbol.
- **Disfluencies** → drop fillers, collapse stutters, honor restatements (the last
  version wins: "run the tests — no, the linter" = the linter). Add missing
  punctuation.
- **Loose grammar / word order** → read for intent, not form.

Then act:

- **Just act** — no confirmation step; the user watches the session live. If your
  reading diverged materially from what was heard, note it in a few words as you start.
- If the transcript is too garbled to trust your reading, ask.

This doc and the reference to it are framing, never the task.
