# Awesome Agent Loops [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)

Claude Code Loops ⚡️
> The best **`/loop`, `/goal`, and `/schedule`** prompts — copy-paste commands from X power users who stopped prompting one step at a time and started running loops.

Three built-in ways to make a coding agent keep working on its own: **`/loop`** re-runs a prompt on an interval, **`/goal`** runs until a condition is true, **`/schedule`** runs in the cloud on a cron. No plugins, no harnesses. (`/goal` is also in Codex, but only tracks a target.)

Copy any prompt, swap in your repo / PR / condition, run it.

## Table of Contents

**`/loop` — repeat on an interval**
- [Kill flaky tests](#kill-flaky-tests) · until 5 green runs in a row
- [Watch tests](#watch-tests) · re-run every 15m, report failures
- [Babysit a PR](#babysit-a-pr) · re-run `/review-pr` on a timer
- [Babysit many PRs](#babysit-many-prs) · keep labeled PRs green & rebased
- [Wait for CI, ping when green](#wait-for-ci) · poll a PR's checks

**`/goal` — run until a condition is true**
- [Fix tests + lint](#fix-tests--lint) · the "$200/hr QA engineer"
- [Hit acceptance criteria](#hit-acceptance-criteria) · endpoint behaves to spec
- [Migrate an API](#migrate-an-api) · move every call site, keep green
- [Ship a PR until green](#ship-a-pr-until-green) · implement → PR → fix CI
- [Reach coverage target](#reach-coverage-target) · add tests until 80%
- [Refactor with subagents](#refactor-with-subagents) · fan out across a module
- [Flash firmware](#flash-firmware) · loop until the hardware behaves

**`/schedule` — cron in the cloud**
- [Morning issue triage](#morning-issue-triage) · label + summarize daily
- [Keep docs in sync](#keep-docs-in-sync) · on every push to main

---

## A. `/loop` — repeat on an interval

`/loop` re-runs a prompt (or another slash command) on a time interval, then sleeps between runs. Press `Esc` to stop. ([docs](https://code.claude.com/docs/en/scheduled-tasks))

### Kill flaky tests

```
/loop run my test suite 20 times, collect every
intermittent failure, fix or quarantine the flaky
ones, and don't stop until you get 5 consecutive
fully-green runs
```

No interval — Claude self-paces and ends the loop itself once 5 green runs in a row is provably met. [source](https://x.com/ericzakariasson/status/2064122350866682100)

### Watch tests

```
/loop 15m run the test suite, and if anything fails,
show me the failing tests and the error output
```

The shape is `/loop <interval> <prompt>`. Stops when you close the terminal — use `/schedule` for runs that survive. [source](https://x.com/cnemalek/status/2062977991328583923)

### Babysit a PR

```
/loop 20m /review-pr 1234
```

The prompt can be another slash command. This is how Boris Cherny works now — he writes loops that prompt Claude, instead of prompting himself. [source](https://x.com/0xAndros/status/2064063929517777147)

### Babysit many PRs

```
/loop 15m check every open PR labeled `codex-watch`
and keep each healthy: fix CI failures, rebase when
behind main, and nudge if a review is pending
```

A 15-minute watchdog over a labeled set of PRs. Popularized as the "PR Babysitter" loop (boris-cherny) in the [loops!](https://loops.elorm.xyz/loops) catalog. [source](https://loops.elorm.xyz/loops)

### Wait for CI

```
/loop 10m run `gh pr checks 1234`; if all pass, tell
me it's ready to merge; if any fail, summarize which
and why
```

Use `/loop` when you want to *watch* something on a clock, not drive an agent to a finish (that's `/goal`). [source](https://x.com/louiswharmby/status/2063962089819869227)

---

## B. `/goal` — run until a condition is true

`/goal` sets a completion condition; after each turn a fast model checks it, and if it's not met Claude keeps going. `/goal <condition>` to set, `/goal` to check, `/goal clear` to stop. ([docs](https://code.claude.com/docs/en/goal))

### Fix tests + lint

```
/goal all tests pass and lint is clean
```

The most-shared `/goal` on X. One sentence, and the agent fixes bugs, runs checks, and ships clean — "literally a $200/hr QA engineer." [source](https://x.com/RodmanAi/status/2054975035639812170)

### Hit acceptance criteria

```
/goal the /users endpoint returns 200 with a
paginated JSON body, all tests pass, and lint is
clean — stop after 20 turns
```

> "write /goals like acceptance criteria… you set a completion condition, the agent works until a fast evaluator confirms it's met."

[source](https://x.com/akshay_pachaar/status/2055208848609460525)

### Migrate an API

```
/goal every file importing from `./legacy-api` now
imports from `./v2-api`, all tests pass, and
`npm run typecheck` is clean — stop after 30 turns
```

> "A SINGLE CODEX `/goal` RUN IS THE CLEAR WINNER. NO ORCHESTRATION… it completely destroyed the Opus orchestration setup."

Try one `/goal` with a clear finish line before reaching for a multi-agent harness. [source](https://x.com/KingBootoshi/status/2060068980728184842)

### Ship a PR until green

```
/goal a PR is open for this change and every CI check
passes — implement it, test locally, push, open the
PR with `gh pr create`, then keep fixing failures
(re-check with `gh pr checks`) until green; stop
after 10 turns
```

The most-copied loop in the [loops!](https://loops.elorm.xyz/loops) catalog — drives the whole implement → push → PR → fix-CI cycle. [source](https://loops.elorm.xyz/loops)

### Reach coverage target

```
/goal test coverage is at least 80% with all tests
passing — add focused tests for the least-covered
files, re-run coverage each turn, stop at the
threshold or after 12 turns
```

A bounded `/goal` with a numeric end state. Swap in your own threshold. [source](https://loops.elorm.xyz/loops)

### Refactor with subagents

```
/goal refactor the dialogue system to support
branching + emotion tags
```

> "Set `/goal`… Claude spawned 3 subagents: one for parser, one for UI, one for tests. All three finished."

`/goal` fanning out across a module. [source](https://x.com/hui1231123/status/2064147531194642525)

### Flash firmware

```
/goal the onboard LED blinks in the specified pattern
```

> "I use `/goal` to define what the firmware should achieve… Claude writes the code and flashes it to the board via a J-Link debugger."

Not just for web — anything with a verifiable end state works. [source](https://x.com/EitanRevach/status/2064135652288217093)

---

## C. `/schedule` — cron in the cloud

`/schedule` saves a Routine — a prompt + repo + connectors that runs on Anthropic's cloud on a schedule (min 1 hour), so it keeps running with your computer off. ([docs](https://code.claude.com/docs/en/routines))

### Morning issue triage

```
/schedule every weekday at 9am, label new issues from
the last 24h by area and priority, and post a
one-line summary on each
```

> "Configure once — prompt + repo + connectors — and it runs 7×24 in the cloud, even with your computer off."

[source](https://x.com/chenchengpro/status/2044212236881932637)

### Keep docs in sync

```
/schedule on every push to main, check whether the
changed code drifted from the docs in /docs, and open
a PR fixing anything out of date
```

> "Trigger agents on a schedule, from a GitHub event, or via API. Anthropic uses this internally for docs and backlog maintenance."

[source](https://x.com/starmexxx/status/2044653921629606204)

---

## More loops

- [loops!](https://loops.elorm.xyz/loops) by [elorm](https://elorm.xyz) — a searchable catalog of 40+ ready-to-copy loops for Claude Code, Cursor, and Codex.

## Contributing

Got a great `/loop`, `/goal`, or `/schedule` use from X? Add it — one of these three commands, the actual command in a code block, the tweet as `[source](url)`. See [contributing.md](contributing.md).

[![CC BY 4.0](https://licensebuttons.net/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/) Licensed under [CC BY 4.0](license).
