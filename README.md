# Lenovo-ThinkCentre-M700-OSX-Sonoma-on-Skylake
Opencore configuration to run OS X Sonoma on a (i5) ThinkCentre M700 Skylake SFF PC.

## Specifications:
- I5-6400t 1.2G Skylake
- Intel® HD Graphics 530
- BIOS version 7/2022 FWKTBFA 
- Has locked MSR2 (CFG Lock var offset = 0x197, and must be unlocked)
- Intel wifi (with Broadcom bluetooth)
- Display connected to the DP connector closest to the power supply (con2)

## Brief guide:

- download the EFI.zip and update the serial number with GenSMBIOS
- should be bootable (OC 0.9.5 and up-to-date kexts)
- the individual folders are provided for guidance (of particular significance is config.plist, which lists the DeviceProperties and NVRAM boot args, along with Misc:Boot:Hibernation)

## Post-install:

Setup pmset as follows:

´sudo pmset -a hibernatemode 25


sudo pmset -a standby 1


sudo pmset -a powernap 1

sudo pmset -a sleep 1

sudo pmset -a standbydelaylow 1

sudo pmset -a standbydelayhigh 1´



## Credits:
- the usual (Apple, the Opencore team, myriads of individual users)
- Dortania on how to unlock CFG: https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually
- (https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/)https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/
- https://github.com/jkbuha/XPS-9500-IceLake-OpenCore
