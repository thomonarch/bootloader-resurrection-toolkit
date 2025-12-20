# Windows Shutdown Loop Fix
This guide explains how to fix the issue where Windows refuses to shut down properly. Common symptoms include:

- You click Shut Down, the screen goes black, and then the system jumps back to the lock screen.
- The system restarts instead of shutting down.
- The system powers off briefly and then turns itself back on.

These problems are usually caused by Fast Startup, power plan conflicts, hybrid sleep, or drivers that hang during shutdown.

---

## 1. Disable Fast Startup

Fast Startup can cause Windows to enter a hybrid sleep state instead of fully shutting down.

1. Open Control Panel.
2. Go to Hardware and Sound -> Power Options.
3. Click “Choose what the power buttons do”.
4. Click “Change settings that are currently unavailable”.
5. Uncheck “Turn on fast startup (recommended)”.
6. Save changes and test shutdown.

This forces Windows to perform a full shutdown.

---

## 2. Check Power Button and Lid Settings

Misconfigured power actions can make Windows ignore shutdown commands.

1. Go to Control Panel -> Power Options.
2. On the left, click “Choose what closing the lid does”.
3. Ensure:
   - “When I press the power button:” is set to “Shut down”.
   - “When I close the lid:” is set to Sleep or Do nothing (not conflicting).
4. Save changes.

If these settings conflict, Windows may switch to sleep or hibernate instead of shutting down.

---

## 3. Disable Automatic Restart on System Failure

If Windows is crashing during shutdown, it may instantly reboot instead of turning off.

1. Press Win + R.
2. Type: sysdm.cpl
3. Go to Advanced -> Startup and Recovery -> Settings.
4. Under System failure, uncheck “Automatically restart”.
5. Apply and OK.

If a BSOD appears during shutdown, note the error for further troubleshooting.

---

## 4. Check for Hanging Services or Processes

Some drivers or services can hang during shutdown and force Windows to restart.

- Open Task Manager (Ctrl + Shift + Esc).
- Look for processes using high CPU or disk when attempting shutdown.
- Update or temporarily uninstall problematic software (antivirus suites, vendor utilities, etc.).

You can also check Event Viewer:

1. Open Event Viewer.
2. Go to Windows Logs -> System.
3. Look for warnings or errors around the shutdown time.

Common culprits include network drivers, audio drivers, and OEM utilities.

---

## 5. Test a Clean Shutdown from Command Line

This helps determine whether the Start menu shutdown is broken or the OS itself cannot shut down.

Open Command Prompt as admin and run:

shutdown /s /t 0

If this works but the Start menu shutdown does not, the issue is likely:

- Shell integration problems
- Corrupted user profile
- Third-party software hooking into shutdown

Testing with a new local user account can help isolate this.

---

## 6. Prevent Devices from Waking the System

If the system shuts down but immediately turns back on, a device may be waking it.

Check wake-capable devices:

powercfg -devicequery wake_armed

Disable wake for network adapters:

1. Open Device Manager.
2. Expand Network adapters.
3. Right-click your adapter -> Properties.
4. Go to Power Management.
5. Uncheck “Allow this device to wake the computer”.

Do the same for keyboard/mouse if needed.

Also check firmware settings:

- Disable Wake on LAN.
- Disable Power on by PCI-E or USB.

---

## 7. Additional Fixes

### Reset Power Plans

powercfg -restoredefaultschemes

This resets all power plans to default.

### Disable Hybrid Sleep

1. Go to Power Options.
2. Change plan settings -> Change advanced power settings.
3. Expand Sleep.
4. Set “Allow hybrid sleep” to Off.

### Disable Fast Boot in BIOS/UEFI

Some systems have a firmware-level Fast Boot that interferes with shutdown.

---

## Summary

This guide covers:

- Disabling Fast Startup
- Fixing power button and lid settings
- Preventing automatic restart on failure
- Checking for hanging services or drivers
- Testing shutdown via command line
- Preventing devices from waking the system
- Resetting power plans and hybrid sleep
- Disabling firmware-level Fast Boot

After applying these fixes, Windows should shut down cleanly without looping back to the login screen or restarting.
