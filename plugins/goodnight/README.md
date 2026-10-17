# goodnight

A skill, invoked only by `/goodnight [per-spend $] [night total $]`. Hands the
session over for the night: nobody will answer a question until morning.

## Why

Two failures bracket an unattended night: waking to work that has to be redone,
and waking to nothing done. The skill splits every open item three ways — do it,
do it on a cheap-to-undo default kept on its own branch, or park it with the
prep done so the morning decision is one answer — and works them in that order.

Small spends are pre-approved within a per-spend and a per-night limit (default
$1 and $10), through accounts already set up, with the running total read from
the log rather than memory. Installing a tool to test it is pre-approved only inside a container, and only
when the tool is auditable, permissively licensed (OSI-approved), maintained in
the last six months, free of open advisories, and gone once the container and
image are deleted. Host installs and adoption still wait for morning.

Every action lands in an append-only log under
`~/.claude/goodnight-logs/`, which survives context condensing and crashes. On
macOS `chflags uappnd` has the filesystem refuse anything but appends; elsewhere
append-only holds by rule. To delete a log on macOS: `chflags nouappnd <file>`
first.

## Provenance

Written 2026-10-16 as a personal skill at
`~/.claude-profiles/primary/skills/goodnight/`, then brought here byte-identical.
