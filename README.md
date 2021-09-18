# Creating A Pop!_OS Portable SSD Workstation for MacBook and Desktop

This is how to create a portable linux workstation using an external ssd. The key ability here is to be able to boot the distro on both your desktop at home/work and also with your MacBook. 

This installation is using Pop!_OS and this guide is directly for those wanting to install Pop!_OS on their external drive. I was unable to find one resource that covered the steps of getting the OS to work on an external ssd while being able to boot on my home desktop PC which natively runs windows, and also onto my MacBook for when I was at school.

If you don't want any explanations or  to do much reading, you can view the [alternative instructions](#alternative-instructions) instead.

## 
## Possible Issues To Avoid

#### Pop!_OS Successfully installed on external SSD, but not recognized / will not boot on MacBook

You need to run the installer from your MacBook in order for it to format the drive to boot with EFI. Do not use Windows or Linux, just do it from your MacBook. Follow the guide and it will boot on both. I spent hours trying to figure out how to make an existing Pop!_OS install run on Mac with no luck.

#### Can not install Pop!_OS onto the same drive as installer

Simple fix: just don’t put the installer on the external drive. Put it on a separate 4GB+ USB.

#### Selecting the wrong drive for install

Don’t do this, I did this. Make sure you know which name each drive has because making a guess can lead to permanent loss of files/your system.

#### Using Paid & Unpaid Software

You don’t need balenaEtcher, you don’t need Mac Linux USB Loader, no rEFInd custom boot loaders, nothing, there are no additional downloads needed.

## 
## What You Need
- External Drive (Hard/Solid State Preferred)
- 4GB USB Drive
- MacBook w/ OS X
- Pop!_OS LTS [.iso](https://pop.system76.com/) file

## 
## Installation

#### 1. Download
Download the latest LTS of Pop_OS [here](https://pop.system76.com/).

![Download Image](https://github.com/c-hebb/portable-linux-for-mac/blob/75048c65eb4bcc17a8c990f8dd9a298e4b614c45/images/download.png)

#### 2. Disk Utility: Formatting the drives
Plug in external drive and open Disk Utility. Erase the drive with the following settings:

Name: SSD

Type: MS-DOS FAT

Scheme: Master Boot Record

![Settings Image](https://github.com/c-hebb/portable-linux-for-mac/blob/75048c65eb4bcc17a8c990f8dd9a298e4b614c45/images/erase.png)

Then wipe it.
Unplug the external drive and plug in the USB drive.
Repeat with other drive.


#### 3. Convert .iso to .dmg
Now we need to convert the .iso into a .dmg and put it onto the USB drive so we can run the installer.

```bash
hdiutil convert /path/to/pop_os.iso -format UDRW -o /path/to/dmg
```

Now we will write it to the USB. So first lets find the USB using diskutil.

```bash
diskutil list
```

Take note of the disk number (diskX) as we will need to unmount our USB and load the dmg onto it. Mine was disk2.

![Terminal Image](https://github.com/c-hebb/portable-linux-for-mac/blob/75048c65eb4bcc17a8c990f8dd9a298e4b614c45/images/terminal.png)

```bash
diskutil unmountDisk /dev/diskX

sudo dd if=/path/to/pop.dmg of=/dev/diskX bs=1m
```

This may take a while, depending on how fast your USB drive is.


#### 4. Booting from the USB
Plug in the external drive.
Restart and hold down Option (Alt) as soon as the screen turns off. Keep spamming it. 

(I also recommend setting a firmware password so not anyone can plug in a USB and boot from it. Go [here](https://support.apple.com/en-us/HT204455) to do that.)

Select the EFI boot (orange drive) and boot it up. Let Pop!_OS load, click try/install, then let it do its thing. 

![Mac Startup Image](https://github.com/c-hebb/portable-linux-for-mac/blob/2db5a3d47400300a4a0c55dc253eb4e4f42a7989/images/startup.png)


#### 5. Installing Pop!_OS
Once the installer is open, go through it, do clean install > select the external ssd. Do not select anything that says apple and has the same space as your internal drive. Selecting the wrong drive will/could wipe your Mac OS and the idea here is to just make a portable Pop!_OS environment.

Also, don’t encrypt the drive. I am pretty sure I read something about encrypting it will make it not work the Mac, I don’t know, but I wasn’t going to risk it. You can look into that if interested.


#### 6. Booting on MacBook
Once successfully installed, restart the computer and hold down Option (Alt). You can remove the USB drive and it should only show one EFI boot option again. Select it and run it. You will now be booted into Pop!_OS and can start customizing it. 

(See [here](#post-installation) for a few things I did after installing)

You will now be able to:
- Boot automatically into OS X when the drive is not connected
- Boot into Pop!_OS by holding down Option (Alt) while booting.


#### Booting on Desktop
Now we need to be able to boot on our other computer, the desktop.
This one is pretty easy, just restart your computer and press your BIOS menu button. (Mine is clicking F2 as fast as I can when I see the motherboard logo). This will get you into your BIOS. Everyone will have a different bios, but ultimately you need to get to your boot options. Make sure secure boot or anything that doesn’t enable USB booting is off)

Mine looks like this:

![BIOS Image](https://github.com/c-hebb/portable-linux-for-mac/blob/2db5a3d47400300a4a0c55dc253eb4e4f42a7989/images/bios.png)

You want to look for a "UEFI: SSD NAME" boot option, not the "USB: SSD NAME" option. UEFI is the one that worked for me, but it doesn’t hurt to try both. It also helps to setup a Boot Manager if that is offered with your BIOS. Then set the boot priorities and you should be able to select which drive to boot on startup (Just like holding option on Mac) every time you boot, no key pressing needed. Alternatively, force boot into the "UEFI: SSD Name" to see if that works. It should, and now you have the Pop!_OS on your desktop.

You are now able to alternate between different computers keeping the same OS and file system. It would be a good idea to keep a cloud backup in case the drive is ever lost or damaged.


#### Notes
A few things to remember:
- Plug in > Boot MacBook > Hold Option (Alt) > EFI boot
- Plug in > Boot desktop > Select drive or boot into bios

You will also need to enable right-click using Gnome Tweaks and set your keyboard shortcuts.

#
## Alternative Instructions

Do the following on OS X:

1. Download [.iso](https://pop.system76.com/)
2. Insert external drive and USB drive, open Disk Utility>Select Drive>Erase

    MS-DOS, Master Boot Record

    Do for both drives.

3. Open Terminal, type the following:
```bash
    hdiutil convert /path/to/pop_os.iso -format UDRW -o /path/for/dmg
```
```bash
    diskutil list
```
Locate the disk# of the USB drive

![Tiny Terminal Image](https://github.com/c-hebb/portable-linux-for-mac/blob/75048c65eb4bcc17a8c990f8dd9a298e4b614c45/images/tinyterminal.png)

```bash
    diskutil unmountDisk /dev/diskX
    sudo dd if=/path/for/pop.dmg of=/dev/diskX bs=1m
```

4. Restart MacBook > Hold down Option (Alt) > Select EFI boot.
5. Run installer > Clean Install > External SSD > Don’t Encrypt
6. Restart > Unplug USB drive > Hold down Option (Alt) > Select EFI boot
7. On Desktop > Boot into BIOS > Use boot manager / Force boot > Select UEFI: SSD NAME
8. (Optional) [MacBook Pop_OS Post Installation]()

#
## Post Installation

- Update / Upgrade
```bash
    sudo apt update
    sudo apt full-upgrade
```
- Media Formats
```bash
	sudo apt-get install ubuntu-restricted-extras
	sudo apt install -y libavcodec-extra libdvd-pkg; sudo dpkg-reconfigure libdvd-pkg
```
- Apps
```bash
	sudo apt install snaps
	sudo apt install -y parted
	sudo apt install gnome-tweak-tool
	sudo apt install -y vlc
```
	   
Gnome tweak allows you to enable right click, weekday in menu bar, min/max buttons, and display battery %

- Enable firewall
```bash
	sudo ufw enable
```
- Enable TLP
```bash
	sudo apt install tlp tlp-rdw
	sudo systemctl enable tlp
```

- Dash Dock (Gnome Extension)

	Open Firefox or your browser, install gnome shell integration, and install Dash to Dock (For Dock)

   Dash to Dock: https://extensions.gnome.org/extension/307/dash-to-dock/

   Gnome Shell Integration: https://addons.mozilla.org/en-US/firefox/addon/gnome-shell-integration/

- Resources
	
   Making the Switch: https://support.system76.com/articles/switch-from-macos-to-popos

   Official Website: https://pop.system76.com/

   Alternate Wiki: https://pop-planet.info/


#
#
#
