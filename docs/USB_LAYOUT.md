# USB Folder Layout

This document describes the expected folder layout on the Ventoy USB drive after integrating the Multiboot Toolkit theme and configuration.

## Ventoy Partition (First Partition) Structure

```
USB_ROOT/
│
├── ventoy/                              ← Ventoy system config directory
│   ├── ventoy.json                      ← Main configuration (copy from repo)
│   └── theme/
│       └── multiboot-toolkit/           ← Theme directory
│           ├── theme.txt                ← GRUB2 theme definition
│           ├── background.png           ← 1920×1080 boot background
│           ├── select_c.png             ← Selection highlight tile
│           ├── menu_c.png               ← Menu background tile
│           ├── scrollbar_thumb.png      ← Scrollbar thumb
│           ├── scrollbar_frame.png      ← Scrollbar track
│           └── icons/                   ← Menu class icons (32×32 PNG)
│               ├── os.png
│               ├── gaming.png
│               ├── security.png
│               ├── recovery.png
│               ├── diagnostics.png
│               ├── vtoydir.png
│               ├── vtoyiso.png
│               ├── vtoyret.png
│               ├── windows.png
│               ├── ubuntu.png
│               ├── debian.png
│               ├── mint.png
│               ├── popos.png
│               ├── bazzite.png
│               ├── pikaos.png
│               ├── nobara.png
│               ├── kali.png
│               ├── tails.png
│               ├── systemrescue.png
│               ├── gparted.png
│               ├── clonezilla.png
│               ├── rescuezilla.png
│               ├── hirens.png
│               └── memtest.png
│
└── ISOs/                                ← ISO storage root (VTOY_DEFAULT_SEARCH_ROOT)
    ├── 01 - Operating Systems/
    │   ├── Win11_*.iso
    │   ├── ubuntu-*-desktop-amd64.iso
    │   ├── debian-*-amd64-netinst.iso
    │   ├── linuxmint-*-cinnamon-64bit.iso
    │   └── pop-os_*_amd64_*.iso
    │
    ├── 02 - Gaming/
    │   ├── bazzite-*.iso
    │   ├── pikaos-*.iso
    │   └── nobara-*.iso
    │
    ├── 03 - Security & Privacy/
    │   ├── kali-linux-*-amd64.iso
    │   └── tails-amd64-*.iso
    │
    ├── 04 - Recovery & Repair/
    │   ├── systemrescue-*.iso
    │   ├── gparted-live-*.iso
    │   ├── clonezilla-live-*.iso
    │   ├── rescuezilla-*.iso
    │   └── Hirens_BootCD_PE_*.iso       (future)
    │
    └── 05 - Diagnostics/
        └── memtest86+-*.iso
```

## Key Points

- **`/ventoy/`** is a reserved Ventoy directory on the first partition. This is where `ventoy.json` and the theme must live.
- **`/ISOs/`** is configured as `VTOY_DEFAULT_SEARCH_ROOT` in `ventoy.json`, so Ventoy only scans this directory for bootable images. This speeds up boot and keeps the menu clean.
- **Category folders** use numbered prefixes (`01 -`, `02 -`, etc.) to guarantee alphabetical/sort order in TreeView mode.
- **`menu_alias`** in `ventoy.json` maps each numbered folder to a clean display name (e.g., `01 - Operating Systems` → `Operating Systems`).
- **`menu_class`** maps each folder to a category icon and each ISO filename substring to a distro-specific icon.

## Notes

- The `ventoy/` directory is automatically excluded from ISO scanning by Ventoy.
- Do **not** place ISOs directly in the USB root — they should always be inside `/ISOs/` subdirectories.
- If you add a new category folder, remember to add corresponding `menu_alias`, `menu_class`, and icon entries.
- The `.ventoyignore` file can be placed in any folder to exclude it from scanning.
