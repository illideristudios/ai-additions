# ai-additions

The home for Zach Gardner's Claude Code behaviour additions: the shared rules,
pointers to the additions that live elsewhere, and the plugins that still live
here.

This began as a single plugin marketplace. The plugins that wanted their own
identity now have it — on 2026-10-10, five of them were extracted at `a01d7eb`
into their own public repositories, each its own Claude Code marketplace. What
stays here:

- [`rules/`](rules/) — six prose rule documents, verbatim copies from Standard
  Configs.
- [`referenced/`](referenced/) — one pointer per addition that lives elsewhere.
  Never a vendored copy.
- [`plugins/`](plugins/) — the nine additions carried locally.
- [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) —
  registers those nine.

## Installing

The extracted plugins install from their own marketplaces, one `marketplace
add` per repo:

```sh
claude plugin marketplace add locriani/<name>
claude plugin install <name>@<name>
```

The nine local plugins install from this marketplace as before:

```sh
claude plugin marketplace add locriani/ai-additions
claude plugin install <name>@ai-additions
```

Updates: `claude plugin marketplace update <marketplace>`, then `claude plugin update <name>@<marketplace>`. Installing again does not upgrade a plugin that is already installed.

## The catalog

Extracted 2026-10-10 — public repos, each its own marketplace. Each plugin's
pointer lives under [`referenced/`](referenced/):

| Plugin | Repo |
|---|---|
| `branch-graph` | [`locriani/branch-graph`](https://github.com/locriani/branch-graph) |
| `bug-hunt` | [`locriani/bug-hunt`](https://github.com/locriani/bug-hunt) |
| `datetime-inject` | [`locriani/datetime-inject`](https://github.com/locriani/datetime-inject) |
| `gitlab-ci-optimizer` | [`illideristudios/ci-optimization-skill`](https://github.com/illideristudios/ci-optimization-skill) |
| `mermaid-system-design` | [`locriani/mermaid-system-design`](https://github.com/locriani/mermaid-system-design) |
| `stack-profiles` | [`locriani/stack-profiles`](https://github.com/locriani/stack-profiles) |

Still local — registered in this repo's marketplace, source under
[`plugins/`](plugins/):

| Plugin | What it is |
|---|---|
| [`brainstorming`](plugins/brainstorming/) | Finds out what the user actually wants, and why, before anything is built. SKILL.md body from obra/superpowers (MIT). |
| [`extras`](plugins/extras/) | Five skills: code-review, time-estimation, defect-density, dev-velocity, skill-perf-judge. |
| [`github-utilities`](plugins/github-utilities/) | Enforces [`rules/GITHUB.md`](rules/GITHUB.md): `gh-issue` files issues from the repo's own templates with native relationships; a PreToolUse guard and a Stop hook hold `gh` to the same rules. |
| [`goodnight`](plugins/goodnight/) | `/goodnight` before sleep: finishes what needs no input, defaults the cheap-to-undo on separate branches, parks the rest with its prep done, within a spend budget, container-only spikes of vetted tools, and an append-only action log. |
| [`logic-rendering`](plugins/logic-rendering/) | Renders logic — conditionals, implications, multi-premise reasoning — as boolean/modal notation alongside short prose. |
| [`prd-design`](plugins/prd-design/) | Interviews before writing a PRD, then decomposes into ordered vertical slices led by a tracer bullet. |
| [`repo-scaffold`](plugins/repo-scaffold/) | One-shot per-stack new-repo scaffolding. |
| [`skill-perf`](plugins/skill-perf/) | Stop-hook SQLite ledger of skill-invocation cost, plus a judge reminder. |
| [`worktree-guard`](plugins/worktree-guard/) | Stop hook blocking writes that cross into another worktree of the same repo. |

## Why this is separate from Standard Configs

Standard Configs bootstraps *this Mac* — Homebrew, launchd agents, macOS
defaults, install runbooks. Its unit of work is "make this machine match the
snapshot."

This repo's unit of work is a behaviour: a rule Claude follows, a hook that
makes it hold, a skill it invokes. Those are portable — they are the same on a
second machine, in a cloud session, or under a different account profile.
Standard Configs consumes this repo through a thin `external/ai-additions/`
stub, the same pattern it already uses for side repos it does not own.
