# My Changes — Non-Interactive Install

All interactive prompts in the HyDE install scripts have been removed and replaced with hardcoded choices, so the install runs fully non-interactively.

## Choices Made

| Prompt | File | Chosen value |
|---|---|---|
| AUR helper | `install.sh` | `yay-bin` |
| Default shell | `install.sh` | `zsh` |
| Grub theme | `install_pre.sh` | `Retroboot` (dark) |
| Chaotic AUR | `install_pre.sh` | Install (yes) |
| SDDM theme | `install_pst.sh` | `Corners` |
| Flatpak packages | `install_pst.sh` | Skip (no) |
| oh-my-zsh plugins | `restore_shl.sh` | Install (yes) |
| Reboot after install | `install.sh` | No (message only) |
| Chaotic AUR reinstall | `chaotic_aur.sh` | Skip if already installed |
| pacman update/refresh | `install_pre.sh` | Pass `--noconfirm` via `use_default` |

## Files Changed

### `Scripts/install.sh`
- **AUR helper prompt** — replaced `prompt_timer` + case with direct `export getAur="yay-bin"`
- **Shell prompt** — replaced `prompt_timer` + case + validation with direct `export myShell="zsh"` + append to package list
- **Reboot prompt** — replaced `read` + conditional reboot with a simple info message to reboot later

### `Scripts/install_pre.sh`
- **Grub theme prompt** — replaced `read` + case + conditional with direct `Retroboot` theme extraction and GRUB config
- **pacman update** — added `${use_default:+"$use_default"}` to `pacman -Syyu` and `pacman -Fy` so they respect `--noconfirm` when the `-d` flag is used
- **Chaotic AUR prompt** — replaced `prompt_timer` + case with direct installation block

### `Scripts/install_pst.sh`
- **SDDM theme prompt** — replaced `read` + case with direct `Corners` theme extraction
- **Flatpak prompt** — replaced `prompt_timer` with unconditional skip

### `Scripts/restore_shl.sh`
- **oh-my-zsh prompt** — removed `prompt_timer` call; the default is already `"y"` via the `${PROMPT_INPUT:-y}` fallback, so oh-my-zsh installs automatically

### `Scripts/chaotic_aur.sh`
- **Reinstall prompt** — when Chaotic AUR is already installed, the script now prints "Skipping reinstall" and exits instead of asking the user
