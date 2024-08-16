---
layout: post
title:  "Recovering From Malfunctioning Windows PIN When Passwordless Login Is Enabled"
#date:   2024-08-16 00:00:00 -0800
categories: 
---

Yesterday after installing the latest Windows Update patches, I found myself unable to login on my laptop.  The error was

> "Something went wrong and your PIN isn't available (code: 0xd0000225).  Restart your device to see if that fixes the problem."

Because I have enabled passwordless login on the device, the only login options available were PIN and Windows Hello face recognition (which relied upon the PIN as well).

I was locked out.

## Have you tried turning it off and on?

The first thing I did was to try a reboot obviously, though to to avail.  Next I tried booting to Safe Mode, but that also only offered the same login types.

I found some similar posts that suggested removing the stored PIN settings.  To do this, I rebooted into the Advanced Startup menu and selected the option to use the Command Prompt.  The settings are located in `C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\Ngc`, so I renamed that folder (a reversible operation if necessary, rather than deleting).    This also required providing the BitLocker keys to unlock my system drive.  (The BitLocker keys were stored in my Microsoft account, so I could look them up via my phone.)  This didn't resolve the issue, but it did change the error message - understandably, since the only login option was still to use a PIN but now there was no stored PIN data.

The next troubleshooting tip I found suggested uninstalling the latest update.  This is also possible to do through the Advanced Startup menu.  It didn't take too long, but the issue remained (note: I had not attempted to restore the renamed `Ngc` folder from the previous step).

## How I got lucky 

As I was rebooting yet again, I noticed that the Power menu options had changed to include the "Update and Restart" option again - i.e. Windows had detected that the update I'd removed was again available.  Since I was still in a broken state after uninstalling the update, I figured why not put it back?  So I installed the update again.

Upon rebooting, when I tried to sign in with the PIN, this time I saw a different experience: Windows asked me to verify using the Authenticator app on my phone, and then allowed me to set a new PIN.  This resolved the issue for me.  I had to reset Windows Hello to detect my face again, I assume because that depended on the data that had been moved.

## What I was about to do before it got fixed

Right before I stumbled into this mitigation, I found how to toggle the Passwordless Login feature in Windows.  Lots of guides point to how to configure this via the Settings app, but that obviously requires you to be logged in first.  However, the setting is stored in a registry key, so you should be able to set it via the Command Prompt if necessary.

The registry key is: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device`.  Set the value of `DevicePasswordLessBuildVersion` to `0` to disable passwordless login, or set it to `2` to enable passwordless login.

I hope nobody else encounters this issue, but from the other posts I found relating to this error, it does seem to occur around update releases.  Hopefully these notes help anyone who does run into it.
