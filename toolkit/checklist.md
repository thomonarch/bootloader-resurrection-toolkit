# Bootloader Recovery Checklist

A quick checklist for diagnosing and fixing Windows bootloader issues.
# Bootloader Recovery Checklist

A quick, practical checklist for diagnosing and fixing Windows bootloader issues after removing Linux, modifying partitions, or encountering EFI/BCD corruption. This assumes a UEFI + GPT Windows 10/11 system.

---

## 1. Before Doing Anything

- Confirm the system uses UEFI + GPT (not Legacy/MBR).
- If possible, back up important data.
- Have Windows installation media or a recovery USB ready.
- Know that drive letters in WinRE may differ from normal Windows.

---

## 2. Boot Into Windows Recovery

- Boot from Windows installation media.
- Open Command Prompt using:
  - Shift + F10, or
  - Repair your computer -> Troubleshoot -> Advanced options -> Command Prompt

---

## 3. Identify Partitions

Use diskpart:

diskpart
list disk
select disk 0
list volume

Identify:

- Windows partition (large NTFS volume)
- EFI partition (small FAT32 volume, 100–300 MB)

Assign letters:

select volume <EFI volume>
assign letter=S:
select volume <Windows volume>
assign letter=C:
exit

---

## 4. Rebuild Bootloader (Primary Fix)

Use bcdboot to recreate EFI boot files:

bcdboot C:\Windows /s S: /f UEFI

This should recreate:

- EFI\Microsoft\Boot\bootmgfw.efi
- A fresh BCD store
- Windows Boot Manager entry

---

## 5. Verify EFI Structure

Check the EFI partition:

S:
dir

You should see:

EFI

Then:

cd EFI
dir

You should see:

Microsoft
Boot

If these folders exist, the EFI structure is correct.

---

## 6. Optional: Use bootrec Tools

bootrec /scanos
bootrec /rebuildbcd

If Windows is detected, type Y to add it.

Note: bootrec /fixboot often gives “Access is denied” on modern Windows. This is normal.

---

## 7. Firmware (BIOS/UEFI) Checks

- Reboot into firmware (F2, Esc, Del, etc.).
- Ensure “Windows Boot Manager” exists.
- Move it to the top of the boot order.
- Disable leftover Linux entries if present.
- Save and reboot.

---

## 8. If Windows Still Won’t Boot

Re-check:

- Correct Windows drive letter in WinRE
- Correct EFI drive letter
- EFI partition is FAT32
- EFI partition is GPT, not MBR
- bcdboot pointed to the correct Windows folder
- EFI partition wasn’t accidentally assigned the wrong volume

If the EFI partition is empty or corrupted, recreate it:

diskpart
select disk 0
select volume <EFI volume>
format fs=fat32 quick
assign letter=S:
exit

Then rerun bcdboot.

---

## 9. After Successful Boot

- Restart multiple times to confirm stability.
- Ensure no GRUB or leftover boot entries remain.
- Optionally clean up unused partitions.
- Optionally disable Fast Startup to avoid hybrid boot issues.

---

## Summary

This checklist covers:

- Identifying EFI and Windows partitions
- Assigning drive letters
- Rebuilding the bootloader with bcdboot
- Verifying EFI structure
- Using bootrec for detection
- Fixing boot order in firmware
- Recreating the EFI partition if needed

Following this checklist should restore Windows Boot Manager and allow normal booting.
