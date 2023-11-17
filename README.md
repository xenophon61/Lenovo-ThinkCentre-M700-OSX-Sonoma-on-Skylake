# Lenovo-ThinkCentre-M700-OSX-Sonoma-on-Skylake
Opencore configuration to run OS X Sonoma on a (i5) ThinkCentre M700 Skylake SFF PC.
{under construction}

## Specifications:
- Lenovo ThinkCentre M700 model 10HY002WMG
- I5-6400t 1.2G (Skylake) with 8Gb RAM
- Intel® HD Graphics 530
- BIOS version 7/2022 [FWKTBFA](https://pcsupport.lenovo.com/us/en/products/desktops-and-all-in-ones/thinkcentre-m-series-desktops/thinkcentre-m700-tiny/10hy/downloads/ds105487-flash-bios-update-intel-b150-for-thinkcentre-m700-tiny-thinkcentre-m800-m900-m900x-tiny?category=BIOS%2FUEFI) 
- Has locked MSR2 (CFG Lock var offset = 0x197, and must be [unlocked](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#what-is-cfg-lock))
- boot disk is a LiteOn m2.SATA (attached to the m2 slot, which does not accept NVME drives)
- Intel wifi (with bluetooth)
- Display connected to the DP connector closest to the power supply (con2?)

## Brief guide:

- download the EFI.zip, expand it and update your serial number with GenSMBIOS (for a MacMini8,1), as per [Dortania](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html)
- note that the uncompressed EFI folder is for review only and is missing Tools and Resources (don't copy it to the EFI partition, instead use the expanded EFI.zip)
- should be bootable (OC 0.9.5 and up-to-date kexts)
- the individual non-compressed folders are provided for guidance (of particular significance is config.plist, which lists the DeviceProperties and NVRAM boot args, along with [Misc:Boot:Hibernation](https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/) setup)

## Miscellaneous (but important) notes:
This is a sweet little machine, took me a while to run Sonoma and then stumbled on the infamous wake-from-sleep issue with its HD 530 iGPU. No way I would accept a non-sleeping computer, so took it upon myself to solve the problem - essentially trying laptop-specific advice from OSXLatitude and insanelymac forum users. Seems it's working (fingers crossed). 
- WOL works, with the included Mausi driver (v1.0.8)
- use powerbutton to wake
- the USB ports were mapped with [CorpNewt's](https://github.com/corpnewt/USBMap) scripts under an iMac19,1 SMBIOS, and then edited to reflect the current MacMini8,1; IIRC the front USB ports only accept USB3.0 devices (i.e. no keyboard/mouses)
- Hackintool reports iGPU as "???", and I don't know the significance of this
- bluetooth is reported as present, but haven't bothered to check if it works or not (probably won't be reliable)
- the zipped ready-to-use EFI does not include Audio resources and other themes (to save space); please download if required

  
## Post-install:

Setup pmset as [follows](https://github.com/jkbuha/XPS-9500-IceLake-OpenCore/commit/705f534991a6458cc23eaea02799413531471470):

```
sudo pmset -a hibernatemode 25
sudo pmset -a standby 1
sudo pmset -a powernap 1
sudo pmset -a sleep 1
sudo pmset -a standbydelaylow 660
sudo pmset -a standbydelayhigh 660

devo@ThinkCentre ~ % pmset -g
System-wide power settings:
Currently in use:
 standby              1
 Sleep On Power Button 1
 womp                 1
 halfdim              1
 hibernatefile        /var/vm/sleepimage
 proximitywake        1
 powernap             1
 autorestart          0
 networkoversleep     0
 disksleep            10
 standbydelayhigh     660
 sleep                1
 hibernatemode        25
 ttyskeepawake        1
 displaysleep         5
 tcpkeepalive         1
 highstandbythreshold 50
 standbydelaylow      660
```
## Epilogue
Does it work? Well, I'll leave it on for as long as possible and report back (currently on day 1, it's November 13, 2023). So far so good!


With greetings from Athens,


[Xen](https://eplabmediterraneo.com)

## Credits:
- the usual (Apple, the Dortania / Opencore team, myriads of individual users)
- [Acidanthera](https://github.com/acidanthera) deserves a standing ovation!
- [insanelymac](https://www.insanelymac.com)
- [osxlatitude forums](https://osxlatitude.com)
- Dortania on how to unlock CFG: https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually
- this post applies to the HD 630, but was used as a template for the HD 530 on the ThinkCentre; genious approach! (https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/)https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/
- https://github.com/lshbluesky/OC-GA-B250M-DS3H-Hackintosh (refers to above insanelymac post)
- https://github.com/jkbuha/XPS-9500-IceLake-OpenCore
- https://github.com/jkbuha/XPS-9500-IceLake-OpenCore/commit/705f534991a6458cc23eaea02799413531471470
- https://github.com/acidanthera/bugtracker/issues/1810 (picked up the optimal standbydelaylow/high settings)
