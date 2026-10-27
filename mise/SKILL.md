---
name: mise
description: >
  This machine manages ALL dev tool versions (node, python, go, java, maven,
  and CLIs like pnpm) with mise. Use for any task involving installing or
  updating tools/runtimes, pinning versions, "command not found" for language
  tools, or setting up a project environment. Never use brew/nvm/pyenv/sdkman
  or standalone installers for these.
license: MIT
---

# mise is the only tool manager

Every runtime — node, python, go, java, maven, plus global CLIs — is installed
and versioned by mise. Never install or manage these with brew, nvm, pyenv,
sdkman, `npm install -g`, or official installers, and never tell the user to.
If `mise --version` fails, mise is not installed: stop and tell the user to
install it from https://mise.jdx.dev — never substitute brew/nvm/pyenv.

## Rules

1. Tool missing or wrong version → mise, nothing else:
   - Project: `mise use node@22` (writes `mise.toml` in repo root + installs)
   - Global default: `mise use -g node@22`
   - After hand-editing `mise.toml`: run `mise install`
2. `command not found` for a language tool → check `mise ls` first. Shims
   (`~/.local/share/mise/shims`) are already on PATH, so a missing binary
   usually means it's not in mise yet — add it there. One-off run:
   `mise x node@20 -- node -v`.
3. Version pins live in `mise.toml` only. Don't create `.nvmrc`,
   `.node-version`, or `.python-version`.
4. Global CLIs (pnpm, etc.) → `mise use -g npm:pnpm` (other backends work
   too: `aqua:`, `cargo:`, `pipx:`, `ubi:`). No standalone installs.
5. mise owns `JAVA_HOME`, `GOROOT`, `GOBIN` etc. Never hardcode those paths.

## Quick reference

```bash
mise ls                # what's installed
mise current           # active versions in cwd
mise use node@22       # pin for this project
mise use -g node@22    # global default
mise install           # install everything in mise.toml
mise x node@20 -- node -v   # run with a one-off version
mise which node        # resolve which binary would run
```

Update mise the way it was installed: `brew upgrade mise` for Homebrew
installs, `mise self-update` otherwise.
