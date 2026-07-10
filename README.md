# SDG-OS-META

Metapackage that installs the full SDG-OS desktop in one command.

## Description

SDG-OS-META is a convenience metapackage. Its only purpose is to install all core SDG-OS packages in sequence via `sdgpkg install`. It bootstraps the complete desktop environment from a single script.

## What It Installs

```bash
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

Note: SDG-TERM is not listed here — it is installed as a dependency of SDG-MANGO-CORE.

## Prerequisites

- SDG-PKG must already be bootstrapped (`sdgpkg` available on PATH)
- Git and curl must be available

## Installation

```bash
sdgpkg install sdgos-meta
```

Or run manually from a local clone:

```bash
git clone https://git.sdgcloud.nl/SDGDen/SDG-OS-META
cd SDG-OS-META
bash install.sh
```

## Dependencies

- `sdgpkg` (SDG-PKG) — must be bootstrapped first
- `git` — required by sdgpkg
- `curl` — required by sdgpkg

## Related Packages

All of them — this metapackage is the single entry point for a complete SDG-OS installation.
