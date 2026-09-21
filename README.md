# Multiboot Toolkit

**BOOT • INSTALL • RECOVER • DIAGNOSE**

A professional, reusable theme and configuration project for [Ventoy](https://www.ventoy.net)-based multiboot USB drives. Designed for IT technicians, system administrators, and field engineers who need a clean, organized boot environment.

---

## What Is This?

Multiboot Toolkit is a **development repository** that provides:

- A custom GRUB2 theme for Ventoy (background, icons, menu styling)
- A pre-configured `ventoy.json` with organized categories
- Clean display names and per-distro icons
- Documentation for USB integration and customization

This repository is the **source of truth** — the finished files are copied onto a Ventoy USB for deployment. No ISOs are stored here.

## What Is Ventoy?

[Ventoy](https://www.ventoy.net) is an open-source tool that creates bootable USB drives. Instead of flashing one ISO at a time, you install Ventoy once and then simply copy ISO files onto the USB. Ventoy presents a boot menu listing all available ISOs.

This project enhances Ventoy with:
- A professional dark theme
- Organized category folders (TreeView mode)
- Distro-specific icons in the boot menu
- Clean display names via aliases
- Windows 11 compatibility bypasses

---

## Supported Categories & ISOs

| Category | ISOs |
|---|---|
| **Operating Systems** | Windows 11, Ubuntu, Debian, Linux Mint, Pop!_OS |
| **Gaming** | Bazzite, PikaOS Niri, Nobara |
| **Security & Privacy** | Kali Linux, Tails |
| **Recovery & Repair** | SystemRescue, GParted Live, Clonezilla, Rescuezilla, Hiren's BootCD PE *(planned)* |
| **Diagnostics** | MemTest86+ |

---

## Repository Structure

```
multiboot-toolkit/
├── README.md                 ← This file
├── LICENSE                   ← MIT License
├── .gitignore
│
├── ventoy/                   ← Ventoy-compatible config (copy to USB)
│   ├── ventoy.json           ← Unified Ventoy configuration
│   └── theme/
│       └── multiboot-toolkit/
│           ├── theme.txt     ← GRUB2 theme definition
│           ├── background.png
│           ├── select_c.png
│           ├── menu_c.png
│           ├── scrollbar_thumb.png
│           ├── scrollbar_frame.png
│           └── icons/        ← 24 menu icons (32×32 PNG)
│
├── assets/
│   └── logo.svg              ← Project logo (for docs/README)
│
└── docs/
    ├── USB_LAYOUT.md          ← Expected USB folder structure
    └── screenshots/           ← Screenshots (added after testing)
```

---

## Installing the Theme onto a Ventoy USB

### Prerequisites

- A USB drive with Ventoy already installed ([download Ventoy](https://www.ventoy.net/en/download.html))
- The USB's first partition mounted (this happens automatically on most systems)

### Installation Steps

1. **Clone this repository** (or download it):
   ```bash
   git clone https://github.com/YOUR_USERNAME/multiboot-toolkit.git
   cd multiboot-toolkit
   ```

2. **Copy the `ventoy/` directory** to the root of your Ventoy USB:
   ```bash
   # Replace /mnt/ventoy with your USB mount point
   cp -r ventoy/ /mnt/ventoy/
   ```
   This places `ventoy.json` and the theme directory where Ventoy expects them.

3. **Create the ISO category folders** on the USB:
   ```bash
   mkdir -p /mnt/ventoy/ISOs/"01 - Operating Systems"
   mkdir -p /mnt/ventoy/ISOs/"02 - Gaming"
   mkdir -p /mnt/ventoy/ISOs/"03 - Security & Privacy"
   mkdir -p /mnt/ventoy/ISOs/"04 - Recovery & Repair"
   mkdir -p /mnt/ventoy/ISOs/"05 - Diagnostics"
   ```

4. **Copy your ISO files** into the appropriate category folders.

5. **Boot from the USB** — the theme and TreeView categories should appear automatically.

### Quick Verification

Before booting, you can verify the configuration:

- Check that `/ventoy/ventoy.json` exists on the USB root
- Check that `/ventoy/theme/multiboot-toolkit/theme.txt` exists
- Check that `/ventoy/theme/multiboot-toolkit/background.png` exists
- Check that the `icons/` directory contains `.png` files

On the boot menu, press **F5 → Check plugin json configuration** to validate `ventoy.json`.

---

## Updating the Theme

To update after making changes in this repository:

```bash
# From the repo directory, replace the theme on USB
rsync -av --delete ventoy/ /mnt/ventoy/ventoy/
```

Or selectively update individual files:

```bash
# Update just the background
cp ventoy/theme/multiboot-toolkit/background.png /mnt/ventoy/ventoy/theme/multiboot-toolkit/

# Update just ventoy.json
cp ventoy/ventoy.json /mnt/ventoy/ventoy/
```

---

## Adding / Removing ISOs

### Adding a New ISO

1. Copy the ISO file into the appropriate category folder on the USB:
   ```bash
   cp ubuntu-24.04-desktop-amd64.iso /mnt/ventoy/ISOs/"01 - Operating Systems"/
   ```

2. If the ISO's filename contains a recognized substring (e.g., `ubuntu`, `kali`, `debian`), the correct icon will appear automatically.

3. If you want a custom icon for a new ISO, add an entry to `menu_class` in `ventoy.json`:
   ```json
   { "key": "newdistro", "class": "newdistro" }
   ```
   Then place `newdistro.png` (32×32) in `ventoy/theme/multiboot-toolkit/icons/`.

### Removing an ISO

Simply delete the ISO file from the USB. No configuration changes needed.

### Adding a New Category

1. Create the folder on the USB: `mkdir /mnt/ventoy/ISOs/"06 - New Category"`
2. Add entries to `ventoy.json`:
   - `menu_alias`: `{ "dir": "/ISOs/06 - New Category", "alias": "New Category" }`
   - `menu_class`: `{ "dir": "/ISOs/06 - New Category", "class": "newcategory" }`
3. Create `newcategory.png` icon in `icons/`

---

## Customizing the Background

The background is a standard 1920×1080 PNG image.

### Requirements
- **Format**: PNG (required by GRUB2)
- **Resolution**: Must match the `gfxmode` setting in `ventoy.json` (default: `1920x1080`)
- **Location**: `ventoy/theme/multiboot-toolkit/background.png`

### Steps
1. Create or edit your background image (keep text in the top ~25% to avoid overlap with the boot menu)
2. Save as PNG at exactly 1920×1080
3. Replace `background.png` in the theme directory
4. Copy to USB and test

---

## Customizing Icons

### Icon Requirements
- **Format**: PNG with transparency
- **Size**: 32×32 pixels (recommended by GRUB2 theme standard)
- **Naming**: Must match the `class` value in `ventoy.json` (e.g., class `ubuntu` → `ubuntu.png`)
- **Location**: `ventoy/theme/multiboot-toolkit/icons/`

### Built-in Ventoy Icon Classes

These special class names are used by Ventoy internally:

| Class | Purpose |
|---|---|
| `vtoydir` | Default icon for directories (in TreeView) |
| `vtoyiso` | Default icon for ISO files with no matching class |
| `vtoyret` | "Go back" / return to parent directory |

### Adding a Custom Icon

1. Create a 32×32 PNG with transparent background
2. Save it to `ventoy/theme/multiboot-toolkit/icons/yourname.png`
3. Add a `menu_class` entry: `{ "key": "filename_substring", "class": "yourname" }`
4. Copy to USB and test

---

## Limitations

### Confirmed Limitations

- **Font format**: GRUB2 only supports `.pf2` font files. To use a custom font, you must convert it using `grub-mkfont`:
  ```bash
  grub-mkfont -s 16 -o CustomFont-16.pf2 CustomFont.ttf
  ```
  Then add the font path to the `fonts` array in `ventoy.json`.

- **Background resolution**: The background PNG must exactly match the `gfxmode` resolution. If the display doesn't support 1920×1080, GRUB2 will fall back to a lower resolution and the background may not display correctly.

- **Icon colors**: GRUB2 renders icons as-is — there's no dynamic theming or color adaptation. Icons should have good contrast against the dark theme.

- **No animated elements**: GRUB2 does not support animations, GIFs, or video backgrounds.

- **TreeView is folder-based**: Category organization depends entirely on your USB folder structure. There's no way to create virtual categories that span multiple folders.

- **menu_class matching is case-sensitive**: The `key` substring match is case-sensitive. `"ubuntu"` won't match `"Ubuntu"` in a filename.

- **Large menu_class arrays**: If you have many ISOs and a long `menu_class` array, boot menu loading may be slightly slower.

---

## Confirmed Ventoy Features Used

Every feature in this project has been verified against the [official Ventoy documentation](https://www.ventoy.net/en/plugin.html):

| Feature | Plugin | Documentation |
|---|---|---|
| Custom GRUB2 theme | `theme` | [plugin_theme.html](https://www.ventoy.net/en/plugin_theme.html) |
| Graphics resolution | `theme.gfxmode` | Same page |
| Ventoy version info position/color | `theme.ventoy_left/top/color` | Same page |
| TreeView as default menu mode | `control.VTOY_DEFAULT_MENU_MODE` | [plugin_control.html](https://www.ventoy.net/en/plugin_control.html) |
| Clean TreeView style (no DIR prefix) | `control.VTOY_TREE_VIEW_MENU_STYLE` | Same page |
| Search root restriction | `control.VTOY_DEFAULT_SEARCH_ROOT` | Same page |
| Win11 TPM/CPU bypass | `control.VTOY_WIN11_BYPASS_CHECK` | Same page |
| Win11 online account bypass | `control.VTOY_WIN11_BYPASS_NRO` | Same page |
| macOS dotfile filter | `control.VTOY_FILT_DOT_UNDERSCORE_FILE` | Same page |
| Display name aliases | `menu_alias` | [plugin_menualias.html](https://www.ventoy.net/en/plugin_menualias.html) |
| Per-ISO/folder icons | `menu_class` | [plugin_menuclass.html](https://www.ventoy.net/en/plugin_menuclass.html) |
| Built-in classes (vtoydir/vtoyiso/vtoyret) | `menu_class` | Same page |
| Hotkey tip labels | `@VTOY_HOTKEY_TIP@` | plugin_theme.html |

---

## Testing the Configuration Safely

### Method 1: JSON Validation (Before USB)

```bash
python3 -c "import json; json.load(open('ventoy/ventoy.json')); print('✓ Valid JSON')"
```

### Method 2: Ventoy's Built-in Checker (On USB)

1. Boot from the USB
2. Press **F5** to open the Tools menu
3. Select **Check plugin json configuration (ventoy.json)**
4. Review each plugin's validation output

### Method 3: QEMU Test Boot (Advanced)

If you have QEMU installed, you can test without rebooting:

```bash
# Create a test disk image from your USB
# WARNING: This reads from your USB — adjust device path carefully
sudo dd if=/dev/sdX of=ventoy_test.img bs=1M count=500
qemu-system-x86_64 -m 2G -drive file=ventoy_test.img,format=raw -boot d
```

### Method 4: Visual Inspection

Check that all referenced files exist:

```bash
# Verify all icons referenced in ventoy.json exist
python3 -c "
import json, os
cfg = json.load(open('ventoy/ventoy.json'))
icons_dir = 'ventoy/theme/multiboot-toolkit/icons'
missing = 0
for entry in cfg.get('menu_class', []):
    icon = entry['class'] + '.png'
    path = os.path.join(icons_dir, icon)
    if os.path.exists(path):
        print(f'  ✓ {icon}')
    else:
        print(f'  ✗ MISSING: {icon}')
        missing += 1
print(f'\n{\"All icons present!\" if missing == 0 else f\"{missing} icon(s) missing\"}'  )
"
```

---

## USB Integration

> **This repository is the development/source project.**
>
> The `ventoy/` directory in this repo maps directly to the `/ventoy/` directory on a Ventoy USB drive. The final integration workflow is:
>
> 1. Make changes in this repository
> 2. Test/validate the JSON and file paths
> 3. Copy `ventoy/` to the USB
> 4. Create the ISO category folders on the USB
> 5. Copy ISO files into the appropriate categories
> 6. Boot and verify

### Integration Checklist

- [ ] Ventoy is installed on the USB drive
- [ ] `ventoy/ventoy.json` copied to USB root `/ventoy/ventoy.json`
- [ ] `ventoy/theme/multiboot-toolkit/` copied to USB `/ventoy/theme/multiboot-toolkit/`
- [ ] ISO category folders created under `/ISOs/`
- [ ] ISO files placed in correct category folders
- [ ] Booted and verified theme appears
- [ ] Verified TreeView shows categories
- [ ] Verified icons display correctly
- [ ] Tested F5 → Check plugin json

---

## Hotkeys (Ventoy Built-in)

| Key | Action |
|---|---|
| **F1** | Show/hide help |
| **F2** | Browse/boot from local disk |
| **F3** | Toggle TreeView / ListView |
| **F4** | Local boot options |
| **F5** | Tools menu (check config, resolution, power) |
| **F7** | Toggle GUI / Text mode |
| **Enter** | Open folder / Boot selected ISO |
| **Esc** | Go back to parent folder |

---

## License

MIT License. See [LICENSE](LICENSE) for details.