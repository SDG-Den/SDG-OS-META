# SDG-OS-META Analysis

## Type
Metapackage (Dependency Aggregation)

## Description
SDG-OS-META is a minimal Bash metapackage whose sole purpose is to install 12 child SDG-OS packages in sequence via `sdgpkg install`. It is a convenience metapackage for installing the "standard" set of SDG-OS modules in one command.

## Usage
SDG-OS-META is used as a one-shot installer to set up a complete SDG-OS desktop. Prerequisites:
1. SDG-PKG must already be bootstrapped (`sdgpkg` available on PATH)
2. The SDG-OS repository indices must be configured in `~/.config/SDG-PKG/`

To use:
```bash
git clone https://git.sdgcloud.nl/SDGDen/SDG-OS-META
cd SDG-OS-META
bash install.sh
```

This runs `sdgpkg install` for all 12 child packages in sequence. Each package's `install.sh` handles dependency resolution via `unipkg` and file deployment.

The metapackage is **not** registered in SDG-REPO's `SDGOS.repo`, so `sdgpkg install sdgos-meta` will not work — it must be run manually from a local clone.

## Files
| File | Purpose |
|------|---------|
| `README.md` | Minimal description ("metapackage") |
| `install.sh` | One-shot installer: calls `sdgpkg install` for each child package |

## install.sh Contents
```bash
#!/bin/bash
# requires sdgpkg to already be bootstrapped
sdgpkg install unipkg
sdgpkg install sdg-fetch
sdgpkg install sdg-docs
sdgpkg install sdg-mango
sdgpkg install sdg-tips
sdgpkg install sdg-util
sdgpkg install sdg-themes
sdgpkg install sdg-wayshell
sdgpkg install sdg-wayshell-conf
sdgpkg install sdg-vox
sdgpkg install sdg-glyphs
sdgpkg install sdg-mango-helpers
```

## Required Dependencies
| Dependency | Required? | Notes |
|------------|-----------|-------|
| sdgpkg (SDG-PKG) | Yes | Must be bootstrapped before running this |
| git | Yes | Required by sdgpkg |
| curl | Yes | Required by sdgpkg |

## Dependents
None (top-level metapackage)

## Notes
- Missing `update.sh`, `uninstall.sh`, `detect.sh` lifecycle scripts
- Not registered in any SDG-REPO `.repo` file (must be run manually)
- Package names in install.sh use shorthand (e.g., `sdg-mango` vs `SDG-MANGO-CORE`)
- Does NOT install `sdg-term` (SDG-TERM) — it is installed as a dependency of SDG-MANGO-CORE instead
