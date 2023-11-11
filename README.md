# Lenovo-ThinkCentre-M700-OSX-Sonoma-on-Skylake
Configuration to run OS X Sonoma on a (i5) ThinkCentre M700 Skylake SFF PC.

Specifications:
I5-6400t 1.2G Skylake
Intel® HD Graphics 530
BIOS version 7/2022 FWKTBFA (DASBOOT) 
Has locked MSR2 (CFG Lock var offset = 0x197)

Brief guide:
- download the EFI.zip and update the serial number with GenSMBIOS
- should be bootable
- the individual folders are provided for guidance (the significant one is config.plist, which lists the DeviceProperties and NVRAM boot args)



Credits:
- the usual (Apple, the Opencore team, myriads of individual users)
- (https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/)https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/
