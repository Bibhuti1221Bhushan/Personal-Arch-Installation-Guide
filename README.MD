![header](https://capsule-render.vercel.app/api?type=waving&section=header&color=gradient&customColorList=12&height=150&text=A%20Personal%20Arch%20Installation%20Guide&fontAlignY=35&fontSize=45)

A Personal Arch Installation Guide So If I Get Lost, This Guide Will Help Me To Remember A Bunch Of Things While Re-Installing [Arch Linux.](https://archlinux.org) If You Just Found This Guide From Some Where, I Recommend You To First Read The Official 
[Installation Guide.](https://wiki.archlinux.org/title/Installation_guide)

> **Note :** This Guide is Focus On Minimal Installation With **" GRUB " , " UEFI " , " Un-Encripted Partition "** And **" Separate Home Partition " .** 

# Pre-Installation -

## Before Installing :

+ Read The Official [Installation Guide.](https://wiki.archlinux.org/title/Installation_guide)
+ Download Installation Image From [Here.](https://www.archlinux.org/download)
+ Check Installation Image Signature.
+ Prepare An Installation Medium.
+ Boot In To The Live Environment.

## Minimum Requirements :

+ An x86_64 ( i.e. 64 Bit ) Compatible Machine.
+ Minimum 512 MB Of RAM. ( Recommended 2 GB )
+ Minimum 2 GB Of Free Disk Space. ( Recommended 20 GB )
+ An Active Internet Connection.
+ Little Bit Knowledge Of Linux Commands.

# Let's Begin Installation -

## Set Input Language Map :

> **Note :** The Default  is **US.**

More Mapping Can Be Listed With :

```bash
ls /usr/share/kbd/keymaps/**/*.map.gz
```

To Modify The Input Mapping, Omitting Path And File Extension. For Example, To Set US Input Map :

```bash
loadkeys us
```

## Set Console Font : 

More Console Fonts Can Be Listed With :

```bash
ls /usr/share/kbd/consolefonts/*.gz
```

Console Font Can Be Set With **" setfont "** Command.  For Example, To Use One Of The Fonts : 

```
setfont default8x16
```

## Verify The Boot Mode :

To Verify The Boot Mode, Check The UEFI Witness :  

```bash
cat /sys/firmware/efi/fw_platform_size
```

> + If The Command Returns 64, Then Machine is Booted In UEFI Mode And Has A **64-Bit x64 UEFI.** 
> + If The Command Returns 32, Then Machine is Booted In UEFI Mode And Has A **32-Bit IA32 UEFI,** Which is Supported But Limits The Boot Loader Choice To GRUB.
> + If The File Does't Exist, Then The Machine is Booted In **BIOS** ( CSM ) Mode.

### Connect To The Internet :

Arch Linux Needs Internet Connection To Install Arch Linux **" Base " , " Linux Kernel " , "Linux Firmware "** And Other Needed Packages.

Ensure Network Interface is Listed And Enabled :

```bash
ip link
```

Above Command Output Will Be Something Like This :

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp7s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN mode DEFAULT group default qlen 1000
		link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DORMANT group default qlen 1000
		link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff permaddr 00:00:00:00:00:00
```

> + **" lo "** is The Loop-Back Interface ( Local Server )
> + **" enp7s0 "** is The Wired Interface ( Ethernet ) 
> + **" wlan0 "** is The Wireless Interface ( Wi-Fi ) 

For Wireless And WWAN, Make Sure The Card is Not Blocked :

```bash
rfkill
```

If The Card is Blocked, Unblock Using :

```bash
rfkill unblock all
```

### Wired Connection : 
-----------------------
***Note :*** Wired Connection is Pre-Configured And Enabled By Default.
In Case, If The Wired Connection is Not Enabled, You Can Enable Connection Using :

```bash
systemctl start dhcpcd@enp7s0
```

> #### Wireless Connection - ***( Recommended iwd )*** :

If You Have Laptop Or Wireless Adapter, You Can Connect To Wireless Access Point Using ***iwctl*** Command From ***iwd***.

***Note : iwd*** is Enabled By Default.
<br>
In Case, If ***iwd*** is Not Enabled.

```bash
systemctl enable iwd
```

Scan For Wi-Fi :

```bash
iwctl station wlan0 scan
```

Get The List Of Scanned Wi-Fi :

```bash
iwctl station wlan0 get-networks
```

Connect To Your Visible Wi-Fi :

```bash
iwctl -P "PASSPHRASE" station wlan0 connect "NETWORK-NAME"
```

##### <center> OR </center>				

Connect To Your Hidden Wi-Fi :    

```bash
iwctl -P "PASSPHRASE" station wlan0 connect-hidden "NETWORK-NAME"
```

Ping A Website To Make Sure We Are Online :

```bash
ping -c 3 1.1.1.1
``` 

If You Receive Error Like ***Unknown host*** Or ***Network is unreachable,*** Means You Are Not Online Yet. Review Your Network Configuration And Redo The Above Steps.

> #### Wireless Connection - ***( wpa_supplicant )*** :

If You Have Laptop Or Wireless Adapter, You Can Connect To Wireless Access Point Using ***wpa_supplicant.***

***Note : wpa_supplicant*** is Enabled By Default.
<br>
In Case, If You Receive Error. Enable Interface Using :

```bash
ifup wlan0
```

Get The List Of Scanned Wi-Fi :

```bash
iwlist wlan0 scan | grep ESSID
```

>> ##### Connect To A Visible Wi-Fi :

Create ***wpa_supplicant.conf*** Using One Line Command :

```bash
wpa_passphrase "NETWORK-NAME" "PASSPHRASE" | tee /etc/wpa_supplicant/wpa_supplicant.conf
```

Connect To Your Wi-Fi :

```bash
wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlan0
```

##### <center>OR</center>

>> ##### Connect To A Hidden Wi-Fi : 

Create & Open ***wpa_supplicant.conf :***

```bash
nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Add This Line In Configuration File :

```bash
network={
	ssid="NETWORK-NAME"
    scan_ssid=1
    psk="PASSPHRASE"
}
```

Connect To Your Wi-Fi :

```bash
wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlan0
```

##### <center>OR</center>

>> ##### Connect To A In-Secure Wi-Fi :

Create & Open ***wpa_supplicant.conf :***

```bash
nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Add This Line In Configuration File :

```bash
network={
	ssid="NETWORK-NAME"
    key_mgmt=NONE
    priority=100
}
```

Connect To Your Wi-Fi :

```bash
wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlan0
```

##### <center>OR</center>

>> ##### Connect To A Wi-FI Via WEP-Authentication Mode :

Create & Open ***wpa_supplicant.conf :***

```bash
nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Add This Line In Configuration File :

```bash
network={
	ssid="NETWORK-NAME"
    key_mgmt=NONE
    wep_key0="PASSPHRASE"  
    wep_tx_keyidx=0
}
```

Connect To Your Wi-Fi :

```bash
wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlan0
```

Ping A Website To Make Sure We Are Online :

```bash
ping -c 3 1.1.1.1
``` 

If You Receive Error Like ***Unknown host*** Or ***Network is unreachable,*** Means You Are Not Online Yet. Review Your Network Configuration And Redo The Steps Above.

### Update The Device Clock :

To Ensure That Clock is Accurate :

```bash
timedatectl set-ntp true
```

To Check The Service Status :

```bash
timedatectl status
```

### Partition The Disks :

When The Disks Are Recognized By The Live Environment, Disks Are Assigned To A Block Device Such As ***/dev/sda, /dev/nvme0n1 or /dev/mmcblk0.*** 
<br>
To Identify These Devices :

```bash
lsblk
```

***Note :*** Results Ending In ***rom, loop*** Or ***airoot*** May Be Ignored.

Let’s Clean Our Drive To Create New Partitions Table For Our Installation. In This Guide, We Will Use ***/dev/sda*** As Our Installation Disk.

```bash
fdisk /dev/sda
```

+ Press <kbd>**Return**</kbd> To Open ***dev/sda*** In ***fdisk***. 

+ Press <kbd>**p**</kbd> To Show Current Partition. Now We Should See Our Drive Showing The ***Partition Number, Partition Size,*** And ***Partition Name.***

+ Press <kbd>**g**</kbd> To ***<u style="color:red;">Format Entire Drive</u>*** And Create An Empty ***GPT Partition Table.***

**Note :** Press <kbd>**d**</kbd> To Delete A Single Partition. 

>> ##### Create The Boot Partition :

+ Press <kbd>**n**</kbd> To ***Create New Partition.*** You Will Be Prompted To Choose A Partition Number.

+ Press <kbd>**1**</kbd> To ***Select Partition Number 1.***

+ Press <kbd>**Return**</kbd> To Continue With The ***Default Block Size For First Sector.***

+ Enter <kbd>**+512M**</kbd> In ***The Last Sector.*** And Press <kbd>**Return**</kbd> To Create ***EFI Partition With 512 Mib.***

+ Press <kbd>**t**</kbd> To ***Change Partition Type*** Of The EFI Partition.

+ Enter <kbd>**1**</kbd> For ***EFI System.*** ( Default is Linux System )

>> ##### Create The Swap Partition :

+ Press <kbd>**n**</kbd> To ***Create New Partition.*** You Will Be Prompted To Choose A Partition Number.

+ Press <kbd>**2**</kbd> To ***Select Partition Number 2.***

+ Press <kbd>**Return**</kbd> To Continue With The ***Default Block Size For First Sector.***

+ Enter <kbd>**+8G**</kbd> In ***The Last Sector.*** And Press <kbd>**Return**</kbd> To Create ***Swap Partition With 8 Gib.***

+ Press <kbd>**t**</kbd> To ***Change Partition Type*** Of The Swap Partition.

+ Enter <kbd>**19**</kbd> For ***Linux Swap.*** ( Default is Linux System )

>> ##### Create The Root Partition :

+ Press <kbd>**n**</kbd> To ***Create New Partition.*** You Will Be Prompted To Choose A Partition Number.

+ Press <kbd>**3**</kbd> To ***Select Partition Number 3.***

+ Press <kbd>**Return**</kbd> To Continue With The ***Default Block Size For First Sector.***

+ Enter <kbd>**+30G**</kbd> In ***The Last Sector.*** And Press <kbd>**Return**</kbd> To Create ***Root Partition With 30 Gib.***

+ **Note :** No Need To Change Partition Type. ***Default is Linux System.***

>> ##### Create The Home Partition :

+ Press <kbd>**n**</kbd> To ***Create New Partition.*** You Will Be Prompted To Choose A Partition Number.

+ Press <kbd>**4**</kbd> To ***Select Partition Number 4.***

+ Press <kbd>**Return**</kbd> To Continue With The ***Default Block Size For First Sector.***

+ Press <kbd>**Return**</kbd> In ***The Last Sector*** To Create ***Root Partition Of Remaining Space.***

+ **Note :** No Need To Change Partition Type. ***Default is Linux System.***

+ Press <kbd>**p**</kbd> To Print The Newly Created Disk Partitions.

+ Press <kbd>**w**</kbd> To ***Write And Quit*** From ***fdisk*** Command.

### Verifying The Partitions :

Use ***lsblk*** Again To Check The Created Partitions. <u>***We? I Thought I'm Doing This Guide For Self Lol.***</u>

```bash
lsblk
```

You Should See ***Something Like This :***

| NAME | MAJ:MIN | RM |  SIZE  | RO | TYPE | MOUNTPOINTS |
| ---- | ------- | -- | ------ | -- | ---- | ----------- |
| sda  |   8:0   | 0  |  240G  |  0 |      |             |
| sda1 |   8:1   | 0  |  512M  |  0 | part |             |
| sda2 |   8:2   | 0  |   8G   |  0 | part |             |
| sda3 |   8:3   | 0  |   30G  |  0 | part |             |
| sda4 |   8:3   | 0  | 201.5G |  0 | part |             |

+ **sda** is The Main Disk.  
+ **sda1** is The Boot Partition.  
+ **sda2** is The Swap Partition.  
+ **sda3** is The Root Partition.  
+ **sda4** is The Home Partition.  

### Format The Partitions :

Format ***/dev/sda1*** Partition As Boot Partition In ***FAT32***.

```bash
mkfs.fat -F 32 -n EFI /dev/sda1
```

Format ***/dev/sda2*** Partition As Swap Partition.

```bash
mkswap -L SWAP /dev/sda2
```

Format ***/dev/sda3*** And ***/dev/sda4*** Partition As ***'Root'*** And ***'Home'*** Partition In ***EXT4***.

```bash
mkfs.ext4 -L ARCH /dev/sda3
mkfs.ext4 -L HOME /dev/sda4
```

### Mount The Partitions :

Mount The Root Partition ***/dev/sda3*** To ***/mnt***.

```bash
mount /dev/sda3 /mnt
```

Create A ***/boot/EFI*** Directory For Boot Partition.

```bash
mkdir -p /mnt/boot/EFI  
```

Mount The Boot Partition ***/dev/sda1/*** To ***/mnt/boot/EFI*** Partition.

```bash
mount /dev/sda1 /mnt/boot/EFI
```

Create a ***/home*** mountpoint:

```
mkdir /mnt/home  
```

Mount ***/dev/sda4*** to ***/mnt/home*** partition. This is will be our `/home`:

```
mount /dev/sda1 /mnt/home
```



## Installation

Now let’s go ahead and install `base`, `linux`, `linux-firmware`, and `base-devel` packages into our system. 

```
# pacstrap /mnt base base-devel linux linux-zen linux-firmware
```

I will install `linux-zen` since it has necessary modules for gaming.

The `base` package does not include all tools from the live installation, so installing other packages may be necessary for a fully functional base system. In particular, consider installing: 

+ software necessary for networking,

	- `dhcpcd`: RFC2131 compliant DHCP client daemon
	- `iwd`: Internet Wireless Daemon
	- `inetutils`: A collection of common network programs
	- `iputils`: Network monitoring tools, including `ping`

+ utilities for accessing `RAID` or `LVM` partitions,

	- `lvm2`: Logical Volume Manager 2 utilities (*if you are setting up an encrypted filesystem with LUKS/LVM, include this on pacstrap*)

+ Zram

	- `zram-generator`

+ a text editor(s),

	- `nano`
	- `vim`
	- `vi`

+ packages for accessing documentation in man and info pages,

	- `man-db`
	- `man-pages`

+ Microcode

	- `intel-ucode`/`amd-ucode`

+ tools:

	- `git`: the fast distributed version control system
	- `tmux`: A terminal multiplexer
	- `less`: A terminal based program for viewing text files
	- `usbutils`: USB Device Utilities
	- `bash-completion`: Programmable completion for the bash shell

+ userspace utilities for the management of file systems that will be used on the system,
	
	- `ntfs-3g`: NTFS filesystem driver and utilities
	- `unrar`: The RAR uncompression program
	- `unzip`: For extracting and viewing files in `.zip` archives
	- `p7zip`: Command-line file archiver with high compression ratio
	- `unarchiver`: `unar` and `lsar`: Objective-C tools for uncompressing archive files
	- `gvfs-mtp`: Virtual filesystem implementation for `GIO` (`MTP` backend; Android, media player)
	- `libmtp`: Library implementation of the Media Transfer Protocol
	- `android-udev`: Udev rules to connect Android devices to your linux box
	- `mtpfs`: A FUSE filesystem that supports reading and writing from any MTP devic
	- `xdg-user-dirs`: Manage user directories like `~/Desktop` and `~/Music`

These tools will be useful later. So **future me**, install these.

## Generating the fstab

```
# genfstab -U /mnt >> /mnt/etc/fstab
```

Check the resulting `/mnt/etc/fstab` file, and edit it in case of errors. 

## Chroot

Now, change root into the newly installed system  

```
# arch-chroot /mnt /bin/bash
```

## Time zone

A selection of timezones can be found under `/usr/share/zoneinfo/`. Since I am in the Philippines, I will be using `/usr/share/zoneinfo/Asia/Manila`. Select the appropriate timezone for your country:

```
# ln -sf /usr/share/zoneinfo/Asia/Manila /etc/localtime
```

Run `hwclock` to generate `/etc/adjtime`: 

```
# hwclock --systohc
```

This command assumes the hardware clock is set to UTC.

## Localization

The `locale` defines which language the system uses, and other regional considerations such as currency denomination, numerology, and character sets. Possible values are listed in `/etc/locale.gen`. Uncomment `en_US.UTF-8`, as well as other needed localisations.

**Uncomment** `en_US.UTF-8 UTF-8` and other needed locales in `/etc/locale.gen`, **save**, and generate them with:  

```
# locale-gen
```

Create the `locale.conf` file, and set the LANG variable accordingly:  

```
# locale > /etc/locale.conf
```

If you set the keyboard layout earlier, make the changes persistent in `vconsole.conf`:

```
# echo "KEYMAP=us" > /etc/vconsole.conf
```

Not using `us` layout? Replace it, stoopid.

## Network configuration

Create the hostname file. In this guide I'll just use `MYHOSTNAME` as hostname. Hostname is the host name of the host. Every 60 seconds, a minute passes in Africa.

```
# echo "MYHOSTNAME" > /etc/hostname
```

Open `/etc/hosts` to add matching entries to `hosts`:

```
127.0.0.1    localhost  
::1          localhost  
127.0.1.1    MYHOSTNAME.localdomain	  MYHOSTNAME
```

If the system has a permanent IP address, it should be used instead of `127.0.1.1`.

## Initramfs  

Creating a new initramfs is usually not required, because mkinitcpio was run on installation of the kernel package with pacstrap. **This is important** if you are setting up a system with encryption!

### Unencrypted filesystem

	```
	# mkinitcpio -P
	```

	DO NOT FORGET TO RUN THIS BEFORE REBOOTING YOUR SYSTEM!

### Encrypted filesystem with LVM/LUKS

+ Open `/etc/mkinitcpio.conf` with an editor:

+ In this guide, there are two ways to setting up initramfs, `udev` (default) and `systemd`. If you are planning to use `plymouth`(splashcreen), it is advisable to use a `systemd`-based initramfs.

	- udev-based initramfs (default).

		Find the `HOOKS` array, then change it to something like this:

		```
		HOOKS=(base udev autodetect keyboard modconf block encrypt lvm2 filesystems fsck)
		```

	- systemd-based initramfs.

		Find the `HOOKS` array, then change it to something like this:

		```
		HOOKS=(base systemd autodetect keyboard sd-vconsole modconf block sd-encrypt lvm2 filesystems fsck)
		```

	- Regenerate initramfs image:

		```
		# mkinitcpio -P
		```

		DO NOT FORGET TO RUN THIS BEFORE REBOOTING YOUR SYSTEM!

### Making Swap File and ZSwap

#### Time to create a swap file! I'll make two gigabytes swap file.

```
# dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
```

Set the right permissions
```
# chmod 0600 /swapfile
```

After creating the correctly sized file, format it to swap:
```
# mkswap -U clear /swapfile
```

Activate the swap file
```
# swapon /swapfile
```

Finally, edit the fstab configuration to add an entry for the swap file in `/etc/fstab`:
```
/swapfile none swap defaults,pri=10 0 0
```

#### Install zram-generator:

```
# pacman -S zram-generator
```

Let's make a config file at `/etc/systemd/zram-generator.conf
!` I prefer having HALF of my TOTAL RAM as zswap size. My laptop have 4 cores, so I'll distribute it to FOUR zram devices. So I'll uthis config :

```
[zram0]
zram-size = ram/8
compression-algorithm = zstd
swap-priority = 100

[zram1]
zram-size = ram/8
compression-algorithm = zstd
swap-priority = 100

[zram2]
zram-size = ram/8
compression-algorithm = zstd
swap-priority = 100

[zram3]
zram-size = ram/8
compression-algorithm = zstd
swap-priority = 100
```

No need to enable/start anything, it will automatically initialize zram devices! Just reboot and run `swapon -s` to check the swap you have.

## Adding Repositories - `multilib` and `AUR`

Enable multilib and AUR repositories in `/etc/pacman.conf`. Open it with your editor of choice:

### Adding multilib repository

Uncomment `multilib` (remove # from the beginning of the lines). It should look like this:  

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

### Adding the AUR repository

Add the following lines at the end of your `/etc/pacman.conf` to enable the AUR repo:  

```
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

### `pacman` goodies

You can enable the "easter-eggs" and goodies in `pacman`, the package manager of archlinux.

Open `/etc/pacman.conf`, then find `# Misc options`. 

To add colors to `pacman`, uncomment `Color`. Then add `Pac-Man` to `pacman` by adding `ILoveCandy` under the `Color` string. To enable parallel downloads, uncomment it too:

```
Color
ILoveCandy
ParallelDownloads = 3
```

### Update repositories and packages

To check if you successfully added the repositories and enable the easter-eggs, run:

```
# pacman -Syu
```

If updating returns an error, open the `pacman.conf` again and check for human errors. Yes, you f'ed up big time.

## Root password

Set the `root` password:  

```
# passwd
```

## Add a user account

Add a new user account. In this guide, I'll just use `MYUSERNAME` as the username of the new user aside from `root` account. (My phrasing seems redundant, eh?) Of course, change the example username with your own:  

```
# useradd -m -g users -G wheel,storage,power,video,audio,rfkill,input -s /bin/bash MYUSERNAME
```

This will create a new user and its `home` folder.

Set the password of user `MYUSERNAME`:  

```
# passwd MYUSERNAME
```

## Add the new user to sudoers:

If you want a root privilege in the future by using the `sudo` command, you should grant one yourself:

```
# EDITOR=vim visudo
```

Uncomment the line (Remove #):

```
# %wheel ALL=(ALL) ALL
```

## Install the boot loader

Yeah, this is where we install the bootloader. We will be using `systemd-boot`, so no need for `grub2`. 

+ Install bootloader:
	
	We will install it in `/boot` mountpoint (`/dev/sda1` partition).

	```
	# bootctl --path=/boot install
	```

+ Create a boot entry `/boot/loader/entries/arch.conf`, then add these lines:

### Unencrypted filesystem

	```
	title Arch Linux  
	linux /vmlinuz-linux  
	initrd  /initramfs-linux.img  
	options root=/dev/sda3 rw
	```

	If your `/` is not in `/dev/sda3`, make sure to change it. 

	Save and exit.

### Encrypted filesystem

Remember the two-types of initramfs earlier? Each type needs a specific kernel parameters. So there's also a two type of entries here. Remember that `volume` is the volume group name and `/dev/mapper/volume-root` is the path to `/`.

+ udev-based initramfs

	```
	title Arch Linux  
	linux /vmlinuz-linux  
	initrd  /initramfs-linux.img  
	options cryptdevice=UUID=/DEV/SDA2/UUID/HERE:volume root=/dev/mapper/volume-root rw
	```

	Replace `/DEV/SDA2/UUID/HERE` with the UUID of your `LVM` partition. You can check it by running `blkid /dev/sda2`. Note that `cryptdevice` parameter  is unsupported by plymouth so it's advisable to use systemd-based initramfs if you are planning to use it.

	Tip: If you are using `vim`, you can write the UUID easier by typing `:read ! blkid /dev/sda2` then hit enter. Then manipulate the output by using visual mode.

+ systemd-based initramfs

	```
	title Arch Linux
	linux /vmlinuz-linux
	initrd /intel-ucode.img
	initrd /initramfs-linux.img
	options rd.luks.name=/DEV/SDA2/UUID/HERE=volume root=/dev/mapper/volume-root rw
	```

	Replace `/DEV/SDA2/UUID/HERE` with the UUID of your `LVM` partition. You can check it by running `blkid /dev/sda2`.

	Tip: If you are using `vim`, you can write the UUID easier by typing `:read ! blkid /dev/sda2` then hit enter. Then manipulate the output by using visual mode.

### Update boot loader configuration

Update bootloader configuration

```
# vim /boot/loader/loader.conf
```

Delete all of its content, then replaced it by:

```
default arch.conf
timeout 0
console-mode max
editor no
```

#### Microcode

Processor manufacturers release stability and security updates to the processor microcode. These updates provide bug fixes that can be critical to the stability of your system. Without them, you may experience spurious crashes or unexpected system halts that can be difficult to track down. 

If you didn't install it using pacstrap, install microcode by:

For AMD processors:

```
# pacman -S amd-ucode
```

For Intel processors:

```
# pacman -S intel-ucode
```

If your Arch installation is on a removable drive that needs to have microcode for both manufacturer processors, install both packages. 

Load  microcode. For `systemd-boot`, use the `initrd` option to load the microcode, **before** the initial ramdisk, as follows:

```
# sudoedit /boot/loader/entries/entry.conf
```

```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /CPU_MANUFACTURER-ucode.img
initrd  /initramfs-linux.img
...
```

Replace `CPU_MANUFACTURER` with either `amd` or `intel` depending on your processor.

## Enable internet connection for the next boot

To enable the network daemons on your next reboot, you need to enable `dhcpcd.service` for wired connection and `iwd.service` for a wireless one.

```
# systemctl enable dhcpcd iwd
```

## Exit chroot and reboot:  

Exit the chroot environment by typing `exit` or pressing <kbd>Ctrl + d</kbd>. You can also unmount all mounted partition after this. 

Finally, `reboot`.

##  Finale

If your installation is a success, then ***yay!!!*** If not, you should start questioning your own existence. Are your parents proud of you? 





![header](https://capsule-render.vercel.app/api?type=waving&section=footer&color=gradient&customColorList=12&height=125&desc=Made%20With%20%20%20🩷&descAlignY=50&animation=fadeIn&fontSize=5000&stroke=feac5e&strokeWidth=50)


