# SDG-OS-META — Repository Analysis

## Function

SDG-OS-META is a **metapackage** (a "meta" or "umbrella" module) for the SDG-OS ecosystem. Its sole purpose is to install a curated set of core SDG-OS modules in one shot via `install.sh`. It is not a library, tool, or configuration provider — it delegates all work to `sdgpkg`.

## Dependencies

SDG-OS-META depends on `sdgpkg` being already bootstrapped on the target system (`install.sh` line 3: `# requires sdgpkg to already be bootstrapped`). The `sdgpkg` command is used to install all 11 child modules via `sdgpkg install <name>`.

The `sdgpkg` tool itself depends on:
- `git` — to clone repositories (`sdgpkg.sh` line 55)
- `curl` — to fetch repository index files (`sdgpkg.sh` line 48)
- `unipkg` — referenced as the first installed package in `install.sh` line 4
- A configured repository file (e.g. `50-core.repo` pointing to `SDGOS.repo`) — without this, `sdgpkg` cannot resolve any package URL (`sdgpkg.sh` lines 20–27).

Repo configuration is at `SDG-PKG/config/SDG-PKG/50-core.repo:1` → `https://git.sdgcloud.nl/SDGDen/SDG-REPO/raw/branch/main/SDGOS.repo`.

## Dependents

Within the SDG-OS-V2 workspace, **no other module references SDG-OS-META**. A grep for `SDG-OS-META`, `SDG-META`, `sdg-meta`, or `SDG_META` across all repos returned zero results outside of this repo's own `README.md`. The metapackage is not listed in any repo index file (`50-core.repo`, `60-containers.repo`, `70-extra.repo`, `80-projects.repo`) and therefore cannot be installed by `sdgpkg install sdg-meta` as-is.

## Use / Configuration

| Item | Value |
|------|-------|
| Entry point | `install.sh` — a top-level lifecycle script |
| Install command | `bash install.sh` (run manually, or via `sdgpkg` if registered) |
| Arguments | None accepted |
| User config | None — no config directory or user-facing files |
| Git remote | Not configured; the repo exists only locally under `SDG-OS-V2/SDG-OS-META/` |

## Known Issues

### 1. Missing `sdg-term` from install list

`sdg-term` is installed as a dependency by `SDG-MANGO-CORE` (`SDG-MANGO-CORE/install.sh` line 23), but it is **not listed in `SDG-OS-META/install.sh`**. This means:
- If a user runs `sdg-meta` before `sdg-mango-core`, `sdg-term` is skipped entirely.
- The metapackage does not form a complete, dependency-ordered install plan — it relies on `sdg-mango-core` to pull in `sdg-term` as a side effect.

### 2. No `update.sh` script

Per the lifecycle contract (`SDG-DOCS/docs/SDG-DOC-DEVS/03-lifecycle-scripts.md` lines 29–39), every module should have an `update.sh` to handle `sdgpkg update` and `sdgpkg upgrade`. SDG-OS-META has none. Running `sdgpkg update sdg-meta` will fail because `sdgpkg.sh` line 65 calls `runfile update.sh`, which will not exist.

### 3. No `uninstall.sh` script

Per the lifecycle contract (`03-lifecycle-scripts.md` lines 18–27), every module should have an `uninstall.sh`. SDG-OS-META has none. Running `sdgpkg uninstall sdg-meta` will fail at `sdgpkg.sh` line 76.

### 4. Not registered in any `SDGOS.repo`

SDG-OS-META has no entry in `SDGOS.repo` (or any of the `.repo` files under `SDG-PKG/config/SDG-PKG/`). The four repo files are:

| File | URL |
|------|-----|
| `50-core.repo:1` | `https://git.sdgcloud.nl/SDGDen/SDG-REPO/raw/branch/main/SDGOS.repo` |
| `60-containers.repo:1` | `https://git.sdgcloud.nl/SDGDen/SDG-REPO/raw/branch/main/SDG-CONTAINERS.repo` |
| `70-extra.repo:1` | `https://git.sdgcloud.nl/SDGDen/SDG-REPO/raw/branch/main/SDG-EXTRA.repo` |
| `80-projects.repo:1` | `https://git.sdgcloud.nl/SDGDen/SDG-REPO/raw/branch/main/SDG-PROJECTS.repo` |

None contain a `sdg-meta` entry. This means `sdgpkg install sdg-meta` will fail — the loop in `sdgpkg.sh` lines 47–60 iterates repos but finds no matching package, exits silently without installing anything.

### 5. No error handling in `install.sh`

`install.sh` chains 11 `sdgpkg install` commands with no error checking, no `set -e`, no logging, and no rollback. If any single `sdgpkg install` fails (network error, missing repo entry, git clone failure, dependency conflict), the script continues blindly. The user sees no indication of which step failed.

### 6. No `detect.sh`

Per the lifecycle reference (`03-lifecycle-scripts.md` lines 41–46), `detect.sh` is an optional script that checks system dependencies before install. SDG-OS-META has none. For a metapackage, a `detect.sh` could verify that `sdgpkg` is bootstrapped and that the SDGOS.repo is reachable before attempting any installs.

### 7. Only two commits in history

```
f04ebf5 Initial commit
643a913 updated for test
```

The repo has no tags, no release branches, and the second commit only added `install.sh`. The README is a single word: `metapackage`.

## Summary of Missing Files

| Required File | Status | Impact |
|---|---|---|
| `install.sh` | ✅ Exists (11 modules listed) | Works if run manually |
| `update.sh` | ❌ Missing | `sdgpkg update` / `sdgpkg upgrade` fails |
| `uninstall.sh` | ❌ Missing | `sdgpkg uninstall` fails |
| `detect.sh` | ❌ Missing | No pre-flight check |
| Repo registration | ❌ Missing | `sdgpkg install sdg-meta` not possible |