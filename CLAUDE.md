# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Repository Overview

This repository builds Arch Linux ARM system images for the FydeTab Duo tablet using the ImageForge build system. Images are bootable from SD card or flashable to eMMC.

## Build Commands

**Prerequisites:**
```bash
sudo pacman -S --needed python parted btrfs-progs git wget \
  arch-install-scripts gptfdisk dosfstools multipath-tools python-imageforge
```

**Build image:**
```bash
cd ~/repos/tweakz-fydetab-hacks/tweakz-fydetab-hacks/images
sudo ./fydetab-arch/profiledef -c fydetab-arch -w ./work -o ./out
```

**Output:** `out/ArchLinux-ARM-FydeTab-Duo-Gnome-YYYY-MM-DD.img.xz`

**Flash to SD card:**
```bash
xzcat out/*.img.xz | sudo dd of=/dev/sdX bs=4M status=progress conv=fsync
```

## Key Files

| File | Purpose |
|------|---------|
| `fydetab-arch/profiledef` | Python build script (ImageForge config) |
| `fydetab-arch/packages.aarch64` | Package list to install |
| `fydetab-arch/pacman.conf.aarch64` | Pacman repos used during build |
| `fydetab-arch/rootfs/` | Files copied into the image root filesystem |
| `fydetab-arch/fydetab-duo_UEFI.img` | UEFI bootloader blob (embedded in image) |

## Customizations

**Added packages** (beyond upstream):
- `ccache`, `git` - Development tools
- `zsh`, `tmux`, `tealdeer` - Shell/terminal
- `paru`, `waydroid` - AUR helper and Android containers

**Shell configuration:**
- `rootfs/etc/skel/.zshrc` - Custom PATH only; zsh4humans bootstraps the rest

## Related Repository

Custom kernel and device packages are built from:
```
~/repos/tweakz-fydetab-hacks/tweakz-fydetab-hacks/pkgbuilds/linux-fydetab-itztweak/
```

The image pulls `linux-fydetab-itztweak` and other custom packages from the Fyde repository configured in `pacman.conf.aarch64`.

## Build Space Requirements

- Work directory: ~8-12GB
- Final compressed image: ~2-4GB
- Do NOT use `/tmp/work` - tmpfs is only 3.8GB

## SD Card Boot Log Analysis

After booting from SD card, analyze boot logs from the mounted filesystem:

**Mount location:** `/run/media/$USER/ROOTFS/`

**Journal location:** `/run/media/$USER/ROOTFS/@log/journal/<machine-id>/`

```bash
# List boots
journalctl --file=/run/media/$USER/ROOTFS/@log/journal/*/system.journal --list-boots

# View specific boot (replace machine-id with actual hex string)
journalctl --file=/run/media/$USER/ROOTFS/@log/journal/<machine-id>/system.journal -b 0

# GPU/driver issues
journalctl --file=/run/media/$USER/ROOTFS/@log/journal/<machine-id>/user-1001.journal -b 0 | grep -iE "gnome-shell|MESA|dri|EGL"
```
