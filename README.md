# Worktale Plugin for GitHub Copilot CLI

**AI session narration for your daily work journal.**

Git captures the *what*. This plugin captures the *why*.

When you activate the Worktale agent in [GitHub Copilot CLI](https://docs.github.com/en/copilot/copilot-cli), your AI coding agent automatically narrates each commit — adding intent, decisions, and context to your [Worktale](https://worktale.org) daily narrative.

## Install

```
/plugin install worktale/worktale-copilot-plugin
```

Requires the [Worktale CLI](https://www.npmjs.com/package/worktale) **v1.1.0+**:

```bash
npm install -g worktale@latest
cd your-repo
worktale init
```

## Usage

In any Copilot CLI session, activate the Worktale agent:

```
@worktale
```

The agent confirms narration is active, then after every commit it makes, it runs:

```bash
worktale note "Refactored auth middleware for compliance — replaced session token storage"
```

### Automatic capture via hooks

This plugin also includes a `postToolUse` hook that automatically runs `worktale capture` after every `git commit` the agent makes — even without explicitly activating the `@worktale` agent. This ensures every commit is captured in your local Worktale database.

Notes accumulate throughout the day. View them with:

```bash
worktale digest    # End-of-day summary with your notes
worktale dash      # Interactive TUI dashboard
```

## What gets captured

The agent writes 1-2 sentence notes focused on:

- **Intent** — why the change was made
- **Decisions** — trade-offs and alternatives considered
- **Problems solved** — bugs found, root causes identified

It does *not* duplicate what git already tracks (file paths, line counts, diffs).

## How it works

1. `/plugin install` registers the Worktale agent and hooks
2. `@worktale` activates the narration agent for the session
3. The `postToolUse` hook fires after every `git commit`, running `worktale capture`
4. The agent runs `worktale note "..."` after each commit with rich context
5. Notes appear in `worktale digest`, the TUI dashboard, and (eventually) your Worktale Cloud portfolio

All data stays local. Nothing leaves your machine.

## Links

- [Worktale CLI](https://github.com/worktale/worktale-cli)
- [Worktale website](https://worktale.org)
- [Documentation](https://worktale.org/docs.html)
- [Claude Code Plugin](https://github.com/worktale/worktale-plugin)
- [Codex CLI Skill](https://github.com/worktale/worktale-codex-plugin)

## License

MIT
