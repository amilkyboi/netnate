---
draft: false
title: Arch Linux
description: A simple installation of Arch from scratch with some basic tools.

tags: [computing]

date: 2023-08-02

ShowToc: true
TocOpen: true
---

## 1 Pre-Installation

### 1.1 Install a torrent client

Download a torrent client like [qBittorrent](https://www.qbittorrent.org/download). On the same page, open the Checksums and Library Versions tab and copy the SHA-256 checksum for the 64-bit installer. Open PowerShell, navigate to the Downloads folder by typing `cd C:\Users\user\Downloads` and then type `(Get-FileHash .\qbittorrent_4.5.2_x64_setup.exe).Hash -eq "f2ec7fa4c5ae273d6d7181c0c9df225eb8ce8e0e85577b236c7b335c093f2e71"`. If the prompt returns `True`, the downloaded file can be installed. Alternatively, install qBittorrent using winget by typing `winget install qBittorrent.qBittorrent` in a PowerShell terminal.

### 1.2 Install GPG

Install GNU Privacy Guard (GPG) via winget in PowerShell by typing `winget install GnuPG.GnuPG`. GPG can now be called via the `gpg` command in the terminal.

### 1.3 Download the Arch Linux ISO

Visit the Arch Wiki [downloads](https://archlinux.org/download/) page, click the magnet link for the latest version of Arch Linux, and torrent the file via qBittorrent. After the file has been downloaded, visit the [checksums](https://archlinux.org/download/#checksums) page and download the ISO PGP Signature. Navigate to the Downloads folder by typing `cd C:\Users\user\Downloads` and type `gpg --verify .\archlinux-2023.05.03-x86_64.iso.sig .\archlinux-2023.05.03-x86_64.iso` to verify the Arch Linux ISO file using the appropriate signature. GPG should  return `Good Signature`. The User ID might not be certified, but this is fine. To be extra cautious, visit the [developers](https://archlinux.org/people/developers/) page and match the primary key fingerprint output by GPG to that shown for the matching developer.

### 1.4 Install Rufus
BIOS
Download Rufus, a bootable storage device creation tool, by visiting the [website](https://rufus.ie/en/). The executable is automatically checked for a SHA-256 signature by Windows. Right-click the executable and navigate to the Digital Signatures tab. The signed name should be ‘Akeo Consulting’. Alternatively, install Rufus using winget by typing `winget install Rufus.Rufus` in a PowerShell terminal.

### 1.5 Prepare the installation medium

Plug in portrable drive into a USB port and launch Rufus (it will automatically default to Administrator privileges). Select the device and click SELECT to find the Arch Linux ISO downloaded earlier. Change the partition scheme to GPT and the target system to UEFI (non CSM). Name the volume something obvious and leave the file system as Large FAT32 with a cluster size of 32 kB. This step is important as the live bootable will not install if NTFS is used instead. Click START to write the ISO to the removable drive. If a pop-up is encountered, continue with writing the disk in ISO image mode. Rufus can be closed when the status bar reads READY. Eject the removable drive and physically remove it.

### 1.6 Prepare the host machine

Boot to the BIOS settings on the target machine (typically by hitting F11 or Enter on some machines during startup). Enter the security tab and ensure that that Secure Boot is disabled. Secure Boot will prevent booting from an external device if this step is not performed. While in the BIOS, hyper-threading and CPU virtualization should also be enabled in the config and security tabs, respectively. Save the changed settings and exit the BIOS.

### 1.7 Boot to the live environment

Plug the removable drive into the target machine and boot it up. Enter the startup interrupt menu via pressing F11 or Enter and proceed to the boot menu. Select the removable drive as the boot device and press Enter. Arriving at the GNU GRUB menu, select the first option ‘Arch Linux install medium (x86_64, UEFI)’ and wait for the live environment to load the Zsh tty `root@archiso ~ #`.

### 1.8 Set the console keyboard layout and font

The default console keymap is US and does not need to be changed. The font will also remain unchanged. Neither requires any commands to be run.

### 1.9 Verify the boot mode as UEFI

Type `ls /sys/firmware/efi/efivars` into the terminal. If the resulting directory is returned without error, the system is booted properly in UEFI mode.

### 1.10 Connect to the internet

Ensure that the network interface is listed and enabled by typing `ip link`. Some number of network devices should appear, the amount and type dependent upon the current machine. To connect to a network via ethernet, just plug the cable directly into the machine. For wireless connectivity, first type `iwctl`. Next, type `device list`. Here, something like `wlan0` should appear. Ensure that the Powered status of the device reads ‘on’. If not, type `device *device* set-property Powered on`. Next, still in the `iwctl` prompt, type `adapter list` and ensure that the adapter is also Powered on. If not, type `adapter *adapter* set-property Powered on`. Using the device name identified earlier, type `station *device* scan` to begin the scan for networks within range. Connect to the appropriate network with the command `station *device* connect *SSID`* and enter the password for the network. Type `exit` to exit from the `iwd` prompt. Test that the network has been properly connected to by running `ping [archlinux.org](http://archlinux.org)` and waiting for return bytes.

### 1.11 Update the system clock

Type `timedatectl` and read the resulting output. For now, the time zone should be UTC, the system clock should be syncronized, and network time protocol (NTP) should be active. If NTP reads inactive, type `timedatectl set-ntp true`.

### 1.12 Partition the disks

#### Removing old partitions

Type `fdisk -l` to view the attached disks. Find the designation for the HDD or SSD that Arch Linux will be installed on (most likely `/dev/sda`). Type `fdisk /dev/sda` to enter the `fdisk` utility interface. Next, type `p` to view the partitions on the selected disk. If there are any preexisting partions, they need to be deleted before going forward. Obviously, if any necessary data is remaining from a previous installation of Linux or Windows, exit `fdisk` immediately by pressing `q`, reboot the machine, and backup the data. Otherwise, remove the partitions one-by-one by typing the command `d` into the `fdisk` prompt until all partitions are removed.

#### Creating EFI boot partition

Still in `fdisk`, type `n` to create a new partition. Type `1` for the partition number, hit Enter for the first sector, type `+512M` for the last sector, and `Y` if prompted to remove the previous signature. Next, the partition type needs to be changed to UEFI. Type `t` to change the partition type and enter `uefi` as the alias. The partition type GUID for UEFI is `C12A7328-F81F-11D2-BA4B-00A0C93EC93B`. Type `p` to view the partition table and ensure `/dev/sda1` was created properly as a UEFI partition with type EFI System.

#### Creating root partition

Type `n` to create another new partition. Partition number `2`, Enter for first sector, `+30G` for last sector, and `Y` if prompted. Type `t`, followed by `2` and `4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709` to change the type to Linux x86-64 root. (This step is not strictly necessary, but this ensures that the filesystem is explicitly in the x86_64 format.)

#### Creating home partition

Again, type `n`, followed by `3` and then Enter twice. Type `p` to view the final partition table. Ensure that partition 1 reads Size 512M EFI Filesystem, 2 reads 30G Linux filesystem, and 3 reads remaining data Linux filesystem. Type `w` inside `fdisk` to write the changes to the disk.

### 1.13 Format the partitions

Enter the following three commands in order: `mkfs.fat -F32 /dev/sda1`, `mkfs.ext4 /dev/sda2`, `mkfs.ext4 /dev/sda3`.

### 1.14 Mount the filesystems

First, the root directory will be mounted by typing `mount /dev/sda2 /mnt`. Next, create a home directory by typing `mkdir /mnt/home`. Now mount the home partition into this directory by typing `mount /dev/sda3 /mnt/home`. Finally, create an EFI directory where the boot partition will be mounted using `mkdir /mnt/boot` followed by `mkdir /mnt/boot/efi`. Mount the boot partition using `mount /dev/sda1 /mnt/boot/efi`.

## 2 Installation

### 2.1 Changing download mirrors

Using the command `nano /etc/pacman.d/mirrorlist` will open the mirror file where different servers can be selected to download from. These can be changed later on.

### 2.2 Installing essential packages

Before installing, type `nano /etc/pacman.conf` to enter the `pacman` configuration file. Scroll down to the line `#ParallelDownloads = 5` and uncomment it to allow `pacman` to download files in parallel. This will speed up the installation process. Next, type `pacstrap -K /mnt base base-devel linux linux-firmware nano`. The `base-devel` package installs `sudo`, which will be needed later on. The `-K` flag initializes a new pacman keyring.

## 3 Configure the system

### 3.1 Fstab

Generate an fstab file using `genfstab -U /mnt >> /mnt/etc/fstab` and check the integrity using `nano /mnt/etc/fstab`. Partition `sda2` should read `/` and `ext4`. Partition `sda3` should read `/home` and `ext4`. Partition `sda1` should read `/boot/efi` and `vfat`.

### 3.2 Chroot

Change root into the newly created filesystem by typing `arch-chroot /mnt`.

### 3.3 Time zone

Set the time zone using `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime`. The hardware clock is then set using `hwclock --systohc`.

### 3.4 Localization

Type `nano /etc/locale.gen` and scroll down to `#en_US.UTF-8 UTF-8`. Uncomment this line. Save the file and run `locale-gen`. Type `nano /etc/locale.conf` and add the line `LANG=en_US.UTF-8`. The keyboard was unchanged during the first few steps of installation so no further changes are needed here.

### 3.5 Hostname

To set a hostname for the machine, type `nano /etc/hostname` and write a line like `archpad`. Then enter `nano /etc/hosts` and write the following lines (using tabs for alignment only):

```txt
127.0.0.1   localhost
::1         localhost
127.0.1.1   Archlinux.localdomain   Archlinux
```

### 3.6 Network management

Type `pacman -S networkmanager` and then `systemctl enable NetworkManager` once it has been installed. Three `symlink` lines should appear if done correctly.

### 3.7 Setting a root password

Type `passwd` and enter something that can be remembered.

### 3.8 Installing the GRUB boot loader

Enter `pacman -S grub efibootmgr` and let them install. Then, type `grub-install --target=x86_64-efi --efi-directory=/boot/**efi** --bootloader-id=grub_uefi --recheck`. The installation should return with no errors. To create the necessary configuration file, type `grub-mkconfig -o /boot/grub/grub.cfg`.

### 3.9 Installing and activating microcode

Type `pacman -S intel-ucode` and install it. After installing, regenerate the GRUB configuration file to activate loading the microcode update by running `grub-mkconfig -o /boot/grub/grub.cfg`. It should find `intel-ucode.img` in the fallback `initrd` images.

### 3.10 Creating swap filesystem

- `fallocate -l 2G /swapfile`
- `chmod 600 /swapfile`
- `mkswap /swapfile`
- `echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab`

### 3.11 Reboot

Type `exit` followed by `umount -R /mnt` to unmount all partitions manually and to detect any errors. Reboot with `reboot`. Remove the live installation media on reboot. A GNU GRUB GUI should appear with Arch Linux as the first option.

## 4 Post-Installation

### 4.0 Pacman interlude

Enable colors and parallel downloads in pacman by editing the config file at `sudo nvim /etc/pacman.conf` and uncommenting the two lines in misc options below:

```bash
Color
ParallelDownloads = 5
```

### 4.1 Connecting to the internet via NetworkManager

Type `nmcli` to access the NetworkManager command line interface. To view the local accessible WiFi networks, type

```bash
nmcli device wifi list
```

A list of networks should appear, each with a SSID, rate, signal strength, and security. To connect to a network, type

```bash
nmcli device wifi connect SSID password PASSWD
```

where SSID is the name of the network and PASSWD is the password. After a while, the respective WiFi device should be activated and connected to the network. Test the connectivity using `ping archlinux.org`.

Alternatively, the NetworkManager GUI can be used to connect to a wireless network instead. Simply type `nmtui` and activate a network.

### 4.2 Updating the system and installing Neovim

Check for system updates by running `sudo pacman -Syu`. Install any necessary updates. Install Neovim by typing `sudo pacman -S neovim`.

### 4.3 Enabling sudo and adding a user

First, edit the `visudo` file to enable the wheel group by typing `sudo EDITOR=nvim visudo`. Scroll down to the wheel section below and uncomment the line so that it looks like the following. Save the file using `:wq`.

```bash
%wheel ALL=(ALL:ALL) ALL
```

Add a user by typing the following line. The `-G` flag adds the user to the following groups and the `-d` flag assigns the user to the following directory.

```bash
sudo useradd -G wheel -d /home/user -m user
```

Give the user a password by typing

```bash
sudo passwd user
```

and then typing in a password twice. Now type `exit` to log out of root and log into the user account. Ensure that `sudo` works by typing something like `sudo pacman -Syu`.

If any problems are encountered establishing the new user, the user’s account and home directory can be deleted using the following command.

```bash
sudo userdel -r user
```

### 4.4 Yay and getting git

To install packages easily from the Arch User Respository (AUR), the Yet Another Yogurt (YAY) package manager is commonly used. First, git needs to be installed.

```bash
sudo pacman -S git
```

To install yay by downloading it from git, run the following commands in the home directory.

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

After the package has been installed, delete the folder that was created on download using `rm -rf /yay`. **Be careful using `rm -rf`**. Yay is used in much the same way as pacman. To install a package:

```bash
yay -S <package>
```

to remove a package and its dependencies:

```bash
yay -Rns <package>
```

and to update all packages:

```bash
yay -Syu
```

### 4.5 Mirror configuration

Visit the [mirrors](https://archlinux.org/mirrorlist/) webpage and select your country with https and IPv4 protocols. Generate the list, copy it, and open up a terminal windows. Type `nano /etc/pacman.d/mirrorlist` and paste the generated list there. The [Reflector](https://wiki.archlinux.org/title/Reflector) Python script can grab the most up-to-date mirrors and overwrite the `mirrorlist` file automatically.

## 5 Installing a window manager

### 5.1 Installing the i3-wm window manager and status

```bash
sudo pacman -S i3-wm
```

Here's a [link](https://www.reddit.com/r/i3wm/wiki/faq/i3i3bari3status/) to a wiki explaining the differences between i3, i3bar and i3status.

### 5.2 Installing Xorg

Xorg itself is installed with the `xorg-server` package. The xinit package allows for Xorg to be started manually for use with window managers.

```bash
sudo pacman -S xorg-server xorg-xinit
```

Next, identify the graphics card being used by the system. For systems with integrated and discrete graphics cards, there will be two outputs: one labeled the VGA controller and one labeled the 3D controller. The integrated graphics card can be used.

```bash
lspci -v | grep -A1 -e VGA -e 3D
```

Search for open-source video drivers using

```bash
pacman -Ss xf86-video
```

and install the correct driver. For systems with Intel integrated graphics, type the following to install the mesa driver. **Don’t use the xf86-video-intel driver**, it is made for older CPUs. Also install the `vulkan-intel` package for Ivy-Bridge and later CPUs.

```bash
sudo pacman -S mesa vulkan-intel
```

### 5.3 Editing xinitrc configuration

Type the following to copy the xinit configuration file to the home directory of the current user.

```bash
cp /etc/X11/xinit/xinitrc ~/.xinitrc
```

Open the newly copied file `nvim ~/.xinitrc` and comment out or delete the following lines at the bottom of the file.

```bash
# twm &
# exec xterm ....
```

Add the following line instead:

```bash
exec i3
```

### 5.4 Starting i3 and generating a config

Start i3 by typing either `startx` or `xinit` in the tty. When launched, press Enter to generate the config file in `~/.config/i3/config` and use Win as the modifier key. Some useful keybinds are:

```bash
$mod+Return - opens a terminal window
$mod+Return+3 - exits both the i3 and xorg sessions
```

Since a terminal emulator has not been installed yet, `$mod+Return` will not work. Exit the i3 session and return to the tty.

## 6 Audio

### 6.1 PipeWire

PipeWire is an audio driver and is generally more well-regarded than something like pulseaudio. To install it, type `sudo pacman -S pipewire`. Next, install the WirePlumber session manager by typing `sudo pacman -S wireplumber`. Finally, install `sudo pacman -S pavucontrol`.

### 6.2 WirePlumber and hardware audio control

Edit the i3 config by typing `nvim .config/i3/config`. Comment out the block of code talking about PulseAudio volume control. Add the following lines

```bash
set $refresh_i3status killall -SIGUSR1 i3status
bindsym XF86AudioRaiseVolume exec --no-startup-id wpctl set-volume -l 1.5 @DEFAULT_AUDIO_SINK@ 5%+ && refresh_i3status
bindsym XF86AudioLowerVolume exec --no-startup-id wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%- && refresh_i3status
bindsym XF86AudioMute exec --no-startup-id wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle && refresh_i3status
bindsym XF86AudioMicMute exec --no-startup-id wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle && refresh_i3status
```

## 7 Brightness control

Install brightnessctl `sudo pacman -S brightnessctl`. Go into `nvim .config/i3/config` and add

```bash
bindsym XF86MonBrightnessUp exec --no-startup-id brightnessctl set 5%+
bindsym XF86MonBrightnessDown exec --no-startup-id brightnessctl set 5%-
```

somewhere with a comment for explanation. This package adds ********hardware******** brightness control. Things like xrandr only adjust the gamma value of the screen, which is not true backlight control.

## 7 Basic programs

### 7.1 Web browser

Install Firefox using `sudo pacman -S firefox`. Since we are using PipeWire, select the package `pipewire-jack` when asked to pick a jack provider. For the session manager, `wireplumber` should already be installed from the PipeWire installation step earlier.

## 8 Customization

### 8.0 Reversing touchpad scroll direction

We’re going to be using `libinput` for this step. First, identify that libinput is being used as the input driver for the touchpad by typing

```bash
grep -e "Using input driver 'libinput'" ~/.local/share/xorg/Xorg.0.log
```

There should be a multi-line output of devices that libinput controls, including the touchpad (something like Synaptics TM3276-031).

In general, libinput config files are located in `/usr/share/X11/xorg.conf.d/40-libinput.conf`. Custom configuration files should be placed in `/etc/X11/xorg.conf.d/` and following a widely used naming schema; `30-touchpad.conf` is often chosen as filename. Create this file using `nvim 30-touchpad.conf` and add the following lines

```bash
Section "InputClass"
    Identifier "Synaptics TM3276-031"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "ClickMethod" "clickfinger"
    Option "NaturalScrolling" "true"
EndSection
```

Here, the Tapping option enables tap-to-click and ClickMethod disables the trackpad middle and right-click areas. Instead, a two-finger click is used for context menus and a three-finger click is used for MMB. Restart i3 and xorg to see the changes take effect.

### 8.1 Intalling rofi for searching

Install rofi using `sudo pacman -S rofi`. Allow rofi to be launched via `$mod+d` using `nvim .config/i3/config` and changing

```bash
bindsym $mod+d exec --no-startup-id dmenu_run
```

to

```bash
bindsym $mod+d exec --no-startup-id rofi -show drun
```

### 8.2 Polybar

Install polybar using `sudo pacman -S polybar`. Edit the i3 config file using `vim ~/.config/i3/config` and add the following line

```bash
exec --no-startup-id polybar
```

somewhere in the file; after nm-applet works well enough.

While we’re in the i3 config, we can also disable title bars and add gaps. After the font line at the beginning of the file, add the following lines.

```bash
for_window [class=".*"] border pixel 1
gaps inner 10
gaps outer 5
```

### 8.3 Installing a terminal emulator

For this tutorial, we will be installing the [Kitty](https://github.com/kovidgoyal/kitty) terminal emulator. Type `sudo pacman -S kitty` and intall it. i3 uses the i3-sensible-terminal as the default terminal emulator. To change this, go into the i3 config using

```bash
nvim ~/.config/i3/config
```

and change the terminal launch line to

```bash
bindsym $mod+Return exec kitty
```

Ensure that kitty is being used in i3 by typing `echo $TERM` in the terminal window.

### 8.4 Installing fish

Instead of bash, we can install the Friendly Interactive Shell (fish). Run

```bash
sudo pacman -S fish
```

to install it. Unlike bash, fish is not POSIX compliant and uses a lot of different syntax and such, meaning it will not run scripts meant for bash or zsh. That’s fine since shell scripts will always run with the shell that they declare in their first line (e.g. `#!/bin/bash`). To ensure fish is installed correctly, simply type `fish` into the terminal. List the currently installed shells by typing `chsh -l`. Change to fish by typing

```bash
chsh -s /bin/fish
```

`$mod+Shift+e` to restart i3. Restart i3 and make sure the shell has been changed by typing

```bash
echo $SHELL
```

If all is well, `/bin/fish` should be returned.

### 8.5 Installing tide

Read the installation instructions on the [tide](https://github.com/IlanCosman/tide) GitHub repo.

### 8.6 Getting a desktop background

Install the [feh](https://wiki.archlinux.org/title/feh) package by typing `sudo pacman -S feh`. Create an images folder in the user’s home directory by typing `mkdir images/` while in `~`. Download an image to the newly created `~/images/` folder. If the file has a cumbersome name, rename it by typing

```bash
mv long-image-name.png background.png
```

Type `nvim .config/i3/config` and add the following line to make the background image stick. (I added it right after starting polybar).

```bash
exec --no-startup-id feh --bg-scale ~/images/background.png
```

### 8.7 Installing a compositor

We will be using [picom](https://wiki.archlinux.org/title/Picom) as a compositor. Type `sudo pacman -S picom` to install it. Copy the config file over to the user’s home directory by first creating the directory (`cd .config/` and `mkdir picom/`) and then typing

```bash
cp /etc/xdg/picom.conf ~/.config/picom/picom.conf
```

Enter the newly copied config file (`nvim ~/.config/picom/picom.conf`) and change

```bash
inactive-opacity = 0.80
```

Finally, add `exec --no-startup-id picom` to `~/.config/i3/config`. I added it right after `feh`. Restart i3 and xorg to see the changes.

This should add some visual candy to window transitions and add background opacity to non-selected windows. Also, it should fix any screen tearing artifacts since it uses vsync.
