My Bootloader Resurrection Story
I wanted to remove Linux from my main PC because it just wasn’t working properly. My Logitech mouse wouldn’t connect over Bluetooth, the NVIDIA drivers were annoying to deal with, and it felt easier to just go back to Windows. So I deleted the Linux partitions thinking it would be a quick cleanup.

That’s when things broke.

After removing the partitions, Windows started acting strange. It would boot normally, but every time I tried to shut it down, it would show “Shutting down…”, the screen would go black, and then it would go straight back to the login screen. No error, nothing. Just a loop.

At that point I knew something was messed up with the boot setup.

Since Linux had been installed before, GRUB was on the EFI partition. Removing Linux left the EFI and the Windows bootloader in a bad state. Windows could still start, but the underlying bootloader files and the BCD store were basically damaged.

So I went into recovery mode and fixed it manually:

Booted into Windows Recovery

Opened Command Prompt

Used diskpart to check the partitions

Mounted or recreated the EFI partition

Used bcdboot to rebuild the Windows bootloader

Windows booted again after that, but because the BCD store was new, Windows didn’t recognize the usual {current} entry anymore. Any command that used it failed with errors about the volume being “externally altered.” So the system was booting, but the identifiers were different from before.

The shutdown issue was still happening too.

To fix that, I did the following:

Turned off Fast Startup

Disabled hybrid shutdown in the registry

Disabled automatic restart on failure

Reset all power schemes

Forced a full shutdown using shutdown /s /f /t 0

After doing all of that, the PC finally shut down properly again instead of bouncing back to the login screen.

At that point I stopped messing with my main PC for the night. I switched to my Linux Mint laptop, which I use as a sandbox, and checked my GitHub repo.

Two people had already viewed and downloaded it.

That’s what made me decide to turn this whole situation into a toolkit, so anyone else who ends up in the same mess has something to follow instead of figuring it out the hard way.
