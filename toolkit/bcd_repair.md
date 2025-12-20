# BCD Repair Guide

This guide will cover how to recreate or repair the Windows BCD store.
# BCD Repair Guide
# BCD Repair Guide

This guide explains how to repair or recreate the Windows Boot Configuration Data (BCD) when the system fails to boot or shows errors like:

- "The boot configuration data for your PC is missing or contains errors."
- Windows not appearing as a boot option.
- Boot loops after removing Linux or modifying partitions.

These steps assume a UEFI + GPT Windows 10/11 system.

---

## 1. Boot into Windows Recovery

Boot from Windows installation media or a recovery USB.

Open Command Prompt using:

- Shift + F10 on the first setup screen, or
- Repair your computer -> Troubleshoot -> Advanced options -> Command Prompt

All commands below are run from that Command Prompt.

---

## 2. Identify Windows and EFI Partitions

Use diskpart:

diskpart
list disk
select disk 0
list volume

Identify:

- Windows partition (large NTFS volume containing Windows folder)
- EFI partition (small FAT32 volume, usually 100–300 MB)

Assign letters:

select volume <EFI volume number>
assign letter=S:
select volume <Windows volume number>
assign letter=C:
exit

Adjust C: if your Windows partition is on a different letter.

---

## 3. Backup or Remove the Old BCD Store

If the BCD is corrupted, rename it:

S:
cd \EFI\Microsoft\Boot
ren BCD BCD.backup

If the folder does not exist, it will be recreated in the next step.

---

## 4. Recreate the BCD with bcdboot

Use bcdboot to copy fresh boot files:

bcdboot C:\Windows /s S: /f UEFI

Where:

- C:\Windows = your actual Windows installation
- S: = EFI partition
- /f UEFI = force creation of UEFI boot files

If successful, this creates:

- A new BCD store
- A new Windows Boot Manager entry
- Fresh bootmgfw.efi files

---

## 5. Optional: Use bootrec Tools

These can help detect Windows installations:

bootrec /scanos
bootrec /rebuildbcd

If /rebuildbcd finds your Windows install, type Y to add it.

Note: bootrec /fixboot often gives "Access is denied" on modern Windows. This is normal and not a failure. bcdboot is the correct tool.

---

## 6. Verify the EFI Structure

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

## 7. Fix Boot Order in Firmware

1. Reboot and enter UEFI firmware (F2, Esc, Del, etc.).
2. Go to Boot or Boot Order.
3. Ensure Windows Boot Manager is present.
4. Move it to the top.
5. Save and reboot.

---

## 8. If Windows Still Does Not Boot

Re-check:

- Correct Windows drive letter in WinRE
- Correct EFI drive letter
- EFI partition is FAT32
- EFI partition is GPT, not MBR
- bcdboot pointed to the correct Windows folder

If the EFI partition was recreated, ensure it was formatted FAT32 and assigned correctly.

---

## Summary

This guide covers:

- Identifying Windows and EFI partitions
- Assigning drive letters
- Backing up or removing corrupted BCD
- Rebuilding BCD with bcdboot
- Using bootrec for detection
- Verifying EFI structure
- Fixing boot order in firmware

A correct BCD rebuild should restore Windows Boot Manager and allow normal booting.
