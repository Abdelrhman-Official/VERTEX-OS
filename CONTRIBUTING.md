# Contributing to Vertex OS

Thank you for your interest in contributing to Vertex OS. This document covers everything you need to know to go from zero to a merged pull request.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Ways to Contribute](#ways-to-contribute)
- [Development Setup](#development-setup)
- [Project Structure](#project-structure)
- [Making Changes](#making-changes)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Translations](#translations)
- [Reporting Bugs](#reporting-bugs)
- [Proposing Features](#proposing-features)

---

## Code of Conduct

All contributors are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md). We maintain a welcoming, respectful community — please read it before participating.

---

## Ways to Contribute

You don't need to write code to contribute to Vertex OS. Here are all the ways you can help:

| Contribution Type | Where to Start |
|---|---|
| 🐛 **Report a bug** | [Open an issue](https://github.com/vertex-os/vertex-os/issues/new?template=bug_report.yml) |
| 💡 **Suggest a feature** | [Open a feature request](https://github.com/vertex-os/vertex-os/issues/new?template=feature_request.yml) |
| 🌍 **Add a translation** | See [Translations](#translations) |
| 🧪 **Test on new hardware** | See [Hardware Testing](#hardware-testing) |
| 📖 **Improve docs** | Edit files in `docs/wiki/` |
| 🎨 **UI/UX design** | Post in the `#design` Discord channel |
| 🔧 **Fix a bug** | Browse [good first issues](https://github.com/vertex-os/vertex-os/labels/good%20first%20issue) |
| ✨ **Implement a feature** | Browse [help wanted](https://github.com/vertex-os/vertex-os/labels/help%20wanted) |

---

## Development Setup

### Prerequisites

- Ubuntu 22.04 LTS **or** Debian 12 Bookworm as the **build host**
- 8 GB RAM minimum on the build host
- 50 GB free disk space
- Internet access during the build (packages are downloaded into the chroot)
- A non-root user with `sudo` access

### One-Command Setup

```bash
git clone https://github.com/vertex-os/vertex-os.git
cd vertex-os
bash scripts/setup-build-env.sh
```

This installs all build dependencies (`live-build`, `debootstrap`, `squashfs-tools`, `xorriso`, and more) and verifies your environment is ready.

### Building the ISO

```bash
bash build/build.sh
```

The resulting ISO will be at `build/output/vertex-os-1.0-amd64.iso`.

> **Tip:** First builds take 20–60 minutes. Subsequent builds are faster if you use `lb build` directly without `lb clean` (the cache is preserved between runs).

### Running in QEMU

```bash
bash scripts/run-qemu.sh
# or manually:
qemu-system-x86_64 \
  -m 2048 \
  -smp 4 \
  -enable-kvm \
  -vga virtio \
  -display gtk \
  -cdrom build/output/vertex-os-1.0-amd64.iso
```

### Developing Apps Without a Full Build

The GTK4 Python apps in `src/apps/` can be run directly on any system with the dependencies installed:

```bash
# Install dependencies
sudo apt install python3-gi python3-gi-cairo gir1.2-gtk-4.0 gir1.2-adw-1
pip3 install psutil pynput pillow pytesseract zeroconf --break-system-packages

# Run an app directly
python3 src/apps/files/main.py
python3 src/apps/settings/main.py
```

---

## Making Changes

### Step 1: Fork and clone

```bash
# Fork the repo on GitHub, then:
git clone https://github.com/YOUR_USERNAME/vertex-os.git
cd vertex-os
git remote add upstream https://github.com/vertex-os/vertex-os.git
```

### Step 2: Create a branch

Use a descriptive branch name:

```bash
# For bug fixes:
git checkout -b fix/issue-123-wifi-nm-crash

# For features:
git checkout -b feat/vertexshare-progress-toast

# For documentation:
git checkout -b docs/update-building-guide
```

### Step 3: Make your changes

Follow the [coding standards](#coding-standards) below. Keep commits focused — one logical change per commit.

### Step 4: Write or update tests

If you're adding or changing functionality, update the relevant test files in `tests/`. See [Testing](#testing).

### Step 5: Commit

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): short summary in present tense

Longer explanation if needed. Wrap at 72 characters.
Explain WHY the change was made, not just what.

Closes #123
```

**Types:** `feat` · `fix` · `docs` · `style` · `refactor` · `test` · `chore` · `build`  
**Scopes:** `shell` · `files` · `settings` · `store` · `shield` · `share` · `lens` · `clip` · `theme` · `build` · `docs` · `i18n`

**Examples:**

```
feat(share): add transfer progress toast notification
fix(shell): prevent taskbar from hiding behind fullscreen apps
docs(wiki): add hardware compatibility table
chore(build): bump DXVK to 2.3.1
```

### Step 6: Push and open a PR

```bash
git push origin your-branch-name
```

Then open a pull request on GitHub. Fill in the PR template completely.

---

## Pull Request Process

1. **Title** — follow the same format as commit messages: `feat(scope): short summary`
2. **Description** — fill in the full PR template
3. **Tests** — all CI checks must pass before review
4. **Review** — at least one maintainer review is required
5. **Squash** — maintainers squash merge to keep `main` history clean

PRs that touch the build system, ISO output, or core shell components require **two** maintainer approvals.

### What we look for in PRs

- Does it solve a real problem?
- Is the code readable and maintainable?
- Does it follow the design system (colors, typography, spacing)?
- Are edge cases handled (low RAM, offline, missing hardware)?
- Are there tests or manual test steps documented?

---

## Coding Standards

### Python (GTK4 apps and shell)

- Python 3.10+
- Follow [PEP 8](https://pep8.org/)
- Max line length: **100 characters**
- Use type hints for all function signatures
- Docstrings for all public classes and functions (Google style)
- No wildcard imports (`from module import *`)

```python
# Good
def get_clipboard_history(max_items: int = 100) -> list[str]:
    """Return the most recent clipboard entries.

    Args:
        max_items: Maximum number of entries to return.

    Returns:
        List of clipboard strings, newest first.
    """
    ...

# Bad
def get_history(n=100):
    ...
```

Linting is enforced by `flake8` and `black` in CI. Run locally:

```bash
pip3 install black flake8
black src/
flake8 src/ --max-line-length=100
```

### Bash (build scripts and hooks)

- Always start with:
  ```bash
  #!/bin/bash
  set -e
  set -u
  set -o pipefail
  trap 'echo "❌ FAILED at line $LINENO" && exit 1' ERR
  ```
- Quote all variables: `"$VAR"` not `$VAR`
- Use `||` with `true` for intentionally-fallible commands
- Comment non-obvious lines
- Test with `shellcheck`: `shellcheck build/config/hooks/live/*.hook.chroot`

### CSS (GTK theme)

- Use only the defined CSS variables — no hardcoded hex colors in component rules
- All new selectors must be added to `src/theme/gtk.css` with a comment header
- Transitions: `0.15s ease` for hover/focus — no longer durations in interactive elements
- Respect the spacing scale: `4px · 8px · 12px · 16px · 24px · 32px`

### Commit hygiene

- No "WIP", "fix stuff", or "asdf" commits in the final PR
- Keep commits bisectable — each commit should leave the repo in a working state
- Reference issue numbers: `Closes #123` or `Part of #456`

---

## Testing

### Running the QA checklist

```bash
bash scripts/run-tests.sh
```

This boots the ISO in QEMU headlessly and runs automated checks from `tests/`.

### Manual test categories

| Category | Location | What to check |
|---|---|---|
| Boot | `tests/boot/` | Plymouth, GRUB, Lite Mode auto-detect |
| Desktop | `tests/desktop/` | Taskbar, Start Menu, wallpaper, window controls |
| Apps | `tests/apps/` | All built-in apps launch and work |
| Hardware | `tests/hardware/` | WiFi, USB, Bluetooth, sleep/wake |
| Security | `tests/security/` | Firewall, telemetry OFF, lock screen |

### Hardware Testing

If you have access to real hardware, especially laptops or older machines, please test and report results in [GitHub Discussions](https://github.com/vertex-os/vertex-os/discussions) under the **Hardware Compatibility** category.

Include: CPU, GPU, RAM, Wi-Fi chip, and which tests from the QA checklist passed or failed.

---

## Translations

Vertex OS ships with 22 languages. To add or improve a translation:

1. Translation strings are in `src/i18n/` as `.po` files (gettext format)
2. The template file is `src/i18n/vertex-os.pot`
3. Each language has its own file: `src/i18n/ar.po`, `src/i18n/fr.po`, etc.

**Adding a new language:**

```bash
# Copy the template
cp src/i18n/vertex-os.pot src/i18n/YOUR_LANG_CODE.po

# Edit the file with your translations
# Use Poedit (GUI) or any .po editor

# Submit a PR with:
# - your new .po file
# - an update to src/i18n/LANGUAGES.md listing the new language
```

**RTL languages (Arabic, Persian):** Make sure to test UI mirroring. A test checklist is in `tests/desktop/rtl-check.md`.

---

## Reporting Bugs

Before opening a bug report:

1. Check the [existing issues](https://github.com/vertex-os/vertex-os/issues) — it may already be reported
2. If it's a security issue — see [SECURITY.md](SECURITY.md) — **do not open a public issue**
3. Make sure you're on the latest release

**Open a bug report:** [New Bug Report](https://github.com/vertex-os/vertex-os/issues/new?template=bug_report.yml)

A good bug report includes:
- Vertex OS version (shown in Settings → System → About)
- Hardware (CPU, GPU, RAM)
- Steps to reproduce (numbered, minimal, reproducible)
- What you expected vs. what happened
- Logs from `journalctl -b -p err` if applicable

---

## Proposing Features

**Open a feature request:** [New Feature Request](https://github.com/vertex-os/vertex-os/issues/new?template=feature_request.yml)

Before proposing a large feature:
- Check the [roadmap](https://github.com/vertex-os/vertex-os/projects) — it may already be planned
- Discuss it in `#dev` on Discord first to get early feedback

Feature proposals should address:
- What problem does it solve?
- Who benefits from it (all users? advanced users? specific hardware?)
- Does it fit Vertex OS's philosophy (lightweight, familiar, private)?
- Any implementation ideas?

---

## Maintainers

| Name | Role | GitHub |
|---|---|---|
| Vertex OS Core Team | Lead maintainers | [@vertex-os](https://github.com/vertex-os) |

To become a maintainer, contribute consistently over 3+ months, demonstrate good judgment in reviews, and be nominated by an existing maintainer.

---

## Recognition

All contributors are listed in [CONTRIBUTORS.md](CONTRIBUTORS.md). Significant contributors are highlighted in release notes.

Thank you for making Vertex OS better. 🎉
