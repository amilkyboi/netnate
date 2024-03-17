+++
title = 'archinstall'
date = 2024-03-17
draft = true
tags = ['computing']
summary = 'Like archinstall, but not.'
+++

Many parts of this writeup are taken verbatim from the [official Arch Linux installation guide](https://wiki.archlinux.org/title/Installation_guide). Always consult the official guide for the most up-to-date information. This guide attempts to consolidate multiple wiki pages into a single source for a more cohesive installation process. Note that the steps listed in the pre-installation section are directed at Windows users.

## 1 Pre-installation

### 1.1 Acquire an installation image

A torrent client is recommended for downloading the ISO file---the [qBittorrent](https://www.qbittorrent.org/download) client is a good choice. Download the latest stable version, but do not open the executable yet. On the qBittorrent downloads page, open the `checksums and library versions` tab and copy the SHA-256 checksum for the 64-bit installer. Open a PowerShell instance, navigate to the folder containing the executable, and verify the checksum by typing:

```powershell
(Get-FileHash ".\qbittorrent_version_x64_setup.exe").Hash -eq "expected-hash"
```

If the prompt returns `True`, go ahead and install qBittorrent.

Next, visit the Arch Linux [download](https://archlinux.org/download/) page and click the magnet link under the BitTorrent section to download the ISO. This will trigger a prompt to open the selected file using qBittorrent, after which the file will begin the torrent process. Once the download is complete, the client will begin "seeding" the file for other users. This process can be terminated manually by removing the file from the transfer list.

### 1.2 Verify signature

Before using the Arch Linux ISO, the signature attached to the file must be verified using GNU Privacy Guard (GPG). In a PowerShell instance, install `gpg` via `winget`:

```powershell
winget install GnuPG.GnuPG
```

Visit the [checksums](https://archlinux.org/download/#checksums) section of the Arch Linux download page and download the ISO PGP signature file. Navigate to the directory containing the ISO file and validate the signature using `gpg`:

```powershell
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig archlinux-version-x86_64.iso
```

The expected output is:

```powershell
Good signature from "First Last <developername@archlinux.org>" [unknown]
```

The User ID might not be certified, and will generate the following output:

```powershell
WARNING: The key's User ID is not certified with a trusted signature!
```

To be extra cautious, visit the [developers](https://archlinux.org/people/developers/) page and match the primary key fingerprint output by `gpg` to the PGP key listed under the appropriate developer.

### 1.3 Prepare an installation medium

For a hardware installation, dowload [Rufus](https://rufus.ie/en/) to create a bootable USB flash drive. After downloading, right-click the executable and navigate to the `Digital Signatures` tab. The signed name should state "Akeo Consulting."

Plug in a USB flash drive and launch Rufus. Select the ISO downloaded earlier and change the partition scheme to `GPT`. The target system should automatically switch to `UEFI (non CSM)`. Name the volume and leave the file system as `Large FAT32` with a cluster size of `32 kB`. Click `START` to write the ISO to the removable drive. If a pop-up is encountered, continue with writing the disk in ISO image mode. Rufus can be closed when the status bar reads `READY`. Eject the drive and physically remove it.

### 1.4 Boot the live environment

Boot to the BIOS settings on the target machine. Find Secure Boot and disable it, as this feature will prevent booting from an external device if this step is not performed. While in the BIOS, hyper-threading and CPU virtualization should also be enabled. Save the changed settings and exit the BIOS.

Plug the removable drive into the target machine and power it on. Enter the startup interrupt menu and proceed to the boot selection menu. Select the removable drive as the boot device. After arriving at the GNU GRUB menu, select `Arch Linux install medium (x86_64, UEFI)`. You're loaded in once the Zsh tty prompt appears:

```zsh
root@archiso ~ #
```

### 1.5 Set the console keyboard layout and font

For users based in the United States, these settings can remain unchanged as the default console keymap is US. The available layouts can be listed with:

```zsh
localectl list-keymaps
```

To set the keyboard layout, pass its name to `loadkeys(1)`. For example, to set a German keyboard layout:

```zsh
loadkeys de-latin1
```

Console fonts are located in `/usr/share/kbd/consolefonts/` and can likewise be set with `setfont(8)` omitting the path and file extension. For example, to use one of the largest fonts suitable for HiDPI screens, run:

```zsh
setfont ter-132b
```

### 1.6 Verify the boot mode

Verify the boot mode by checking the UEFI bitness:

```zsh
cat /sys/firmware/efi/fw_platform_size
```

If the command returns `64`, then the system is booted in UEFI mode and has a 64-bit x64 UEFI. If the command returns 32, then system is booted in UEFI mode and has a 32-bit IA32 UEFI; while this is supported, it will limit the boot loader choice to systemd-boot. If the file does not exist, the system may be booted in BIOS (or CSM) mode. If the system did not boot in the mode you desired (UEFI vs BIOS), refer to your motherboard's manual.

### 1.7 Connect to the internet

Ensure that your network interface is listed and enabled by typing:

```zsh
iplink
```

Some number of network devices should appear, the amount and type dependent upon the current machine. Devices named `lo` and labeled `link/loopback` can be ignored. Once the network has been set up using one of the options below, test that the network has been properly connected to by running:

```zsh
ping archlinux.org
```

#### Ethernet

Ethernet connectivity works out of the box. Simply plug in the ethernet cable.

#### Wireless

For wireless connectivity, enter the `iwctl` utility:

```zsh
iwctl
```

Get a list of the wireless devices:

```zsh
device list
```

Here, something like `wlan0` should appear. Ensure the device is powered on by reading the appropriate column. If the device is powered off, type:

```zsh
device device_name set-property Powered on
```

Still in the `iwd` prompt, type adapter list and ensure that the adapter is also powered on. If not, type:

```zsh
adapter adapter_name set-property Powered on
```

Begin the scan for networks within range by typing:

```zsh
station device_name scan
```

You can then list all available networks:

```zsh
station device_name get-networks
```

Connect to the appropriate network with the command

```zsh
station device_name connect SSID
```

and enter the password for the network. Finally, exit the `iwd` prompt by typing `exit`.

### 1.8 Update the system clock

In the live environment, `systemd-timesyncd` is enabled by default and time will be synced automatically once a connection to the internet is established. Use `timedatectl(1)` to ensure the system clock is accurate:

```zsh
timedatectl
```

For now, the time zone should be UTC, the system clock should be syncronized, and network time protocol (NTP) should be active. If NTP reads inactive, type:

```zsh
timedatectl set-ntp true
```

### 1.9 Partition the disks

Resources:

- [read about fdisk](https://wiki.archlinux.org/title/Fdisk)
- [read about partitioning](https://wiki.archlinux.org/title/Partitioning)
- [read about efi](https://wiki.archlinux.org/title/EFI_system_partition)
- [read about swap](https://wiki.archlinux.org/title/Swap)

Identify the devices recognized by the live system using the `fdisk` utility.

```zsh
fdisk -l
```

Remove old partitions.

Create a new GUID Partition Table (GPT).

Modify the partition tables.

```zsh
fdisk /dev/selected_disk
```

#### Single root partition

| Mount point | Partition             | GPT partition type  | GUID                                 | Suggested size                               |
| ----------- | --------------------- | ------------------- | ------------------------------------ | -------------------------------------------- |
| `/mnt/boot` | `/dev/efi_partition`  | EFI system          | C12A7328-F81F-11D2-BA4B-00A0C93EC93B | 1 GiB                                        |
| `[SWAP]`    | `/dev/swap_partition` | Linux swap          | 0657FD6D-A4AB-43C4-84E5-0933C84B4F4F | At least 4 GiB                               |
| `/mnt`      | `/dev/root_partition` | Linux root (x86-64) | 4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709 | Remainder of the device. At least 23–32 GiB. |

#### Discrete partitions

| Mount point | Partition             | GPT partition type  | GUID                                 | Suggested size          |
| ----------- | --------------------- | ------------------- | ------------------------------------ | ----------------------- |
| `/mnt/boot` | `/dev/efi_partition`  | EFI system          | C12A7328-F81F-11D2-BA4B-00A0C93EC93B | 1 GiB                   |
| `[SWAP]`    | `/dev/swap_partition` | Linux swap          | 0657FD6D-A4AB-43C4-84E5-0933C84B4F4F | At least 4 GiB          |
| `/mnt`      | `/dev/root_partition` | Linux root (x86-64) | 4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709 | At least 23–32 GiB      |
| `/mnt/home` | `/dev/home_partition` | Linux home          | 933AC7E1-2EB4-4F13-B844-0E14E2AEF915 | Remainder of the device |

### 1.10 Format the partitions

boot

```zsh
mkfs.fat -F 32 /dev/efi_partition
```

swap

```zsh
mkswap /dev/swap_partition
```

root

```zsh
mkfs.ext4 /dev/root_partition
```

home

```zsh
mkfs.ext4 /dev/home_partition
```

### 1.11 Mount the file systems

boot

```zsh
mount --mkdir /dev/efi_partition /mnt/boot
```

swap

```zsh
swapon /dev/swap_partition
```

root

```zsh
mount /dev/root_partition /mnt
```

home

```zsh
mount --mkdir /dev/home_partition /mnt/home
```

## 2 Installation

### 2.1 Select the mirrors

Before installing any packages with `pacman`, it is beneficial to select a list of mirrors optimized for geographic locality and download speed. First, back up the current mirrorlist in case of bad overwrites:

```zsh
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
```

Use the built-in `reflector` script to save a list of the 20 most recently syncronized mirrors (located in the US and using the https protocol) into the `pacman` mirrorlist, sorted by download rate:

```zsh
reflector --verbose --latest 20 --country US --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

To enable parallel downloads, open the `pacman` configuration file for editing:

```zsh
vim /etc/pacman.conf
```

The following lines will be listed under the `[options]` section. Uncomment them to enable colors (for increased clarity when downloading and installing packages) and parallel downloads:

```zsh
[options]
#Colors
...
#ParallelDownloads = 5
```

### 2.2 Install essential packages

The following lines will be listed under the `[options]` section. Uncomment them to enable colors (for increased clarity when downloading and installing packages) and parallel downloads:

{{< notice note >}}
No software or configuration (except for `/etc/pacman.d/mirrorlist`) gets carried over from the live environment to the installed system.
{{< /notice >}}

## 3 Configure the system

### 3.1 Fstab

### 3.2 Chroot

### 3.3 Time

### 3.4 Localization

### 3.5 Network configuration

### 3.6 Initramfs

### 3.7 Root password

### 3.8 Boot loader

## 4 Reboot

## 5 Post-installation
