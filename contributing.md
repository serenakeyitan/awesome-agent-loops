# Contributing

Thanks for helping grow this list. It collects **real `/loop`, `/goal`, and `/schedule` uses** people posted on X — nothing else.

## What belongs here

- One of the three built-in commands: **`/loop`**, **`/goal`**, or **`/schedule`** (Claude Code, and Codex where it applies).
- A **real use** someone actually posted — not a hypothetical.
- The **actual command** in a code block.
- The **tweet** it came from, linked as `[source](url)`.

## What doesn't

- Hooks, headless `while` loops, orchestrators, SDKs, plugins — this list is only the three built-in commands.
- "You could do X" with no command and no source.
- Dead or unverifiable tweet links.

## Entry format

Match the existing entries exactly:

```markdown
### Short title for the use

​```
/goal the actual command someone ran
​```

> Optional pull-quote from the tweet.

A line on what it does / when to reach for it.

[source](https://x.com/handle/status/1234567890)
```

- Put it under the right section: A (`/loop`), B (`/goal`), or C (`/schedule`).
- Add a Table of Contents link for your entry.
- Confirm the `[source](url)` link actually resolves before submitting.

## How to submit

1. Fork the repo.
2. Add your entry to the right section of `README.md` and a TOC link.
3. Open a pull request with the source tweet in the description.
