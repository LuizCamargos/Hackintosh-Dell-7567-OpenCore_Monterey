# Personal Hackintosh Guide/Instructions for Dell 7567

### Screenshot
![macOS Big Sur - 11.0.1](./tools/ScreenShot.png)

### Download Required Files
Check [releases]()


### Notes
 - HDMI doesn't work on this hack because it is connected to nvidia card and we disabled it
 - Any other issues? Just open an issue in this repo!

### Known Bugs
 - 2.1 audio (2.0 works)
 - Bluethooth not workin with some devices (in my case my wireless mi mouse not working)

### Specs
 - Intel i7-7700HQ CPU
 - Intel HD Graphics 630 / nVidia GTX 1050 Ti
 - 16GB 2400MHz DDR4 RAM
 - 15.6” 1080p IPS Display
 - 500GB Samsung 970 Evo Plus SSD (Nvme)
 - 1TB 5400RPM Toshiba HDD

### Prerequisites
 - Set BIOS options properly:
   - Disable Legacy Option ROMs
   - Change SATA operation to AHCI (If already using windows, google how to)
   - Disable Secure Boot

## Creating macOS USB With MacOS (you can create usb installer in windows just google it)

- Download MacOs Big Sur from app store and run this code in terminal

```bash
# Figure out identifier for of your usb (/dev/diskX, for example)
diskutil list

# Partition your usb in GPT
diskutil partitionDisk /dev/diskX 1 GPT HFS+J "install_osx" R

# Copy installer imge 
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/install_osx

# Rename 
sudo diskutil rename "Install macOS Big Sur" install_osx
```
- Download [Clover Configrator](https://mackie100projects.altervista.org/download-clover-configurator/)
- Use the app to mount USB EFI partition
- Copy EFI folder from OpenCore USB File to Efi partition


## Booting USB and Installing macOS

  - Press F12 repeatedly for one-time boot-menu and select your usb (if your usb not appear on boot menu you must add manualy from bios)
  - Choose *install_osx* in OpenCore
  - Open *diskutility* and format the partition into apfs and label the partition (eg. macOS)
  - Continue to install
  - System now automatically reboots, boot again into clover, but now select 'Install macOS Big Sur' instead of 'install_osx'
  

## Post Installation

- Use Clover Configrator to mount apfs EFI partition
- Copy EFI folder from OpenCore Post-install File to Efi partition


**Configure ComboJack for Headphone Jack**
  - open terminal type *sodu* then drag install.sh from tools/comboJack_Installer then press 'Enter'
  - Rebuild the cache using `sudo kextcache -i /`
  - Reboot  

**Disable Hibernation**
```bash
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
sudo pmset -a powernap 0
```

**4) Scrolling is choppy with third party mice**
 - Use this tool: https://mos.caldis.me/, works well
 - Scroll direction can also be set via this tool


## Credits
 - [Lersy](https://github.com/lersy) and [Me](https://www.github.com/maxis7567/) *(For putting this together & adding many required patches)*
