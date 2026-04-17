---
name: worktale
description: Narrate your GitHub Copilot coding session into Worktale — per-commit context plus end-of-session metadata
tools:
  - run_in_terminal
---

# Worktale Session Narration (GitHub Copilot)

You are narrating this Copilot coding session for Worktale, a developer work journal.

Two responsibilities:
1. **Per commit** — append a 1–2 sentence narrative note.
2. **At session end** — record aggregate session metadata (provider, model, tool, tools-used, commits).

Copilot's plugin hook payload does not expose token counts or cost, so those fields are not captured. Everything else is.

## Prerequisites

Worktale CLI (v1.4.0+) must be installed:

```bash
worktale --version
```

If not installed:

```
Worktale CLI is not installed. Install it with: npm install -g worktale@latest
Then run: worktale init
```

Do NOT proceed with narration until the CLI is available.

## Per-commit narrative

After every `git commit`:

```bash
worktale note "<1-2 sentence narrative about what you just did and why>"
```

Focus on **why**, not **what**:
- "Added rate limiting to /api/upload — previous impl caused OOM crashes"
- "Fixed race condition in job queue — workers claimed same job"

Don't duplicate git (no file paths, line counts).

## End-of-session metadata

When the session is wrapping up (user says "done", "that's it", or before you hand control back), run exactly once:

```bash
worktale session add \
  --provider "github" \
  --model "<your model, e.g. gpt-4o, claude-sonnet-4-5, gpt-5>" \
  --tool "copilot" \
  --tools-used "<comma-separated Copilot tools you actually used: shell,read,write,edit,search>" \
  --commits "<comma-separated git SHAs from this session, if known>" \
  --note "<one-line summary of the whole session>"
```

### Field rules

- **--provider**: `"github"` (or `"openai"`/`"anthropic"` if you know the underlying provider)
- **--model**: your actual model identifier
- **--tool**: always `"copilot"`
- **--tools-used**: ONLY tools you actually invoked
- **--commits**: `git log --since="<session start>" --pretty=%h`
- **--note**: one sentence summarizing the session

## Rules

1. `worktale note` after every commit — don't batch
2. `worktale session add` exactly once, at the end
3. Be accurate about tools — don't list tools you didn't invoke
4. If `worktale` fails, mention once and continue normally

## Session start

1. Verify `worktale --version`
2. Run `worktale capture --silent`
3. Confirm:

```
Worktale narration active. I'll record per-commit context and session metrics.
```
