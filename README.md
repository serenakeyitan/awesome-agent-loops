# Awesome Agent Loops [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)

Claude Code Loops ⚡️
> A curated collection of the best **`/loop`, `/goal`, and `/schedule`** prompts — copy-paste commands sourced from Twitter/X power users who stopped prompting one step at a time and started running loops.

These are the three built-in ways to make a coding agent keep working on its own: **`/loop`** re-runs a prompt on an interval, **`/goal`** runs until a condition is true, and **`/schedule`** runs in the cloud on a cron. No plugins, no harnesses — just commands you paste into Claude Code today. (`/goal` also exists in Codex, though it tracks a target rather than looping on its own.)

Every prompt below is copy-paste-ready and sourced to the tweet it came from. Swap in your own repo, PR number, or condition and run it. If you want your agent to have a heartbeat, **this is the repo!**

## Table of Contents

### A. `/loop` — run a prompt on an interval
- [Watch a deploy and report when it changes](#watch-a-deploy-and-report-when-it-changes)
- [Re-check flaky tests on an interval](#re-check-flaky-tests-on-an-interval)
- [Babysit a PR on a schedule](#babysit-a-pr-on-a-schedule)
- [Poll an open PR's CI and ping when green](#poll-an-open-prs-ci-and-ping-when-green)

### B. `/goal` — run until a condition is true
- [The $200/hr QA engineer](#the-200hr-qa-engineer)
- [Write goals like acceptance criteria](#write-goals-like-acceptance-criteria)
- [Migrate a codebase, end to end](#migrate-a-codebase-end-to-end)
- [`/goal` + subagents](#goal--subagents)
- [`/goal` for firmware / hardware](#goal-for-firmware--hardware)

### C. `/schedule` — run in the cloud on a cron
- [Triage new issues every morning](#triage-new-issues-every-morning)
- [Keep docs in sync on every push to main](#keep-docs-in-sync-on-every-push-to-main)

---

## A. `/loop` — run a prompt on an interval

`/loop` re-runs a prompt (or another slash command) on a time interval, then goes dormant between runs. Press `Esc` to stop. Good for polling and watching. ([docs](https://code.claude.com/docs/en/scheduled-tasks))

### Watch a deploy and report when it changes

```
/loop 5m check the status of the latest deployment with `gh run list --limit 1`, and if it changed since last check, summarize what changed and whether it succeeded
```

> "Polling a deploy. Babysitting PRs. Re-checking flaky tests. Set it once, walk away. Your AI agent finally has a heartbeat."

*source: https://x.com/abfayy/status/2064131017225752786*

### Re-check flaky tests on an interval

```
/loop 15m run the test suite, and if anything fails, show me the failing tests and the error output
```

The shape is `/loop <interval> <prompt>` — Claude schedules it and runs automatically. Note: it stops when you close the terminal (use `/schedule` for runs that survive).

*source: https://x.com/cnemalek/status/2062977991328583923*

### Babysit a PR on a schedule

```
/loop 20m /review-pr 1234
```

The prompt can be another slash command. This is how Boris Cherny works now — he doesn't prompt anymore, he writes loops that prompt Claude.

*source: https://x.com/0xAndros/status/2064063929517777147*

### Poll an open PR's CI and ping when green

```
/loop 10m run `gh pr checks 1234`; if all checks pass, tell me it's ready to merge; if any fail, summarize which ones and why
```

`/loop` is the right tool when you want to *watch* something on a clock rather than drive an agent to completion (that's `/goal`).

*source: https://x.com/louiswharmby/status/2063962089819869227*

---

## B. `/goal` — run until a condition is true

`/goal` sets a completion condition; after each turn a fast evaluator checks it and, if not met, Claude keeps going. Set it with `/goal <condition>`, view with `/goal`, stop with `/goal clear`. ([docs](https://code.claude.com/docs/en/goal))

### The $200/hr QA engineer

```
/goal all tests pass and lint is clean
```

The single most-shared `/goal` invocation on X. One sentence and the agent fixes bugs, runs checks, and ships clean — "literally a $200/hr QA engineer."

*source: https://x.com/RodmanAi/status/2054975035639812170*

### Write goals like acceptance criteria

```
/goal the /users endpoint returns 200 with a paginated JSON body, all tests pass, and lint is clean — stop after 20 turns
```

> "write /goals like acceptance criteria. `/goal` is now everywhere — Claude Code, Codex, Hermes — you set a completion condition, the agent works autonomously until a fast evaluator model confirms it's met."

*source: https://x.com/akshay_pachaar/status/2055208848609460525*

### Migrate a codebase, end to end

```
/goal every file importing from `./legacy-api` is migrated to `./v2-api`, all tests pass, and `npm run typecheck` is clean — stop after 30 turns
```

> "A SINGLE CODEX `/goal` RUN IS THE CLEAR WINNER. NO ORCHESTRATION, NO OUROBOROS, JUST ONE LITTLE AGENT THAT COULD 🤯 — it completely destroyed the Opus orchestration setup."

Before reaching for a multi-agent harness, try one `/goal` with a clear finish line.

*source: https://x.com/KingBootoshi/status/2060068980728184842*

### `/goal` + subagents

```
/goal refactor the dialogue system to support branching + emotion tags
```

> "Set `/goal`… Claude spawned 3 subagents: one for parser, one for UI, one for tests. All three finished." A clean example of `/goal` fanning out across a codebase.

*source: https://x.com/hui1231123/status/2064147531194642525*

### `/goal` for firmware / hardware

```
/goal the onboard LED blinks in the specified pattern
```

> "I use the `/goal` skill to define exactly what the firmware should achieve… Claude writes the code and flashes it directly to the board via a J-Link debugger." `/goal` isn't just for web — anything with a verifiable end state works.

*source: https://x.com/EitanRevach/status/2064135652288217093*

---

## C. `/schedule` — run in the cloud on a cron

`/schedule` saves a Routine — a prompt + repo + connectors that runs on Anthropic's cloud on a schedule (min interval 1 hour), so it keeps running with your computer off. ([docs](https://code.claude.com/docs/en/routines))

### Triage new issues every morning

```
/schedule every weekday at 9am, label any new issues opened in the last 24h by area and priority, and post a one-line summary of each as a comment
```

> "Configure once — prompt + repo + connectors — and it runs 7×24 in Anthropic's cloud. Runs even with your computer off. Cron down to a 1-hour minimum interval, plus a dedicated API endpoint per routine."

*source: https://x.com/chenchengpro/status/2044212236881932637*

### Keep docs in sync on every push to main

```
/schedule on every push to main, check whether the changed code drifted from the docs in /docs, and open a PR fixing any docs that are now out of date
```

> "Claude Code Routines are here — trigger agents on a schedule, from a GitHub event, or via API. Anthropic uses this internally for docs and backlog maintenance and it changed how they work."

*source: https://x.com/starmexxx/status/2044653921629606204*

---

## Contributing

Found a great `/loop`, `/goal`, or `/schedule` use on X? Add it. The rule: it must be **one of these three built-in commands**, with the actual command in a code block and the tweet linked as `*source:*`. See [contributing.md](contributing.md).

[![CC BY 4.0](https://licensebuttons.net/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/) Licensed under [CC BY 4.0](license).
