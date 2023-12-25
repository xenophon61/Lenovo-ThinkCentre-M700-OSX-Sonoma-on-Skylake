# Lenovo-ThinkCentre-M700-OSX-Sonoma-on-Skylake
Opencore configuration to run OS X Sonoma on a (i5) ThinkCentre M700 Skylake SFF PC.
{under construction}. Some information is in the Wiki.

## Specifications:
- Lenovo ThinkCentre M700 model 10HY002WMG
- I5-6400t 1.2G (Skylake) with 8Gb RAM
- Intel® HD Graphics 530
- BIOS version 7/2022 [FWKTBFA](https://pcsupport.lenovo.com/us/en/products/desktops-and-all-in-ones/thinkcentre-m-series-desktops/thinkcentre-m700-tiny/10hy/downloads/ds105487-flash-bios-update-intel-b150-for-thinkcentre-m700-tiny-thinkcentre-m800-m900-m900x-tiny?category=BIOS%2FUEFI) 
- Has locked MSR2 (CFG Lock var offset = 0x197, and must be [unlocked](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#what-is-cfg-lock)); you can use the local [Wiki](https://github.com/xenophon61/Lenovo-ThinkCentre-M700-OSX-Sonoma-on-Skylake/wiki) for guidance
- boot disk is a LiteOn m2.SATA (attached to the m2 slot, which does not accept NVME drives)
- Intel wifi (with bluetooth)
- Display connected to the DP connector closest to the power supply (con2?)

## Brief guide:

- enable CSM in BIOS, as per the [Wiki](https://github.com/xenophon61/Lenovo-ThinkCentre-M700-OSX-Sonoma-on-Skylake/wiki); now is the time to turn CFG Lock off, and enable XCHI (again, check the Wiki)
- download the EFI.zip, expand it and update your serial number with GenSMBIOS (for an iMac19,1), as per [Dortania](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html)
- note that the Files section above includes only a subset, for review purposes. Don't copy it to the EFI partition, instead use the expanded EFI.zip
- should be bootable (OC 0.9.5 and up-to-date kexts)
- note: HibernationFixup and Lilu are debug versions
- the individual non-compressed folders are provided for guidance (of particular significance is config.plist, which lists the DeviceProperties and NVRAM boot args, along with [Misc:Boot:Hibernation](https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/) setup)

## Miscellaneous (but important) notes:
This is a sweet little machine, took me a while to run Sonoma and then stumbled on the infamous wake-from-sleep issue with its HD 530 iGPU. No way I would accept a non-sleeping computer, so took it upon myself to solve the problem - essentially trying laptop-specific advice from OSXLatitude and insanelymac forum users. Seems it's working (fingers crossed). 
- ~~WOL works, with the included Mausi driver (v1.0.8)~~
- use powerbutton to wake
- the USB ports were initially mapped with [CorpNewt's](https://github.com/corpnewt/USBMap) scripts under an iMac19,1 SMBIOS
- however, the final map(*) was obtained by booting Windows 10 and then running [USBToolBox/tool](https://github.com/USBToolBox/tool); of note it detected the Intel Bluetooth adapter and assigned it to an "internal" port, but not really identifying it
- Hackintool reports iGPU as "???", and I don't know the significance of this; it works fine
- Bluetooth is reported as present, but haven't bothered to check if it works or not (probably won't be reliable); the IntelBT injectors are disabled in the config.plist, but BlueToolFixup is required (otherwise you get an annoying dialog at startup)
- these are the current boot arguments(*)
```
  boot-args	alcid=21 igfxagdc=0  revpatch=sbvmm forceRenderStandby=0 igfxonln=1 keepsyms=1 -amfipassbeta swd_panic=1 hbfx-ahbm=1 darkwake=0 -noDC9  -liludbgall -hbfxdbg -btlfxallowanyaddr
```
- the SSDT-USBW.aml is created as per [Dortania](https://dortania.github.io/OpenCore-Post-Install/usb/misc/keyboard.html#method-1-add-wake-type-property-recommended) but unused; same for the stock Dortania SSDT-USBX.aml
- final important note: AppleALC is disabled (must use a USB sound adapter)(*)

Solving the display wake issue required a lot of Voodoo; the items marked with an asterisk probably played a major role, and I just don't have the resources to figure out which exactly is critical or not. I plan to gradually introduce them and see (AppleALC.kext in particular).

  
## Post-install:

- Setup pmset as [follows](https://github.com/jkbuha/XPS-9500-IceLake-OpenCore/commit/705f534991a6458cc23eaea02799413531471470):

```
sudo pmset restoredefaults
sudo pmset -a hibernatemode 25
sudo pmset -a standby 1
sudo pmset -a powernap 0
sudo pmset -a sleep 1
sudo pmset -a standbydelaylow 610
sudo pmset -a standbydelayhigh 610

System-wide power settings:
Currently in use:
 standby              1
 Sleep On Power Button 1
 womp                 0
 autorestart          0
 hibernatefile        /var/vm/sleepimage
 proximitywake        0
 powernap             0
 networkoversleep     0
 disksleep            10
 standbydelayhigh     610
 sleep                1
 hibernatemode        25
 ttyskeepawake        1
 displaysleep         10
 tcpkeepalive         1
 highstandbythreshold 50
 standbydelaylow      610
```
- disable RTC wakes; This has been described very eloquently by [JayMonkey](https://www.tonymacx86.com/threads/solved-ventura-random-scheduled-pm-wake-from-sleep.323359/post-2387982), and involves some steps to create a new powerd.plist.

## Epilogue
Does it work? Well, I'll leave it on for as long as possible and report back (currently December 9, 2023). So far so good!

25-Dec-23 update: new setup, slightly changed from above, has slept and was awakened (using the power button) n=10 times in the past couple of days. Stay posted for an updated EFI folder in a week or two.

With greetings from Athens,

[Xen](https://eplabmediterraneo.com)

## Debugging
Here's a small collection of commands to view the debug logs, gathered from the internet:
```
pmset -g log | egrep "\b(Sleep|Wake|DarkWake|Start)\s{2,}"
sudo dmesg | grep HBFX
log show --predicate 'process == "kernel"' --style syslog --source --debug --last boot
sudo log show --predicate 'process == "kernel"' --style syslog --source --debug | grep hibern
```

## Credits:
- the usual (Apple, the Dortania / Opencore team, myriads of individual users)
- [Acidanthera](https://github.com/acidanthera) deserves a standing ovation!
- [insanelymac](https://www.insanelymac.com)
- [osxlatitude forums](https://osxlatitude.com)
- Dortania on how to unlock CFG: https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually
- this post applies to the HD 630, but was used as a template for the HD 530 on the ThinkCentre; genious approach! (https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/)https://www.insanelymac.com/forum/topic/355419-90-solved-hibernatemode-25-successfully-working-on-intel-hd-630-igpu-but-glitch-issues-on-first-wake-screen/
- https://github.com/lshbluesky/OC-GA-B250M-DS3H-Hackintosh (same author as the above insanelymac post)
- https://github.com/jkbuha/XPS-9500-IceLake-OpenCore
- https://github.com/jkbuha/XPS-9500-IceLake-OpenCore/commit/705f534991a6458cc23eaea02799413531471470
- https://github.com/acidanthera/bugtracker/issues/1810 (picked up the optimal standbydelaylow/high settings)
