## 0. Pre-Installation

## 0.1 Acquire an Installation Image

Visit the [Download](https://www.archlinux.org/download/) page.





## 0.2 Create an Arch Linux Installer USB Stick

- Linux

  ```bash
  sudo dd bs=4M if=path/to/iso of=/dev/NAME status=progress oflag=sync
  ```

  Or simply

  ```bash
  su -c 'cat path/to/archlinux.iso > /dev/sdx'
  ```

  And other methods:

  ```bash
  cp path/to/archlinux.iso /dev/sdx
  tee < path/to/archlinux.iso > /dev/sdx
  ```

- Windows

  - [Rufus - The Official Website (Download, New Releases)](https://rufus.ie/)
  
    > Note: If the USB drive does not boot properly using the default ISO Image mode, **DD Image mode** should be used instead.
  
  - [USBWriter download | SourceForge.net](https://sourceforge.net/projects/usbwriter/)





## 0.3 Boot the Live Environment

> **Note:** Arch Linux installation images do not support Secure Boot. You will need to [disable Secure Boot](https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface/Secure_Boot#Disable_Secure_Boot) to boot the installation medium.

**Boot:**

1. Plug in your USB and press `F2`, `F12` or something else during the [POST](https://en.wikipedia.org/wiki/Power-on_self_test) phase.

2. Select the first column.

3. Then you will be logged in on the first [virtual console](https://en.wikipedia.org/wiki/Virtual_console) as the root user, and presented with a [Zsh](https://wiki.archlinux.org/index.php/Zsh) shell prompt.

   > To switch to a different console, use the Alt+arrow shortcut.





## 0.4 Verify the Boot Mode

> To verify the boot mode, list the [efivars](https://wiki.archlinux.org/index.php/Efivars) directory:
>
> ```bash
> ls /sys/firmware/efi/efivars
> ```
>
> If the command shows the directory without error, then the system is booted in UEFI mode. If the directory does not exist, the system may be booted in [BIOS](https://en.wikipedia.org/wiki/BIOS) (or [CSM](https://en.wikipedia.org/wiki/Compatibility_Support_Module)) mode. 





## 0.5 Connect to the Internet

Don't be stupid, please use Ethernet.

The connection may be verified with:

```
pacman -Syy
```





## 0.6 Update the System Clock

Use [timedatectl(1)](https://jlk.fjfi.cvut.cz/arch/manpages/man/timedatectl.1) to ensure the system clock is accurate:

```bash
timedatectl set-ntp true
```

To check the service status, use `timedatectl status`.





## 0.7 Partition the Disks

**First list devices:**

```bash
fdisk -l # Results ending in rom, loop or airoot may be ignored.
```



The following [partitions](https://wiki.archlinux.org/index.php/Partition) are **required** for a chosen device:

- One partition for the [root directory](https://en.wikipedia.org/wiki/Root_directory) `/`.
- For booting in [UEFI](https://wiki.archlinux.org/index.php/UEFI) mode: an [EFI system partition](https://wiki.archlinux.org/index.php/EFI_system_partition).



Partition your hard drive (`/dev/sda` will be used in this guide) with **`fdisk`** or **`cfdisk`**, the partition numbers and order are at your discretion:

```
cfdisk /dev/sda
```

**UEFI with** [GPT](https://wiki.archlinux.org/index.php/GPT)

| Mount point |           Partition           |    Partition type     |     Suggested size      |
| :---------: | :---------------------------: | :-------------------: | :---------------------: |
| `/mnt/boot` | `/dev/*efi_system_partition*` | EFI system partition  |          500 M          |
|   `/mnt`    |    `/dev/*root_partition*`    | Linux x86-64 root (/) | Remainder of the device |





## 0.8 Format the Partitions

```bash
mkfs.fat -F32 /dev/sda1; mkfs.ext4 /dev/sda2
```





## 0.8 Mount the File Systems

```bash
mount /dev/sda2 /mnt; mkdir /mnt/boot; mount /dev/sda1 /mnt/boot
```

```bash
root@archiso ~ # lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0    60G  0 disk 
├─sda1   8:1    0   500M  0 part /mnt/boot
└─sda2   8:2    0  59.5G  0 part /mnt
```

[genfstab(8)](https://jlk.fjfi.cvut.cz/arch/manpages/man/genfstab.8) will later detect mounted file systems and swap space.







# 1. Installation

## 1.1 Select the Mirrors

```bash
reflector -c China -f 10 --save /etc/pacman.d/mirrorlist; cat /etc/pacman.d/mirrorlist
```

`/etc/pacman.d/mirrorlist` will later be copied to the new system by `pacstrap`, so it is worth getting right.



```bash
pacman -Syy fish; fish # swtich to fish shell
```





## 1.2 Install Essential Packages

```bash
pacstrap /mnt base base-devel linux linux-firmware linux-headers vim bash fish reflector git
```





## 1.3 Fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab; cat /mnt/etc/fstab # check
```







# 2. Configure the system

## 2.1 Change root

```bash
arch-chroot /mnt
fish 
```





## 2.2 Swap

### 2.2.1 Swap Partition

First create a swap partition using `fdisk` or `cfdsik`.

Then

```bash
mkswap /dev/sd?
swapon /dev/sd?
```

To enable this swap partition on boot, add an entry to `/etc/fstab`:

```
UUID=device_UUID none swap defaults 0 0
```

Get UUID by:

```bash
ls -l /dev/disk/by-uuid
```



#### Disabling swap

To deactivate specific swap space:

```bash
swapoff /dev/sdxy
```

And remove the relevant entry from `/etc/fstab`.



### 2.2.2 Swap Device

Use [dd](https://wiki.archlinux.org/index.php/Dd) to create a swap file the size of your choosing. 

For example:

```bash
dd if=/dev/zero of=/swapfile bs=1M count=512 status=progress # 512M
dd if=/dev/zero of=/swapfile bs=1G count=9 status=progress # 9G
```

Then

```bash
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo "/swapfile none swap defaults 0 0" >> /etc/fstab
cat /etc/fstab # check
```



#### Remove swap file

To remove a swap file, it must be turned off first and then can be removed:

```bash
swapoff /swapfile
rm -f /swapfile
```

Finally remove the relevant entry from `/etc/fstab`.





## 2.3 Time Zone

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime; timedatectl set-ntp true; hwclock --systohc; date # check
```





## 2.4 Localization

**First edit `/etc/locale.gen`** and uncomment 

```bash
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

**Then **Generate the locales:

```
locale-gen
```

**Lastly** create the [locale.conf(5)](https://jlk.fjfi.cvut.cz/arch/manpages/man/locale.conf.5) file, and [set the LANG variable](https://wiki.archlinux.org/index.php/Locale#Setting_the_system_locale) accordingly:

```bash
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
```





## 2.5 Network Configuration

**First** create your hostname:

```bash
echo "arch" >> /etc/hostname
```

**Then** add matching entries to [hosts(5)](https://jlk.fjfi.cvut.cz/arch/manpages/man/hosts.5):

```bash
/etc/hosts
127.0.0.1    localhost
::1          localhost
127.0.1.1    arch.localdomain arch
```

**And install network manager:**

```bash
pacman -S networkmanager network-manager-applet nm-connection-editor 

systemctl enable NetworkManager.service
```





## 2.6 Set Root Password

```bash
passwd
```





## 2.7 Boot Loader

```bash
pacman -S grub efibootmgr os-prober dosfstools ntfs-3g mtools amd-ucode
```

```bash
grub-install --target=x86_64-efi --efi-directory=/boot/ --bootloader-id=GRUB; grub-mkconfig -o /boot/grub/grub.cfg
```





## 2.8 Reboot







# Tricks

## Openssh

If you want to install (arch) linux from another system, you can use `openssh` to do that.

> If you are using a vm, you need to enable vm's bridge network feature.
>
> - Vmware
>   - Go to `VM->Setting->Network Adapater`, and click `Bridged: ...`.
> - Virtual Box
>   - Go to `Setting->Network->Adapter2`, and choose `Bridged Adapter` and `enp...`.

```bash
pacman -S openssh
# systemctl enable sshd
systemctl start sshd
passwd # make sure you have set your password
```

Now run `ip a` to find your ip address.

```bash
root@archiso ~ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:13:dc:3f brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 192.168.0.104/24 brd 192.168.0.255 scope global dynamic ens33
       valid_lft 5177sec preferred_lft 5177sec
    inet6 fe80::20c:29ff:fe13:dc3f/64 scope link 
       valid_lft forever preferred_lft forever
```

And `192.168.0.104` is what we need.

Now you can log into your installation system from another system.

```bash
ssh root@192.168.0.104 # DO NOT FOLLOW IT WITH -p 24
```



### Enable SSH Root Login

By default, root login via ssh is not enabled. We need to change some settings inside the `sshd_config` file to enable root login.

```bash
sudo vim /etc/ssh/sshd_config
```

Find the following lines and make some changes to it

```bash
#PermitRootLogin prohibit-password
PermitRootLogin yes
PasswordAuthentication yes
```

Close and save the file. Finally restart ssh service

```bash
sudo systemctl restart sshd
```

Now try to login again.

