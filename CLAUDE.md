# ai-additions

Portable Claude Code behaviour additions, shaped as a plugin marketplace.

## Structure

- `.claude-plugin/marketplace.json` — the registry. One entry per plugin
  carried here. A plugin in the repo is a plugin installed; there is no
  separate enablement step.

  ```
  R(p) := p is under plugins/ and registered in marketplace.json
  I(p) := p is installed for the user
  E    := a separate enablement step exists

  1.  ∀p: R(p) → I(p)      [a plugin in the repo is a plugin installed]
  2.  ¬E                    [there is no separate enablement step]
  ∴   adding p to the repo means installing it, with no second approval
  ```
- `plugins/<name>/` — one directory per addition, each with its own
  `.claude-plugin/plugin.json`, plus `skills/`, `hooks/`, and/or MCP source.
  Nine additions are carried here: `brainstorming`, `extras`,
  `github-utilities`, `goodnight`, `logic-rendering`, `prd-design`,
  `repo-scaffold`, `skill-perf`, and `worktree-guard`. The extracted five live in their own repos — see
  `referenced/`.

## Referenced, not absorbed

An addition that already has its own repo stays there and installs from that
repo's own marketplace (`claude plugin marketplace add locriani/<name>`, then
`claude plugin install <name>@<name>`). `referenced/` holds a
pointer with its coordinates. Vendoring a copy creates a second
source of truth that goes stale silently, which is the failure mode this
convention exists to avoid.

## Relationship to Standard Configs

`~/Developer/Standard Configs` bootstraps the machine and will consume this repo
via a thin `external/ai-additions/` stub. That stub is the *only* sanctioned
coupling: this repo must never depend on Standard Configs' `stdcfg/` library or
assume its directory layout. Plugins here run on a stranger's machine under
Claude Code alone.
