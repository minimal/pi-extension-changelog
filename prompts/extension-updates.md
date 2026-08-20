---
description: Review pending pi extension package updates before running `pi update --extensions`
argument-hint: "[package ...]"
---

Review what changed in pending pi extension package updates and summarize the
changes for me before I decide whether to update. Scope: ${@:-all outdated packages}.

1. Run the changelog tool to collect the data:

   ```bash
   pi-extension-changelog $@
   ```

   If `pi-extension-changelog` is not on PATH, run it from this package instead:

   ```bash
   node ~/.pi/agent/npm/node_modules/pi-extension-changelog/pi-extension-changelog $@
   ```

2. If neither works, gather the data manually:
   - Read installed packages from `~/.pi/agent/npm/package.json` (`dependencies`)
     and each package's resolved version from `node_modules/<name>/package.json`.
   - For each, fetch `https://registry.npmjs.org/<name>` and compare
     `dist-tags.latest` with the installed version.
   - For outdated packages, resolve the GitHub repo from the npm `repository`
     field and fetch release notes from
     `https://api.github.com/repos/<owner>/<repo>/releases` (or the compare API
     between the installed and latest tags).

3. Summarize per package:
   - Version bump (installed → latest) and publish age.
   - Notable features and fixes; anything breaking, risky, or security-related.
   - If a package is part of a shared monorepo (multiple packages share one
     repo), say so — the diff spans the whole repo, not just that package.

4. If the output ends with a note that release notes were truncated, re-run
   with `--full` for those packages so nothing important is hidden.

5. Finish by asking whether to proceed: `pi update --extensions` (everything),
   `pi update --extension npm:<name>` (a single package), or skip for now.
