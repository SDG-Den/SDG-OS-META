# SDG-OS-META Documentation Plan

## Current Status
No documentation exists beyond a two-word README. No docs/ or tips/ directories.

## Docs System (`docs/`)
**Deploy location**: `~/.local/docs/SDG-OS-META/`

### Planned Doc Topics
| # | Topic | Description | Priority |
|---|-------|-------------|----------|
| 1 | What is SDG-OS-META | Metapackage concept, what it installs, prerequisites | High |
| 2 | Installation Guide | How to bootstrap and run the metapackage | High |
| 3 | Package Reference | List of all 12 child packages with descriptions | Medium |
| 4 | Customization | How to modify install.sh to add/remove packages | Low |

### Implementation
- Create `docs/SDG-OS-META/` directory with markdown files
- Follow SDG-DOCS naming convention
- Add deployment to `install.sh`

## Tips System (`tips/`)
**Deploy location**: `~/.local/tips/SDG-OS-META/`

### Planned Tips
| # | Tip | Description | Priority |
|---|-----|-------------|----------|
| 1 | One-command desktop | `bash install.sh` from SDG-OS-META installs the full SDG-OS desktop | High |
| 2 | Requires sdgpkg | SDG-PKG must be bootstrapped first | High |

### Implementation
- Create `tips/SDG-OS-META/tips.list`
- Add deployment to `install.sh`
