# Bootloader Resurrection Toolkit

A practical, field-tested collection of guides for restoring the Windows bootloader after removing Linux, wiping partitions, or breaking the EFI/BCD setup. This toolkit exists because I went through the entire process myself — from a dead bootloader to a fully resurrected system — and documented every step that actually worked.

If Windows refuses to boot, loops back to recovery, or won’t shut down properly, this toolkit gives you the exact commands and checks needed to bring it back.

---

## Version

v1.0.0 — First public release

---

## Badges

Windows 10/11 • UEFI + GPT • Real-World Tested • No BS • Human-Written

---

## Contents

### Core Guides
- EFI Rebuild Guide  
  Recreate the EFI System Partition and restore Windows Boot Manager.  
  toolkit/efi_rebuild.md

- BCD Repair Guide  
  Fix corrupted or missing BCD stores and rebuild boot entries.  
  toolkit/bcd_repair.md

- Shutdown Loop Fix  
  Solve the issue where Windows refuses to shut down and jumps back to the lock screen.  
  toolkit/shutdown_fix.md

- Bootloader Recovery Checklist  
  A fast, structured checklist for diagnosing and repairing bootloader problems.  
  toolkit/checklist.md

---

## Quick Start (If Windows Won’t Boot)

1. Boot into Windows Recovery (Shift + F10).
2. Identify EFI + Windows partitions:

   diskpart  
   list volume

3. Assign letters:

   assign letter=S:  (EFI)  
   assign letter=C:  (Windows)

4. Rebuild bootloader:

   bcdboot C:\Windows /s S: /f UEFI

5. Reboot and set Windows Boot Manager first in BIOS.

Full details are in the EFI and BCD guides.

---

## Why This Toolkit Exists

This isn’t a theoretical collection of commands.  
It’s the exact process I used to recover a Windows system after:

- Removing Linux  
- Breaking the EFI partition  
- Losing Windows Boot Manager  
- Getting stuck in WinRE  
- Rebuilding everything from scratch  

Every guide is written from real troubleshooting, not guesswork.

---

## Philosophy

- No fluff — only steps that matter  
- No theory — everything here was used in a real recovery  
- No assumptions — every command is explained  
- No fear — EFI and BCD aren’t magic, just files and folders  
- No gatekeeping — this is for anyone who needs it  

---
## Tested On

- Windows 11
- UEFI + GPT
- Real hardware (personal PC)
- After Linux removal
- Successful EFI + BCD rebuild
- Verified shutdown fixes

## Who This Toolkit Helps

- Anyone who removed Linux and broke Windows booting  
- Anyone stuck in WinRE with “missing or corrupted boot configuration”  
- Anyone who wiped or damaged the EFI partition  
- Anyone whose system restarts instead of shutting down  
- Anyone who wants a clean, reliable recovery process  

---

## Common Errors and What They Mean

- “Boot files missing”  
  EFI partition is empty or overwritten.

- “The boot configuration data for your PC is missing or contains errors”  
  BCD store is corrupted or missing.

- “Access is denied” when running bootrec /fixboot  
  Normal on modern Windows. Use bcdboot instead.

- Windows restarts instead of shutting down  
  Fast Startup or hybrid sleep interfering.

- Windows boots straight into BIOS  
  Boot order is wrong or EFI partition is unreadable.

---

## Troubleshooting FAQ

Q: My EFI partition doesn’t show up.  
A: It may be unformatted or deleted. Recreate it using the EFI guide.

Q: bcdboot says “Failure when attempting to copy boot files.”  
A: Wrong Windows drive letter. Check with dir C:\, dir D:\, etc.

Q: Windows Boot Manager doesn’t appear in BIOS.  
A: Rebuild EFI, then reboot into BIOS and refresh boot entries.

Q: Shutdown still loops back to login.  
A: Disable Fast Startup, hybrid sleep, and wake devices.

---

## Roadmap

- v1.1.0: Add troubleshooting flowchart  
- v1.2.0: Add “Common WinRE Drive Letter Mappings” reference  
- v1.3.0: Add optional scripts for automated checks (Windows-safe)  
- v2.0.0: Add full “Disaster Recovery Mode” guide  

---

## License

MIT License — use it, modify it, share it.

---

## Author

Created by Thom  
GitHub: https://github.com/thomonarch

If this toolkit helped you fix your system, consider starring the repo. It helps others find it and shows that real-world recovery guides matter.
