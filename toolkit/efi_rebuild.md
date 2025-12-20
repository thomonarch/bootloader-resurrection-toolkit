# EFI Rebuild Guide
This guide walks through rebuilding the EFI System Partition (ESP) and restoring the Windows bootloader on a UEFI system. Use this when Windows won’t boot after removing Linux, wiping partitions, or otherwise breaking the EFI setup.

> This assumes a UEFI + GPT system and Windows 10/11.

---

## 1. Boot into Windows Recovery

1. Boot from Windows installation media (USB/DVD).
2. On the first setup screen, open Command Prompt using:
   - Shift + F10, or
   - Repair your computer -> Troubleshoot -> Advanced options -> Command Prompt

All commands below are run from that Command Prompt.

---

## 2. Identify the EFI Partition

Run:

diskpart
list disk
select disk 0
list volume

Look for a small FAT32 partition (100–300 MB), usually labeled SYSTEM or EFI.

Assign it a drive letter:

select volume <EFI volume number>
assign letter=S:
exit

Now the EFI partition is mounted as S:.

---

## 3. (Optional) Recreate the EFI Partition Structure

Only do this if the EFI partition is corrupted or empty.

diskpart
select disk 0
select volume <EFI volume number>
format fs=fat32 quick
assign letter=S:
exit

This wipes and reformats the EFI partition.

---

## 4. Find Your Windows Installation Drive

In WinRE, drive letters often shift. Check which one contains Windows:

dir C:\
dir D:\
dir E:\

Look for the one containing:

Windows
Program Files
Users

Assume it’s C: for the next step, but adjust if needed.

---

## 5. Rebuild Windows Boot Files with bcdboot

Run:

bcdboot C:\Windows /s S: /f UEFI

Where:

- C:\Windows = your actual Windows installation
- /s S: = the EFI partition
- /f UEFI = force creation of UEFI boot files

If successful, you’ll see:

Boot files successfully created.

This recreates:

- EFI\Microsoft\Boot\bootmgfw.efi
- A fresh BCD store
- Windows Boot Manager entry

---

## 6. Verify the EFI Structure (Optional)

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

If these folders exist, the EFI is correctly rebuilt.

---

## 7. Fix Boot Order in Firmware

1. Reboot and enter UEFI firmware (F2, Esc, Del, etc.).
2. Go to Boot or Boot Order.
3. Ensure Windows Boot Manager is present.
4. Move it to the top.
5. Save and reboot.

---

## 8. If Windows Still Doesn’t Boot

Try the classic bootrec tools:

bootrec /scanos
bootrec /rebuildbcd

If /rebuildbcd finds your Windows install, type Y to add it.

If /fixboot gives “Access is denied,” that’s normal on modern Windows — bcdboot is the correct tool.

If Windows still doesn’t boot, re-check:

- Correct Windows drive letter
- Correct EFI drive letter
- EFI partition is FAT32
- EFI partition is GPT, not MBR
- You didn’t accidentally point bcdboot at the wrong Windows folder

---

## 9. Summary

This guide covers:

- Identifying the EFI partition
- Assigning it a drive letter
- Rebuilding the bootloader with bcdboot
- Fixing BCD issues
- Ensuring Windows Boot Manager is restored
- Verifying EFI structure
- Fixing boot order

If everything is correct, Windows should boot cleanly after these steps.
