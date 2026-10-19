# gitlab-ci-optimizer

**Upstream:** [`illideristudios/ci-optimization-skill`](https://github.com/illideristudios/ci-optimization-skill) (public; created as `locriani/ci-optimization-skill`, which redirects).
**Local checkout:** `~/Developer/Gauntlet/Projects/ci-optimization-skill`
**Status:** added 2026-10-19 on *"get it into my ai additions, set it up, etc please"* (Zach). The plugin manifest is on branch `feat/claude-plugin` (1.0.0) and is not yet on `main`, so until it merges the plugin installs only from the local checkout with that branch checked out.

A Claude Code plugin with two skills. `gitlab-ci-optimizer` writes and reviews `.gitlab-ci.yml` and `Dockerfile.ci` to the Gauntlet GitLab Primer: worker and heap caps sized to the runner (exit 137), the prebuilt CI image, install-once artifacts with `needs:`, `workflow: rules` against duplicate MR pipelines, and sharding within runner slots. It ships `audit_pipeline.py`, an 11-point Lean Score auditor, and `simulate_budget.py`, a RAM and concurrency estimator from illustrative constants. `simulate-budget` is the `/simulate-budget` front end to the simulator. Both scripts need only Python 3. The repo also carries adapters for Antigravity, Cursor and Codex.

## Install

```sh
claude plugin marketplace add illideristudios/ci-optimization-skill   # once feat/claude-plugin is on main
claude plugin install gitlab-ci-optimizer@gitlab-ci-optimizer
```

Before that merge, `claude plugin marketplace add ~/Developer/Gauntlet/Projects/ci-optimization-skill` with `feat/claude-plugin` checked out. The installed copy is a version-stamped snapshot under `~/.claude/plugins/cache/`; after a new release, `claude plugin marketplace update gitlab-ci-optimizer`, then `claude plugin update gitlab-ci-optimizer@gitlab-ci-optimizer`.
