<div align="center">

<img src="docs/assets/vertex-logo.svg" width="120" alt="Vertex OS Logo" />

# Vertex OS

**A modern Linux distribution built for humans.**  
Fast. Beautiful. Private. Built on Debian 12 Bookworm.

[![Build Status](https://img.shields.io/github/actions/workflow/status/vertex-os/vertex-os/build.yml?branch=main&style=flat-square&label=ISO%20Build&color=0078D7)](https://github.com/vertex-os/vertex-os/actions/workflows/build.yml)
[![Latest Release](https://img.shields.io/github/v/release/vertex-os/vertex-os?style=flat-square&color=27AE60&label=Latest)](https://github.com/vertex-os/vertex-os/releases/latest)
[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg?style=flat-square)](LICENSE)
[![Discord](https://img.shields.io/discord/000000000?style=flat-square&logo=discord&label=Community&color=5865F2)](https://discord.gg/vertex-os)
[![X](https://img.shields.io/badge/Follow-%40VertexOS-black?style=flat-square&logo=x)](https://x.com/vertexos)

[**Download**](https://vertex-os.io/download) · [**Documentation**](https://vertex-os.io/docs) · [**Screenshots**](https://vertex-os.io/screenshots) · [**Roadmap**](https://github.com/vertex-os/vertex-os/projects)

</div>

---

## What is Vertex OS?

Vertex OS is a Debian-based Linux distribution designed to be immediately familiar to Windows users while delivering a genuinely modern experience — hardware-accelerated animations, an AirDrop-style file sharing, a built-in Windows app compatibility layer, and zero telemetry by default.

It runs well on 1 GB of RAM. It looks like a 2025 OS. It doesn't spy on you.

---

## Screenshots

<table>
  <tr>
    <td><img src="docs/assets/screenshot-desktop.png" alt="Desktop" /></td>
    <td><img src="docs/assets/screenshot-start.png" alt="Start Menu" /></td>
  </tr>
  <tr>
    <td align="center"><b>Desktop — Hyprland + Waybar</b></td>
    <td align="center"><b>Start Menu</b></td>
  </tr>
  <tr>
    <td><img src="docs/assets/screenshot-files.png" alt="Files" /></td>
    <td><img src="docs/assets/screenshot-settings.png" alt="Settings" /></td>
  </tr>
  <tr>
    <td align="center"><b>Vertex Files</b></td>
    <td align="center"><b>Settings</b></td>
  </tr>
</table>

---

## Key Features

| Feature | Details |
|---------|---------|
| 🖥️ **Display** | Wayland (Hyprland) · X11 Sway fallback · Openbox Lite |
| ⚡ **Performance** | ~280 MB RAM at idle (Full Mode) · ~120 MB (Lite Mode) |
| 🔒 **Privacy** | Zero telemetry by default · UFW firewall · ClamAV built-in |
| 🪟 **Windows Apps** | Wine 9 + DXVK 2.3.1 + VKD3D-Proton via Exora |
| 📁 **File Sharing** | VertexShare — AirDrop-style LAN transfer |
| 📋 **Clipboard** | VertexClip — 100-item history, pinning, search |
| 🔍 **OCR** | VertexLens — screen region to text in one hotkey |
| 🎮 **Gaming** | Gaming Mode · Games Hub · Steam/GOG/itch.io integration |
| 🌍 **Languages** | 22 languages at launch, full RTL support |
| 💾 **Installer** | Calamares — same installer used by Manjaro, KDE Neon |

---

## System Requirements

| | Minimum | Recommended |
|--|---------|-------------|
| **CPU** | 2 cores (x86_64) | 4+ cores |
| **RAM** | 1 GB | 4 GB |
| **Storage** | 8 GB | 25 GB |
| **GPU** | Any (VESA) | Vulkan-capable (Intel/AMD) |
| **UEFI/BIOS** | Both supported | UEFI |

> **Auto Lite Mode:** If Vertex OS detects less than 1 GB of RAM at boot, it switches to Openbox automatically — no configuration needed.

---

## Quick Start

### Download the ISO

```bash
# Download the latest stable release
wget https://vertex-os.io/releases/vertex-os-1.0-amd64.iso

# Verify the checksum
sha256sum -c vertex-os-1.0-amd64.iso.sha256
```

### Test in QEMU (fastest — no install needed)

```bash
sudo apt install qemu-system-x86 -y

qemu-system-x86_64 \
  -m 2048 \
  -smp 4 \
  -enable-kvm \
  -vga virtio \
  -display gtk \
  -cdrom vertex-os-1.0-amd64.iso
```

### Flash to USB

```bash
# Replace /dev/sdX with your USB device
sudo dd if=vertex-os-1.0-amd64.iso of=/dev/sdX bs=4M status=progress oflag=sync

# Or use the graphical tool
sudo apt install balena-etcher
```

---

## Building from Source

> **Requirements:** Ubuntu 22.04 LTS or Debian 12 host · 8 GB RAM · 50 GB free disk · Internet access

### 1. Clone the repository

```bash
git clone https://github.com/vertex-os/vertex-os.git
cd vertex-os
```

### 2. Install build dependencies

```bash
sudo apt update && sudo apt install -y \
  live-build debootstrap squashfs-tools xorriso \
  grub-pc-bin grub-efi-amd64-bin mtools dosfstools \
  git wget curl build-essential python3 python3-pip \
  syslinux-utils isolinux calamares
```

### 3. Run the build

```bash
bash build/build.sh
```

The ISO will be at `build/output/vertex-os-1.0-amd64.iso`.  
Full build time: **20–60 minutes** depending on internet speed.

For detailed build instructions, see [**docs/wiki/Building.md**](docs/wiki/Building.md).

---

## Architecture

```
┌────────────────────────────────────────────────────────────┐
│                      USER APPS                             │
│  Nebula │ Files │ Office │ Media │ Terminal │ Settings     │
├────────────────────────────────────────────────────────────┤
│               VERTEX SHELL  (Python + GTK4)                │
│  Waybar (taskbar) │ Start Menu │ Notification Center       │
│  greetd + vertex-greeter (lock/login) │ swaybg (wallpaper) │
├────────────────────────────────────────────────────────────┤
│             WAYLAND COMPOSITOR  (Hyprland)                 │
│   Blur · rounded corners · animations · gestures           │
│   Fallback 1: Sway (Wayland, older GPUs)                   │
│   Fallback 2: Openbox + Picom (X11 Lite Mode)              │
├────────────────────────────────────────────────────────────┤
│               SYSTEM SERVICES  (systemd)                   │
│   NetworkManager │ PipeWire │ UPower │ Flatpak             │
│   ClamAV │ UFW │ udisks2 │ bluez                           │
├────────────────────────────────────────────────────────────┤
│               EXE COMPATIBILITY LAYER  (Exora)             │
│   Wine 9 │ DXVK 2.3.1 │ VKD3D-Proton 2.12 │ Proton GE    │
├────────────────────────────────────────────────────────────┤
│               LINUX KERNEL  6.6 LTS                        │
│   BFQ scheduler │ zswap │ zram │ KMS/DRM │ HZ_250         │
└────────────────────────────────────────────────────────────┘
```

---

## Repository Structure

```
vertex-os/
├── .github/
│   ├── ISSUE_TEMPLATE/       # Bug report, feature request, app request templates
│   ├── workflows/            # CI/CD: ISO build, lint, QA, release
│   └── PULL_REQUEST_TEMPLATE.md
├── build/
│   ├── auto/                 # live-build auto scripts (config, clean)
│   ├── config/
│   │   ├── package-lists/    # APT package lists per component
│   │   ├── hooks/live/       # Chroot hooks (repos, installs, theming)
│   │   ├── includes.chroot/  # Static files copied into the live system
│   │   └── calamares/        # Installer configuration + branding
│   ├── build.sh              # Master build script
│   └── output/               # Built ISOs land here (gitignored)
├── src/
│   ├── shell/                # Vertex Shell: taskbar, greeter, notifications
│   ├── apps/                 # All GTK4 Python apps (files, settings, store…)
│   ├── games/                # SDL2 mini games (minesweeper, blocks, snake)
│   ├── installer/            # Calamares module patches + OOBE scripts
│   └── theme/                # GTK CSS, icon SVGs, Plymouth theme
├── scripts/
│   ├── setup-build-env.sh    # One-command build host setup
│   ├── run-tests.sh          # Run the QA checklist in QEMU
│   └── release.sh            # Tag + publish a new release
├── tests/
│   ├── boot/                 # Boot test cases
│   ├── desktop/              # Desktop/WM test cases
│   ├── apps/                 # App smoke tests
│   ├── hardware/             # Hardware compatibility tests
│   └── security/             # Security and privacy validation
├── docs/
│   ├── assets/               # Logos, screenshots, diagrams
│   └── wiki/                 # Extended documentation
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE                   # GNU GPL v3
└── SECURITY.md
```

---

## Built-in Apps

| App | Description |
|-----|-------------|
| **Nebula** | Chromium-based browser — DuckDuckGo default, uBlock Origin, no Google sync |
| **Vertex Files** | File manager — Windows Explorer layout, colored folders, tabs, real-time search |
| **Vertex Word / Slides / Calc** | LibreOffice suite, re-themed with Vertex Dark skin |
| **Vertex Terminal** | foot (Wayland) / xterm (X11) with Fish shell and tab support |
| **Vertex Pad** | Code and text editor with syntax highlighting |
| **Vertex Media** | MPV-backed player with GTK4 UI, subtitles, playlist, mini player |
| **Vertex Monitor** | Task Manager — CPU/RAM/disk/network graphs, process kill |
| **Vertex Shield** | ClamAV antivirus with real-time monitoring, quarantine, definition auto-update |
| **Vertex Store** | Flatpak + .deb + AppImage app store |
| **Vertex Capture** | Screenshot tool — region/window/fullscreen, annotations |
| **VertexShare** | AirDrop-style LAN file sharing — right-click any file to send |
| **VertexLens** | OCR — `Super+Shift+L` to extract text from any screen region |
| **VertexClip** | Clipboard manager — `Super+V` for history, search, pin, sync |
| **Exora** | Windows .exe compatibility via Wine 9 + DXVK + Proton GE |
| **Games Hub** | Game launcher with Steam/GOG/itch.io links + 3 built-in games |

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Super` | Open Start Menu |
| `Super + E` | Open File Manager |
| `Super + L` | Lock screen |
| `Super + V` | Open VertexClip history |
| `Super + Shift + S` | Screenshot (region select) |
| `Super + Shift + L` | VertexLens OCR |
| `Super + D` | Show desktop |
| `Super + ←/→` | Snap window left/right |
| `Super + ↑` | Maximize window |
| `Ctrl + Alt + T` | Open Terminal |
| `Alt + F4` | Close window |
| `Super + 1–9` | Switch virtual desktop |

---

## Design System

Vertex OS uses a consistent design language across all components.

**Colors**

| Token | Value | Use |
|-------|-------|-----|
| `--vertex-blue` | `#0078D7` | Primary accent, focus rings, active states |
| `--bg-base` | `#0A0A0F` | Desktop background |
| `--bg-window` | `#141418` | App windows |
| `--bg-taskbar` | `rgba(12,12,18,0.85)` | Taskbar + blur |
| `--text-primary` | `#FFFFFF` | Headings, active labels |
| `--text-secondary` | `#CCCCCC` | Body text |

**Typography:** DM Sans (UI body) · Outfit (clock, wordmarks) · Noto Sans (fallback/CJK)  
**Corner radius:** 12px (windows) · 8px (buttons, inputs) · 4px (small elements)  
**Window controls:** macOS-style (left side) — Red `#FF5F57` · Yellow `#FEBC2E` · Green `#28C840`

See [**docs/wiki/Design-System.md**](docs/wiki/Design-System.md) for the full specification.

---

## Contributing

We welcome contributions of all kinds — bug fixes, new features, translations, documentation, and testing.

**Quick start:**

```bash
git clone https://github.com/vertex-os/vertex-os.git
cd vertex-os
bash scripts/setup-build-env.sh   # sets up your build environment
```

See [**CONTRIBUTING.md**](CONTRIBUTING.md) for the full guide, including:
- How to report bugs
- How to submit pull requests
- Coding standards (Python, Bash, CSS)
- How to add or update translations
- How to write and run tests

---

## Roadmap

| Phase | Timeline | Status |
|-------|----------|--------|
| **Alpha** — Core desktop, Calamares installer, base apps | Month 1–2 | 🟡 In Progress |
| **Beta** — VertexShare, VertexLens, VertexClip, Games Hub | Month 3–4 | ⚪ Planned |
| **1.0 Release** — NVIDIA support, Btrfs snapshots, website | Month 5–6 | ⚪ Planned |
| **Post-1.0** — VertexSync, VertexCast, mobile companion | Ongoing | ⚪ Future |

See the full [**Project Board**](https://github.com/vertex-os/vertex-os/projects) for open issues, in-progress work, and milestone tracking.

---

## Community

- 💬 **Discord:** [discord.gg/vertex-os](https://discord.gg/vertex-os) — main community hub
- 🐦 **X / Twitter:** [@VertexOS](https://x.com/vertexos)
- 📣 **Announcements:** [GitHub Releases](https://github.com/vertex-os/vertex-os/releases)
- 🐛 **Bug reports:** [GitHub Issues](https://github.com/vertex-os/vertex-os/issues)

---

## Security

If you discover a security vulnerability, **please do not open a public issue.**

Report it privately via [security@vertex-os.io](mailto:security@vertex-os.io) or through GitHub's [Private Security Reporting](https://github.com/vertex-os/vertex-os/security/advisories/new).

We aim to respond within **48 hours** and release a patch within **7 days** for critical issues.

See [**SECURITY.md**](SECURITY.md) for our full disclosure policy.

---

## License

Vertex OS is open source software released under the [GNU General Public License v3.0](LICENSE).

Individual components may carry their own licenses:

| Component | License |
|-----------|---------|
| Vertex OS (this repo) | GPL-3.0 |
| Debian base system | Various (GPL, LGPL, MIT) |
| Hyprland | BSD-3-Clause |
| Wine | LGPL-2.1 |
| DXVK | zlib |
| Calamares | GPL-3.0 |
| LibreOffice | MPL-2.0 |

---

<div align="center">

Built with ❤️ by the Vertex OS team and contributors worldwide.

[vertex-os.io](https://vertex-os.io) · [Changelog](CHANGELOG.md) · [Code of Conduct](CODE_OF_CONDUCT.md)

</div>
