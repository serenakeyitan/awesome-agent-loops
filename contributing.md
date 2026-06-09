# Contributing

Thanks for helping grow this list. The goal is a catalog of **agent loops you can actually set up in Claude Code or OpenAI Codex** — not a list of ideas or theory.

## What belongs here

An entry must be a loop a reader can copy and run. That means one of:

- A **built-in** command or flag (`/loop`, `/goal`, `codex exec`, `claude -p`, a `Stop` hook).
- A **harness or orchestrator** repo that runs an agent in a loop.
- A **concrete workflow** someone actually ran, with the setup shown and a source linked.

If you can't show how to set it up, it doesn't go in.

## What doesn't

- Vaporware, "coming soon," or paywalled-only tools with no runnable path.
- Generic "AI agent" projects that don't loop.
- Pure essays or hot takes with no loop you can run.
- Archived or unmaintained repos.

## Entry format

One bullet, link first, description ends with a period:

```
- [name](https://link) - What it does and how to set it up, in one or two sentences. Source/credit if it came from a post.
```

- Start with the link. Use `https`, no trailing slash.
- Separator between link and description is space-hyphen-space (` - `).
- Show the actual command or config when it's short enough to inline.
- If the loop came from someone's post, credit them (`Shared by [@handle](https://x.com/handle).`).
- Put the entry in the section that matches its **mechanism** (built-in / hook / headless / orchestrator / in-the-wild).

## Before you open a PR

- Check that every URL you added resolves (no 404s).
- Confirm the setup syntax is correct against the tool's official docs — wrong commands are worse than no entry.
- Run `npx awesome-lint` if you can; fix what it flags.
- One entry per PR keeps review easy.

## How to submit

1. Fork the repo.
2. Add your entry to the right section of `README.md`.
3. Open a pull request describing the loop and linking your source.
