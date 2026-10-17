---
name: goodnight
description: Use when the user runs /goodnight before going to sleep, because unattended work needs its own triage, spending limits, container spikes and append-only log rather than the usual ask-when-unsure loop.
disable-model-invocation: true
argument-hint: "[per-spend limit $] [night total $]"
---

# Goodnight

The user is asleep from now until they next speak. Nobody will answer a question, approve a prompt or unblock a stall. Two outcomes count as failure: waking up to work that has to be redone, and waking up to nothing done.

## Never wait on the user

- Ask nothing. No AskUserQuestion, no "should I…?", no turn that ends waiting for a reply.
- Every standing rule in CLAUDE.md and memory still holds. Goodnight lifts none of them; anything they say needs the user's yes is parked, not done. The exceptions are the spending and the container spikes below.

## Pre-approved spending

Arguments: `$ARGUMENTS`. The first number is the most one spend may cost, the second the most all spends may cost tonight. Defaults: **$1 per spend, $10 per night**.

The user approves, in advance, money spent through an account that is already set up (a paid API call, a cloud run, a metered service) when all of these hold:

- the cost is known or capped before you spend it, and fits the per-spend limit;
- it keeps tonight's running total within the night limit;
- it serves an item you would otherwise do (bucket 1, 2, or prep for 3).

Anything else is parked: an unknown or uncapped cost, a new signup or payment detail, a subscription or anything recurring, or a spend that would break either limit. This approval covers money only; it never approves installing or adopting a library, tool or service. This session's own model usage doesn't count against the budget.

Before each spend, take the running total from the last `spend` line in the action log, not from memory.

## Pre-approved container spikes

The user approves, in advance, installing a library, package or tool inside a container to test it, when all of these hold and each is checked, not assumed:

- **auditable**: its source is public and readable;
- **open source under a permissive OSI-approved licence** (MIT, BSD, Apache-2.0, ISC or similar); copyleft or no licence parks it;
- **well maintained**: a release or commit in the last six months;
- **no open security advisories** for the version used, checked in OSV or GitHub Security Advisories;
- **uninstallable**: deleting the container and its image removes every trace.

The spike runs only inside a container from a runtime already on the machine. Mount nothing from the host beyond the scratchpad, pass in no credentials, and delete the container and image when the spike ends. Log the install as `install` with the evidence for each check.

This covers testing only. Installing on the host, adopting it into a project, or anything a standing rule bans outright still parks, with the spike's findings as its prep.

## Action log

First thing, before any work, start tonight's log and make it append-only:

```bash
mkdir -p ~/.claude/goodnight-logs
log=~/.claude/goodnight-logs/$(date +%Y-%m-%d-%H%M)-$(basename "$PWD").md
echo "# Goodnight $(date '+%Y-%m-%d %H:%M') in $PWD" >> "$log" && echo "$log"
chflags uappnd "$log" 2>/dev/null || echo "no append-only flag on this OS; append-only holds by rule"
```

Shell state doesn't carry between commands, so reuse the printed path literally.

- Append one line per action, as it happens, with `>>` only: `HH:MM <kind> <item>: <what happened>`, where kind is `start`, `do`, `default`, `park`, `spend`, `install`, `verify` or `fail`. The first line after the header is `start` with tonight's limits.
- A `spend` line carries its cost and the new total: `spend $0.03 (total $0.41): <what, why>`.
- Never rewrite or delete a line; a correction is a new line. On macOS the system refuses anything but appends, and the flag stays on.
- The log is the source of truth for the night. After context is condensed, or in a new session, read it before continuing.

## Triage every open item

Go through everything pending in this session (tasks, todo lists, review findings, follow-ups you offered) and put each item in one bucket:

1. **Do**: no decision is needed from the user. Finish it completely.
2. **Do on a default**: a decision is needed, but a sensible default exists and undoing it is cheap. Pick the default, keep the work separate (its own branch, worktree or commit) so it can be dropped without untangling, and record the assumption.
3. **Park**: a standing rule requires the user's yes, the action is irreversible or outward-facing, or a wrong guess would mean redoing real work. Don't build it. Do the prep instead: research, read the code, draft the options, so the morning decision is a single answer.

Work in that order: all of bucket 1, then bucket 2, then prep for bucket 3. When unsure between 2 and 3, the test is how much rework a wrong guess costs; if it's more than deleting a branch, park it.

## Finish things for real

- "Done" means verified: tests run and pass, or the change is shown working. Unverified work is reported as unverified.
- If an approach fails twice, park the item with what you tried and move on. Don't burn the night in a loop.
- Don't stop while bucket 1 or 2 has anything left, or while a parked item still lacks its prep.

## Red flags

| Thought | Checksum | Reality |
|---|---|---|
| "I'll ask quickly, they might still be up." | `◇awake ↛ awake` | Nobody answers tonight. Decide by the buckets. |
| "They'd obviously say yes to this install." | `◇approve ↛ approve` | Only a spike inside a container that passes every check is pre-approved. On the host or into the project, park it with the prep done. |
| "The licence is probably MIT, it's popular." | `¬obs(P) ↛ ¬P` | Read the licence file and check the advisories before installing. |
| "It's basically working, mark it done." | `felt(P) ↛ tested(P)` | Run the test, or report it as unverified. |
| "The total is about $3, there's room." | `knew(t₀) ↛ knows(t₁)` | Read the last `spend` line before spending. |
| "Each spend is tiny, the night limit can flex." | `∀i small(cᵢ) ↛ small(Σcᵢ)` | The night limit is the limit. Park the spend that would break it. |
| "Nothing left I can do, so stop." | `¬obs(P) ↛ ¬P` | Check every bucket, including prep for parked items, before stopping. |

## Morning report

End with one report, short enough to read before coffee. Append it to the action log, then show it, with the log's path:

- **Done**: each item and how it was verified.
- **Done on assumptions**: each assumption made, and where the work is so it can be dropped (branch, commit, path).
- **Parked**: for each, the one question that unblocks it and your recommended answer.
- **Failed**: what broke, what you tried, the exact error.
- **Spent**: each spend, with the total against tonight's limit.
- **Spiked**: each container install, the evidence for each check, what it showed, and confirmation the container and image are gone.
