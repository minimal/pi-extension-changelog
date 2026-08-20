# pi-extension-changelog

Show what changed in pending pi extension package updates before you run
`pi update --extensions`.

Provides a pi **prompt template** (`/extension-updates`) that instructs the model to
collect and summarize the changes. Nothing runs automatically — the prompt only
expands when you invoke it, so it costs zero context otherwise.

## Usage

In a pi session, type:

```
/extension-updates                # all outdated packages
/extension-updates pi-web-access  # specific package(s)
```

The model then collects the data and summarizes per package (version bump,
features/fixes, breaking or security-relevant changes), and asks whether to
proceed with `pi update --extensions`.

Data collection uses the bundled `pi-extension-changelog` CLI when available
(installed packages vs npm registry + GitHub release notes/commit diffs via
`gh`), and falls back to direct npm/GitHub API lookups with built-in fetch
tools otherwise.

## Standalone CLI (optional)

The package also ships a CLI for use outside pi:

```bash
pi-extension-changelog                # all outdated packages
pi-extension-changelog pi-web-access  # specific package(s)
pi-extension-changelog --full         # untruncated release notes + commit lists
pi-extension-changelog --json         # machine-readable output
pi-extension-changelog --all          # include up-to-date packages
pi-extension-changelog --max-notes 2000  # cap release-note length
pi-extension-changelog --agent-dir DIR   # override ~/.pi/agent
```

Requires Node >= 18 and `gh` on PATH.

The default text output caps each package's release notes at `--max-notes`
chars (default 5000) and ends with a note when anything is truncated — re-run
with `--full` for the untruncated notes. `--json` output is never truncated.
Identical release notes shared across packages from the same monorepo are
printed once; the other packages point back to them.
