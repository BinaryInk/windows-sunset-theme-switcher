# Windows Sunset Mode Switcher

- Requires: Windows 11, PowerShell 7+ (pwsh.exe), active internet connection
    - Not tested, but this should work for Windows 10 as well.
    - pwsh.exe is currently hardcoded in the task scheduler settings, so modern powershell is a must.
    - This currently requires an internet connection and queries the public sunrise-sunset.org API. No API key required.

## About

A simple script that changes the Windows Theme **Mode** on sunset/sunrise to dark/light respectively. It does not change other personalization settings.

## Installation & Use

1. Move the `ps1` script where you want it to reside.
2. With an elevated terminal, run the script and pass your latitude and longitude: `.\windows-sunset-mode-switcher.ps1 -Lat 47.673 -Long -122.121`

The script will create a logon task and a primary task. The logon task is to update the instructions if they are old (for instance: a laptop that has been off for over a day). That said, both the sunset and sunrise tasks are set to "Daily" instead of "Once" to also attempt correction.

### Windows File Explorer Settings

This change is **HIGHLY RECOMMENDED**. Please read.

In order to enforce the change of mode to the taskbar and other desktop elements, this script restarts the _explorer.exe_ process that manages the desktop. Without doing so, the change to the theme mode will not apply to the task bar and some other desktop-related elements. I plan to look into how to cause this change without killing processes, but until then...

If the below setting is _not_ enabled, it will close all open file explorer windows as well, which is obviously undesired. (I would recommend this change regardless of whether you choose to use this script: if the desktop crashes, your open File Explorer windows likely won't).

1. Open a File Explorer window (Win+E) and click on the elipses in the top-right, click Options
2. Under the View tab, enable the 'Launch folder windows in a separate process' option and click OK to apply.

Note that after the change, you will need to restart the explorer.exe process. You can do so in PowerShell with the following command: `ps explorer | kill`. This will also close any open file explorer windows.

## Most Recent Update

### Version 2 - 2023-09-24

- Name change to reflect what is actually being managed by the script (Theme **mode**, not the theme itself)
- Major re-write that makes the script automatic
  - Creates and updates scheduled tasks on execution, no manual setup.
- No longer uses registry
- No longer needs to be run continuously to check state
  - Primary tasks are updated on each execution.

## Roadmap

- **Offline sunset calculation**
  - Either through my own functions or via an external library, it would be beneficial to have the option to calculate offline. This is a privacy feature in addition to a redundancy feature.
- **Graceful Mode Switching**
  - Need to find how to change the mode w/o killing explorer.exe.
- **Misc Improvements**
  - Checks to prevent unnecessary API calls.
  - (Better) correction detection
