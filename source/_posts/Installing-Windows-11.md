---
title: Installing Windows 11
date: 2025-10-18 12:55:24
tags:
---

Microsoft broke several ways of upgrading from 10 to 11 just in time to EOL Windows 10, and sometimes you don't have or want to use TPM or a Microsoft account.
I don't know how to _keep your files_ while installing Windows 11, but if you still want to install 11, here's what I did:

<!-- more -->

1. Back up your files; this will wipe your HDD/SDD/M2 drive
1. Back. Up. Your. Files.
1. If you have multiple drives, you can move Steam & GOG games to the secondary drive for basically instant importing them back into Steam or GOG Galaxy.
1. Download the Windows 11 .iso file from Microsoft [here](<https://www.microsoft.com/en-us/software-download/windows11>)
1. Download [Rufus](<https://rufus.ie/en/>) to write the .iso to a USB flash drive
1. Plug a USB flash drive into your computer
1. Run Rufus, select the USB flash drive and the Windows 11 .iso file
1. You can set various configurations here
1. Start the process, wait for it to complete, and eject the USB flash drive
1. Shut down your computer
1. Unplug any _additional_ drives you have connected, like `D:\`, `E:\`, etc.
1. With the USB flash drive still plugged in, boot your computer, and press whatever key your BIOS tells you to push to interrupt the boot or change settings or whatever. If you make it to the Windows 10 login screen, you did it wrong.
1. In your BIOS, make sure UEFI Safe Boot is enabled (depends on your motherboard)
1. Boot from the USB drive (depends on your motherboard)
1. Go through the process to install Windows 11
1. Click "I don't have a product key" when prompted to enter one
1. If you need to repartition your `C:\` drive, I deleted all the partitions, leaving the whole drive as "empty" or "available" or whatever, and selected that space and pressed "Next"
1. Follow the rest of the process. Your computer will reboot several times.
1. When you get into Windows 11 actual, you can remove the USB flash drive
1. Run Windows Update. This will take a long time. Meanwhile ...
1. Go through and shut off all the crap that Microsoft wants you to sync/share/etc.
1. Uninstall all the crap that Microsoft thinks you need but you don't
1. Wait for updates to complete; reboot a few times
1. To bypass the account login, follow the instructions [here](<https://github.com/massgravel/Microsoft-Activation-Scripts>)
1. Reboot after doing that
1. I've found [Windhawk](<https://windhawk.net/>) to be great for un-fucking the start menu, and [TranslucentTB](<https://github.com/TranslucentTB/TranslucentTB>) to be nice for visuals. I always use [Nanite](<https://ninite.com/>) to install regular programs I'll need.
1. Shut down, plug in any additional drive(s), and boot back up. Windows should recognize them. Download Steam, GOG Galaxy, etc. and locate your installed games on those spare drives.
