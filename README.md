# Awesome Agent Loops [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of agent loops you can set up in **Claude Code** and **OpenAI Codex** — the slash commands, hooks, and harnesses people are actually running instead of typing one prompt at a time.

A *loop* is an agent that keeps going on its own: it works, checks itself, and runs again until a condition is met — on an interval, until tests pass, until a PR is green. Below is a catalog of loops you can copy and run today. Each entry says what it does, how to set it up, and where it came from.

## Contents

- [Built-in loops](#built-in-loops)
- [Hook-driven loops](#hook-driven-loops)
- [Headless & scripted loops](#headless--scripted-loops)
- [Orchestrator & multi-agent loops](#orchestrator--multi-agent-loops)
- [Loops in the wild](#loops-in-the-wild)

## Built-in loops

Loops that ship inside the tool — nothing to install.

- [`/loop` (Claude Code)](https://code.claude.com/docs/en/scheduled-tasks) - Re-runs a prompt or another slash command on an interval until you stop it. `/loop 5m check the deploy` runs every 5 minutes; `/loop 20m /review-pr 1234` runs another command on a schedule; `/loop fix the failing tests` with no interval lets Claude self-pace and stop itself when the work is provably done. Press `Esc` to stop; recurring loops auto-expire after 7 days. Needs Claude Code v2.1.72+.
- [`/goal` (Claude Code)](https://code.claude.com/docs/en/goal) - Keeps working turn after turn until a condition is true. `/goal all tests pass and lint is clean or stop after 20 turns` — after each turn a fast model checks the condition and, if not met, feeds the reason back and runs again. `/goal` shows status, `/goal clear` stops it. One goal per session. Needs v2.1.139+.
- [`/goal` (Codex)](https://developers.openai.com/codex/cli/slash-commands) - Pins a persistent objective to the active thread so Codex keeps it in view across a long task. Note: unlike Claude's `/goal`, this **tracks** a target rather than looping turn-after-turn on its own — `/goal <objective>` to set, `/goal pause|resume|clear` to manage.
- [`codex exec` (Codex)](https://developers.openai.com/codex/cli/) - The non-interactive entry point you wrap in your own loop (`codex e` for short). `codex exec --json -o out.txt "<prompt>"` runs once, streams JSONL events, and writes the final message to a file — the building block for every scripted Codex loop below. Use `codex exec resume --last` to carry state between iterations.
- [`/schedule` (Claude Code)](https://code.claude.com/docs/en/routines) - Cloud-managed cron for durable, recurring agent runs (Routines) that outlive your session, minimum interval one hour. Reach for this instead of `/loop` when the loop should keep running after you close the terminal.

## Hook-driven loops

Loops built from Claude Code's `Stop` hook — the hook fires when Claude finishes, and a `"decision": "block"` response feeds a reason back in, so Claude never stops until your condition is met.

- [Stop-hook continuation loop](https://code.claude.com/docs/en/hooks) - The DIY version of `/goal`. Add a `Stop` hook in `settings.json` that inspects state (tests, lint, a checklist) and returns `{"decision": "block", "reason": "..."}` to keep going, or allows the stop when done. This is the primitive `/goal` wraps; use it when you need custom continuation logic.

```json
{
  "hooks": {
    "Stop": [
      { "matcher": "*", "hooks": [ { "type": "command", "command": "./scripts/loop-gate.sh" } ] }
    ]
  }
}
```

- [Codex autofix-on-PR loop](https://x.com/kr0der/status/2063003932105093159) - A Codex [skill](https://developers.openai.com/codex/skills) (`name: autofix`) that fixes review comments on an open PR, fired on an interval by an external driver (cron or a `codex exec` loop — Codex skills add capability but don't schedule themselves) so it keeps checking until the thread is clean. Shared by [@kr0der](https://x.com/kr0der).

## Headless & scripted loops

The classic shape: run the agent in a shell loop, let it see its own output each pass.

- [`claude -p` while-loop](https://code.claude.com/docs/en/headless) - Run Claude Code headless inside a bash loop, shown below in its minimal one-line form. `--output-format stream-json` gives parseable output, `--max-turns N` caps each pass, and `-c` continues the prior session across iterations.

```bash
while :; do claude -p "work on TODO.md until every box is checked"; done
```

- [`codex exec` while-loop](https://developers.openai.com/codex/cli/reference) - Same pattern for Codex. `codex exec --json` streams events; `--output-schema` constrains the final answer to JSON so a script can decide whether to loop again; `--skip-git-repo-check` and a sandbox flag keep it unattended.
- [continuous-claude](https://github.com/AnandChowdhary/continuous-claude) - A ready-made "loop with PRs": runs Claude Code in a continuous loop that opens a branch, creates a PR, waits for CI with `gh pr checks`, and merges on success or discards on failure. Drop-in if you want the loop to ship code, not just edit it.

## Orchestrator & multi-agent loops

When one looping agent isn't enough — these fan out, coordinate, and loop a whole fleet.

- [ralph-orchestrator](https://github.com/mikeyobrien/ralph-orchestrator) - Runs an agent in a loop with "backpressure" gates (tests, lint, typecheck) and a multi-persona system, iterating until it emits a completion signal or hits an iteration cap. Backends include Claude Code, Codex, and Gemini CLI.
- [claude-flow](https://github.com/ruvnet/ruflo) - Multi-agent harness for Claude Code that spawns coordinated swarms with shared memory and an autopilot that keeps agents working in a loop. Installed as the `claude-flow` npm package / CLI.
- [Claude Agent SDK](https://github.com/anthropics/claude-agent-sdk-python) - Build your own loop in code: the SDK gives you the same agent loop, tools, and context management that power Claude Code, in [Python](https://github.com/anthropics/claude-agent-sdk-python) and [TypeScript](https://github.com/anthropics/claude-agent-sdk-typescript). Use it when a slash command isn't programmable enough.

## Loops in the wild

Real loops people posted, with the workflow they actually ran — useful as templates.

- [Three-subagent `/goal` refactor](https://x.com/hui1231123/status/2064147531194642525) - Sets `/goal` on a dialogue-system refactor and Claude spawns three subagents (each scoped to a slice), looping until the goal check passes. A clean example of `/goal` + fan-out. Shared by [@hui1231123](https://x.com/hui1231123).
- ["Turn the workflow into a skill + `/loop`"](https://x.com/AlejandroDnsmr/status/2063817679988007009) - The operating principle behind most of this list: *"use `/goal` and `/loop` instead of sending one prompt at a time; whenever you find yourself doing a similar workflow, turn it into a skill + `/loop`."* Works the same across Claude and Codex. From [@AlejandroDnsmr](https://x.com/AlejandroDnsmr).
- [Cross-agent paid worker loop](https://x.com/tetsuoarena/status/2064017913372139598) - An agent that runs 24/7 and does paid work on its own, the same whether it's driven by Claude or Codex — pushing the loop all the way to autonomous operation. From [@tetsuoarena](https://x.com/tetsuoarena).

## Contributing

Contributions welcome — see [contributing.md](contributing.md). The bar: it must be a **loop you can actually set up in Claude Code or Codex**, with the setup shown and a source linked. No vaporware.

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/) This work is dedicated to the public domain under [CC0](license).
