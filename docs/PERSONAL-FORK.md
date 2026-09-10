# Personal fork

This repository is the owner's working fork of
[ericlitman/open-pstack](https://github.com/ericlitman/open-pstack).
The initial baseline is Open Pstack 1.4.0, commit
`c8481f702c11184c42dfb4efec6e7f9e6de9caac`.

The previous Dan-Stack package is retired. Its repository is archived for history.
The existing local workspace can keep its `Dan-Stack` directory name; its active
origin is now `https://github.com/dan815/open-pstack.git`.

## Work on improvements

- `origin` points to this fork, `dan815/open-pstack`.
- `upstream` points to `ericlitman/open-pstack`.
- Keep personal issues and PRs in this fork. Propose generally useful changes
  to the upstream project separately.
- Edit the existing `plugins/pstack/skills/` tree. Keep Claude/Codex tool mapping
  in `poteto-mode/references/codex-tools.md` and provider routing in
  `poteto-mode/references/provider-dispatch.md`.
- Follow AGENTS.md and UPSTREAM.md for checks and live verification before merge.

To inspect updates from Open Pstack:

```sh
git fetch upstream main
git log --oneline main..upstream/main
git diff --stat main...upstream/main
```

Integrate reviewed updates on a focused branch. The Cursor content synchronization
described in UPSTREAM.md is a separate layer; Open Pstack remains this fork's
immediate upstream.

## Install this fork in Codex

```sh
codex plugin marketplace add dan815/open-pstack --ref main
codex plugin add pstack@open-pstack
```

The marketplace and plugin retain their upstream names. Register one Open Pstack
marketplace at a time. Start a new Codex task after installation.

After merging an improvement into this fork, refresh the Git marketplace and
reinstall. Keep all version fields synchronized as required by the static checks.

```sh
codex plugin marketplace upgrade open-pstack
codex plugin add pstack@open-pstack
```

## Owner-selected model setup

The owner selected Astra/OpenAI only. The local Codex model sheet at
`~/.codex/pstack-models.md` maps every documented role to `inherit-parent` and is
mirrored verbatim inside a bounded `pstack:models` block in `~/.codex/AGENTS.md`.
`features.multi_agent = true` enables native parallel work.

The normal parent is `gpt-6-astra`, OpenAI provider, medium reasoning. No external
Claude, Grok or OpenRouter runner is enabled by this setup. Panels remain
independent same-model reviews and must not be reported as cross-provider review.

This is an explicit owner override of the upstream four-family setup procedure,
not a completed four-provider setup. If that preference changes, run
`pstack:setup-pstack` and perform its provider-specific probes and mixed-panel
smoke before enabling those routes. Personal authentication and model sheets are
not committed to this public fork.

## Local checks

The upstream CI uses Bun 1.4.0. Install dependencies in
`plugins/pstack/skills/poteto-mode/scripts` with `bun install --frozen-lockfile`,
then run the test and typecheck commands described by upstream CI. Run static
invariants with `PSTACK_STATIC_ONLY=1 bash tests/skill-collision-repro.sh` from the
repository root. Preserve any Windows-specific failures as evidence until fixed.

On the initial Windows setup, preserve LF checkout bytes (`core.autocrlf=false`)
and include Git for Windows' `usr/bin` tools on the test process PATH. Even with
those prerequisites, the unmodified upstream suite reported 133 passing tests,
25 failures and 3 errors. Failures include symlink privilege and process/signal
handling. Strict typecheck, static invariants and Claude manifest validation
passed. These Windows failures remain follow-up work; do not claim a green suite.

The bundled Codex CLI 0.140.0 was too old for Astra. A standalone CLI 0.154.0 is
installed at `~/.local/codex-cli` through the official `@openai/codex` npm package.
A short installation path avoids the Windows helper launch failure seen under
the deeply nested WinGet npm prefix. The elevated sandbox path also failed on
this machine; the read-only smoke uses the supported per-invocation
`windows.sandbox="unelevated"` setting. Global sandbox settings remain unchanged.

The final CLI smoke passed on Astra. A fresh Codex session discovered the sole
`pstack@open-pstack` installation, read the installed `how` skill, explained a
small Python fixture correctly, and confirmed that the role inherits the parent.
No external provider or delegated task was used in that smoke. Restart the app
or open a new task to refresh plugin discovery in an existing desktop session.
