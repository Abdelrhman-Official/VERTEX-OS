<div align="center">

<img src="assets/logo.png" alt="Vertex OS Logo" width="120" height="120" />

# Vertex OS

### A modern, lightweight Linux distribution built on Debian 12 Bookworm

Fast • Minimal • Elegant • Built for everyone

<p>
  <a href="https://github.com/Abdelrhman-Official/VERTEX-OS/releases/latest">
    <img src="https://img.shields.io/github/v/release/Abdelrhman-Official/VERTEX-OS?style=for-the-badge&color=6C63FF&label=Latest%20Release" alt="Latest Release" />
  </a>
  <a href="https://github.com/Abdelrhman-Official/VERTEX-OS/releases">
    <img src="https://img.shields.io/github/downloads/Abdelrhman-Official/VERTEX-OS/total?style=for-the-badge&color=6C63FF" alt="Downloads" />
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/github/license/Abdelrhman-Official/VERTEX-OS?style=for-the-badge&color=6C63FF" alt="License" />
  </a>
</p>

<p>
  <a href="https://github.com/Abdelrhman-Official/VERTEX-OS/issues">
    <img src="https://img.shields.io/github/issues/Abdelrhman-Official/VERTEX-OS?style=for-the-badge&color=FFB347" alt="Issues" />
  </a>
  <a href="https://github.com/Abdelrhman-Official/VERTEX-OS/stargazers">
    <img src="https://img.shields.io/github/stars/Abdelrhman-Official/VERTEX-OS?style=for-the-badge&color=FFD700" alt="Stars" />
  </a>
  <img src="https://img.shields.io/badge/Status-Beta%201.0-orange?style=for-the-badge" alt="Status" />
</p>

---
## Overview

Vertex OS is a lightweight Linux distribution based on Debian 12 (Bookworm), designed to be modern, familiar, and fast on both old and new hardware. It ships with a polished Wayland desktop (Sway) with an X11 fallback (Openbox) for maximum hardware compatibility.

| Attribute | Value |
|---|---|
| **Base** | Debian 12 Bookworm (stable) |
| **Kernel** | Linux 6.1 LTS |
| **Architecture** | x86\_64 (64-bit) |
| **Init System** | systemd |
| **Display Server** | Sway (Wayland) · Openbox (X11 fallback) |
| **ISO Type** | Bootable hybrid (BIOS + UEFI) |
| **Status** | Beta 1.0 |

---

## ✨ Features

- 🚀 **Lightweight** — Runs on as little as 512 MB RAM
- 🖥️ **Dual desktop** — Sway (Wayland) with Openbox fallback for old GPUs
- 🔊 **Modern audio** — PipeWire + WirePlumber
- 🔌 **Live boot** — Try before you install via squashfs overlay
- 🛠️ **Ready out of the box** — Terminal, file manager, audio, networking all included
- 📦 **Debian-based** — Access to the entire Debian/Ubuntu package ecosystem

---

## 💾 Download

> **Vertex OS Beta 1.0** is the current release.

| File | Size | Type |
|---|---|---|
| `vertex-os-1.0-beta-x86_64.iso` | ~TBD | Bootable Hybrid ISO |

👉 **[Go to Releases →](https://github.com/YOUR_USERNAME/vertex-os/releases/latest)**

### Verify your download

After downloading, verify the SHA256 checksum from the release page:
```bash
sha256sum vertex-os-1.0-beta-x86_64.iso
```
Compare the output to the `SHA256SUMS` file attached to the release.

---

## 🖥️ System Requirements

| Component | Minimum | Recommended |
|---|---|---|
| **RAM** | 512 MB | 2 GB+ |
| **CPU** | 1 GHz single-core | 2 GHz dual-core |
| **Disk** | 5 GB | 20 GB+ |
| **GPU** | Any KMS/DRM capable | Modern GPU for Wayland |

---

## ⚡ Quick Start

### Flash to USB (Linux/macOS)

```bash
# Replace /dev/sdX with your USB drive — double check before running!
sudo dd if=vertex-os-1.0-beta-x86_64.iso of=/dev/sdX bs=4M status=progress oflag=sync
```

### Flash to USB (Windows)

Use [Rufus](https://rufus.ie) or [balenaEtcher](https://etcher.balena.io/). Select the ISO, choose your USB drive, and click Flash.

### Default Credentials

| User | Password |
|---|---|
| `vertex` | `vertex` |
| `root` | `vertex` |

> ⚠️ Change passwords immediately after installation.

---

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Super + Enter` | Open terminal (Foot) |
| `Super + D` | App launcher (Wofi) |
| `Super + Q` | Close window |
| `Super + F` | Toggle fullscreen |
| `Super + Space` | Toggle floating |
| `Super + H/J/K/L` | Focus left/down/up/right |
| `Super + Shift + H/J/K/L` | Move window |
| `Super + 1–4` | Switch workspace |

---

## 🗺️ Roadmap

| Phase | Status | Contents |
|---|---|---|
| Phase 1–2 | ✅ Done | Base system, Sway desktop, core utilities |
| Phase 3 | 🔄 Planned | GTK theme + branding (Vertex theme, logos, wallpapers) |
| Phase 4 | 🔄 Planned | Core Vertex apps (Settings, Files, Store) |
| Phase 5 | 🔄 Planned | EXE Simulator, games, security tools |

---

## 🏗️ Build It Yourself

See the full build guide in [docs/build-guide.md](docs/build-guide.md).

**Build host requirements:**
- Linux (Debian or Ubuntu recommended)
- 8 GB RAM, 50 GB free disk
- Required packages: `debootstrap squashfs-tools xorriso grub-pc-bin grub-efi-amd64-bin mtools dosfstools`

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/vertex-os.git
cd vertex-os

# Install build dependencies
sudo apt install debootstrap squashfs-tools xorriso grub-pc-bin grub-efi-amd64-bin mtools dosfstools

# Run the build pipeline
sudo bash scripts/build.sh
```

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting issues or pull requests.

- 🐛 [Report a bug](https://github.com/YOUR_USERNAME/vertex-os/issues/new?template=bug_report.yml)
- 💡 [Request a feature](https://github.com/YOUR_USERNAME/vertex-os/issues/new?template=feature_request.yml)
- 📖 [Improve the docs](docs/)

---

## 🔐 Security

Found a security vulnerability? Please do **not** open a public issue.  
See [SECURITY.md](SECURITY.md) for responsible disclosure instructions.

---

## 📜 License

Vertex OS is free software. The build scripts and configuration in this repository are released under the [MIT License](LICENSE). Debian packages included in the distribution are each covered by their own respective licenses (GPL, MIT, Apache, etc.).

---

<div align="center">
  Made with ❤️ — Built on the shoulders of Debian
</div>
