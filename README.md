<div align="center">

# The Void

**HWID spoofing with temporary and permanent modes, live per-title status, and one-click rollback.**

[![Platform](https://img.shields.io/badge/platform-Windows%2010%20%2F%2011-0078D4?style=flat-square)](#requirements)
[![Modes](https://img.shields.io/badge/modes-temporary%20%7C%20permanent-3c3c3d?style=flat-square)](#modes)
[![Anti-cheat](https://img.shields.io/badge/coverage-Vanguard%20%7C%20EAC%20%7C%20BattlEye%20%7C%20Ricochet%20%7C%20Javelin-b22222?style=flat-square)](#coverage)

</div>

<img width="1280" height="640" alt="thevoid" src="https://github.com/user-attachments/assets/d8a98d6b-aa7a-4ca1-8319-e56f65476481" />


---

The Void rewrites the hardware identifiers an anti-cheat reads from a Windows machine, so the machine presents as a different one without reinstalling the OS or swapping parts.

Two operating modes share one panel. **Temporary** applies identifiers for the current boot session and reverts on restart. **Permanent** writes a new identifier set that persists across reboots until an explicit rollback. Both take a snapshot before touching anything, and both roll back from the same panel.

## Requirements

- Windows 10 or 11, x64
- Administrator rights
- Secure Boot disabled (required for the driver component)
- Game and launcher fully closed before applying
- Reboot for permanent mode; temporary mode needs no reboot

## Modes

| | Temporary | Permanent |
| --- | --- | --- |
| Lifetime | Current boot session | Until rollback |
| Reboot required | No | Yes |
| Reverts automatically | Yes, on restart | No |
| Rollback | Implicit | One click |
| Snapshot taken | Yes | Yes |
| Intended use | Validate a title before committing | Sustained use on a flagged machine |

Temporary is the probe. Apply, launch, confirm the game starts and the identifier changed, then restart to revert. Permanent is applied only after a title is confirmed working.

## Coverage

| Anti-cheat | Temporary | Permanent |
| --- | --- | --- |
| Riot Vanguard | Yes | Yes |
| Easy Anti-Cheat | Yes | Yes |
| BattlEye | Yes | Yes |
| Ricochet | Yes | Yes |
| Javelin | Yes | Yes |
| Proprietary / in-house | Case by case | Case by case |

Per-title status is published live rather than fixed here: **clear**, **degraded**, or **down**, refreshed continuously. A title marked degraded works under specific launchers or Windows builds only. The published board supersedes this table.

## What is modified

Identifiers are grouped by surface. Each group is independently toggleable, and the snapshot records the original values for every group before writing.

- SMBIOS: system UUID, board serial, chassis serial, BIOS version string
- Disk: volume serials and physical device identifiers
- Network: adapter MAC addresses
- Registry: machine GUID and hardware-keyed product identifiers
- GPU: adapter description strings and device IDs

Applying a subset is supported and is the correct approach when only one surface is checked. Applying all groups when a title reads only SMBIOS increases the blast radius for no gain.

## Usage

1. Close the game and its launcher completely.
2. Launch The Void as administrator.
3. Select Temporary or Permanent, select identifier groups, apply.
4. Reboot if permanent mode was used.
5. Launch the game normally.
6. On completion, roll back from the panel — the snapshot restores every recorded value.

Rollback restores from the pre-apply snapshot rather than generating new identifiers. Running rollback twice is safe; the second call is a no-op.

## Limitations

- **A spoof is not a ban shield.** It changes what the anti-cheat reads on the machine. It does not change account state, and a new ban is still possible.
- Detection surfaces change with anti-cheat patches. Coverage can regress between releases without warning.
- Hardware-keyed licensing tools (some DAWs, CAD, Windows activation edge cases) may require reactivation after a permanent apply.
- Secure Boot must be off for the driver to load; machines dual-booting with BitLocker should suspend protection before applying.
- Proprietary anti-cheats are unsupported by default and need verification per title.
- Virtual machines are partially supported — nested identifiers are frequently not forwarded by the hypervisor.

## Notes

- Identifiers are generated locally; nothing is transmitted.
- The snapshot is written to disk before any write and is required for rollback. Do not delete it while a permanent spoof is active.
- Temporary mode holds no persistent state, so a crash before rollback loses nothing.

## Disclaimer

Independent tool, unaffiliated with any game publisher or anti-cheat vendor. Confirm the current status board before applying to an account of value. Use at your own risk.
