# repo-scaffold

One-shot per-stack scaffolding for a new repo: `CLAUDE.md`, `DEV_TOOLS.md`,
`README`, `.gitignore`, the stack's directory skeleton, `git init`, and
optionally `gh repo create`.

Two entry points, one implementation:

- `commands/scaffold-repo.md` — the `/scaffold-repo` slash command.
- `bin/scaffold-repo` — a CLI shim.

Both call `scaffold-new-repo.sh`. The per-stack content lives in `dirs/`,
`gitignore/`, `readme/` and `next-steps/`.

When `--stack` or the target directory is missing, it asks in plain text with
numbered options rather than guessing.

## Two entry points, one install

Installing the plugin registers the `/scaffold-repo` slash command *and* makes
`scaffold-repo` a bare command: `bin/` is the default executables location, which
Claude Code adds to the Bash tool's `PATH` while the plugin is installed.

Earlier revisions claimed a plugin cannot put a binary on `PATH` and prescribed a
`ln -s` into `~/.local/bin`. That was wrong; nothing needs linking.

## Paths

Both entry points resolve inside the plugin: the shim execs
`scaffold-new-repo.sh` one directory above its `bin/`, and the slash command calls
the bare `scaffold-repo`. The script finds its per-stack content alongside itself.
1.0.0 pointed both at Standard Configs' deploy layout (`~/.local/bin/`,
`~/.local/share/repo-scaffold/`), which a plugin install never creates; 1.0.1
fixed that.

The CLAUDE.md template is still read from `~/Developer/CLAUDE_TEMPLATE.md`, and
the script fails clearly when it is absent.

## Provenance

From `Standard Configs/claude-core/repo-scaffold/templates/` at `d36d794`, all
files byte-identical in 1.0.0. 1.0.1 changed only `bin/scaffold-repo` and
`commands/scaffold-repo.md` (see Paths).
