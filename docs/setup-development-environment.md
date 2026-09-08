# Setup Development Environment

## System Setup

1. [Install and activate mise](https://mise.jdx.dev/installing-mise.html)

2. Install Docker Desktop (or Docker Engine on Linux)

3. Configure github CLI with `gh auth login` and ensure you have access to the repository (optional, for convenience).

4. Install the graphviz dependency
   - macOS: `brew install graphviz`
   - Linux: `apt install graphviz`

5. Activate the virtual environment:
   ```bash
   # - Install all the tools defined in mise.toml (python, uv, ...)
   # - Create .venv on the pinned Python and activate it
   mise install

   # Usually unnecessary: [deps.uv] in mise.toml runs `uv sync --all-extras` automatically
   # before any `mise run` when uv.lock or pyproject.toml changed, or .venv is missing.
   mise run install

   # To recreate the virtualenv from scratch
   mise run reinstall
   ```

   Add and remove dependencies with `uv add` / `uv remove`, which update `pyproject.toml`
   and `uv.lock` together. (`mise deps add` does not support uv.)

   Run tools with `uv run <tool>` rather than bare — see
   [Building and Testing](building-and-testing.md) for why that matters when more than one
   project's virtualenv is in play.

6. Upgrade rsync if you're on macOS, as the default version is too old and lacks some features nbkp relies on:
   ```bash
   brew install rsync
   ```

7. (Optional) If you want to record the demo, install pv:
   ```bash
   brew install pv
   ```

## VSCode Setup

Accept the recommended extensions when VSCode prompts on first open — the set is committed in
`.vscode/extensions.json`. See
[VSCode setup](https://github.com/iglootools/common-guidelines/blob/main/guidelines/ide.md#vscode) in the
shared guidelines for what each one is for, and which committed settings silently stop working
without it.

No interpreter selection is needed: `.vscode/settings.json` and `[tool.pyright]` in `pyproject.toml`
already point the editor at `.venv`. See
[Pyright environment resolution](https://github.com/iglootools/common-guidelines/blob/main/guidelines/ide.md#pyright-environment-resolution)
in the shared guidelines for what those settings do, how to verify them, and why a window reload is
required after changing them.

## Claude Code Setup

Both plugins below are enabled for this repository in `.claude/settings.json`, but a plugin the
project enables still has to be installed once per machine.

For the shared iglootools guidelines, follow
[Install it per project](https://github.com/iglootools/common-guidelines#install-it-per-project-not-per-user)
in the shared guidelines, then
[Updating one project](https://github.com/iglootools/common-guidelines#updating-one-project) —
or [every project at once](https://github.com/iglootools/common-guidelines#updating-every-project)
— whenever a new version is released. The commands live there so they are corrected in one place.

Install the Pyright LSP plugin so Claude resolves symbols instead of grepping for them. Follow
[Claude Code setup in the shared guidelines](https://github.com/iglootools/common-guidelines/blob/main/guidelines/ide.md#claude-code)
once the virtualenv step above is done — it covers the install command, why `pyright-langserver` has
to come from this project's `.venv`, and how to verify that it does.
