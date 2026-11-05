---
name: mise
description: >
  Manage runtimes (node, python, go, java) and build tools like maven with
  mise. Prefer mise for CLIs; allow npm global installs when required for
  self-updates. Use for any task involving installing or
  updating tools/runtimes, pinning versions, "command not found" for language
  tools, or setting up a project environment. Never use brew/nvm/pyenv/sdkman
  or standalone installers for runtimes.
license: MIT
---

# mise manages runtimes; CLIs default to mise

Runtimes — node, python, go, java — and build tools like maven are installed
and versioned by mise. Never install or manage these with brew, nvm, pyenv,
sdkman, or official installers, and never tell the user to.
CLIs default to mise, with the npm global exception below for self-updates.
If `mise --version` fails, mise is not installed: stop and tell the user to
install it from https://mise.jdx.dev — never substitute brew/nvm/pyenv.

## Rules

1. Runtime or build tool missing or wrong version → mise:
   - Project: `mise use node@22` (writes `mise.toml` in repo root + installs)
   - Global default: `mise use -g node@22`
   - After hand-editing `mise.toml`: run `mise install`
2. `command not found` for a language tool → check `mise ls` first. Shims
   (`~/.local/share/mise/shims`) are already on PATH, so a missing binary
   usually means it's not in mise yet — add it there. One-off run:
   `mise x node@20 -- node -v`.
3. Runtime version pins live in `mise.toml` only. Don't create `.nvmrc`,
   `.node-version`, or `.python-version`.
4. Global CLIs default to mise: e.g. `mise use -g npm:pnpm` (other backends
   work too: `aqua:`, `cargo:`, `pipx:`, `ubi:`).
   - If a CLI's supported self-update or in-app update flow requires an npm
     global install, allow `npm install -g <package>` using mise-managed
     Node/npm. Preserve that update flow when choosing an installation method.
   - Keep one installation method per CLI. When migrating, install and verify
     the replacement first, then remove the old installation and its mise
     configuration, and update any daemon/service to use the new executable.
   - npm's global prefix may depend on the active Node version. After switching
     Node versions, check whether those CLIs need reinstalling and service
     paths need updating.
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
