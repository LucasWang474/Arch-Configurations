# References

- [Youtube: EF - Linux Made Simple](https://www.youtube.com/channel/UCX_WM2O-X96URC5n66G-hvw) (强烈推荐)

- [Arch wiki](https://wiki.archlinux.org/index.php/installation_guide)
- [How to install Arch Linux in the cloud](https://upcloud.com/community/tutorials/install-arch-linux/)
- [Installing 2019 Arch Linux on a Vultr Server](https://www.vultr.com/docs/installing-2019-arch-linux-on-a-vultr-server?__cf_chl_jschl_tk__=741c9f6169ab579e8dbfde914747b1743bcf6c02-1612449275-0-AV3LXK85jCpdIFwKeZJVVNuB7QM-r8K4IK_LsFdmQxllDU3LiULXQvdGa7fBEFt7lduB_rG_HBgd_g_orUTavhpVJ2l291gmTTldDwLWqFWdewReuhY6gVUT_e0NXt0YtWSLJxWLIJkGa7D50yN4lewwh6pruJImZNQj1IvG4j8qcr59AEUGnaFNQO8_caj-9bJfiO6b4FGwxgx-gA1gblq-VXY4eJL6GcKtdSSnJdgw-BotqF85io-SbncbKzF0Ict3R2iUUR5MO3YFhzuUm6FKLsGx-mqrxGEGn5caUbXHOUOOcBahAMN2E1jD7Mv0926y3-3-C2C3-T-Q3PwOFcu9uIwcQJ3aKJltCs3O8J5L22JorUXIwfdT1kSiYopm-7QEFfOSMBaMe_gxB2ZSuLx3POPb27JG-3OZ3SyrKdzFbjY2SQtLcCndJCRfD_T23w)






# 0. Pre-Installation

## 0.1 Acquire an Installation Image

Visit the [Download](https://www.archlinux.org/download/) page.

For example, use `wget`:

```bash
cd Downloads

wget https://mirrors.sjtug.sjtu.edu.cn/archlinux/iso/2021.05.01/archlinux-2021.05.01-x86_64.iso
```



## 0.2 Create an Arch Linux Installer USB Stick

- Linux

  ```bash
  sudo dd bs=4M if=path/to/iso of=/dev/NAME status=progress oflag=sync
  ```

  Or

  ```bash
  su -c 'cat path/to/archlinux.iso > /dev/sdx'
  ```

  Or

  ```bash
  cp path/to/archlinux.iso /dev/sdx
  tee < path/to/archlinux.iso > /dev/sdx
  ```

- Windows

  - [Rufus - The Official Website (Download, New Releases)](https://rufus.ie/)
  
    > **Note: If the USB drive does not boot properly using the default ISO Image mode, DD Image mode may be used instead.**
  
  - [USBWriter download | SourceForge.net](https://sourceforge.net/projects/usbwriter/)





## 0.3 Boot the Live Environment

> **Note:** Arch Linux installation images do not support Secure Boot. You will need to [disable Secure Boot](https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface/Secure_Boot#Disable_Secure_Boot) to boot the installation medium.

**Boot:**

1. Plug in your USB and press `F2`, `F12` or something else during the [POST](https://en.wikipedia.org/wiki/Power-on_self_test) phase.

2. Select the corresponding USB row.

3. Then you will be logged in on the first [virtual console](https://en.wikipedia.org/wiki/Virtual_console) as the root user, and presented with a [Zsh](https://wiki.archlinux.org/index.php/Zsh) shell prompt.

   > To switch to a different console, use the Alt+arrow shortcut.





## 0.4 Verify the Boot Mode

To verify the boot mode, list the [efivars](https://wiki.archlinux.org/index.php/Efivars) directory:

```bash
ls /sys/firmware/efi/efivars
```

If the command shows the directory without error, then the system is booted in UEFI mode. If the directory does not exist, the system may be booted in [BIOS](https://en.wikipedia.org/wiki/BIOS) (or [CSM](https://en.wikipedia.org/wiki/Compatibility_Support_Module)) mode. 





## 0.5 Connect to the Internet

Don't be stupid, please use Ethernet.

The connection may be verified with:

```bash
pacman -Syy
```



### Trick: Login via SSH

In ArchISO, start the SSH server.

```bash
systemctl start sshd
```



Then check that the SSH server is running.

```bash
systemctl status sshd
```



Once SSH is running, take note of your public IP address using the command below.

```bash
ip addr
```

The second interface and the first called *ens** should have your public IPv4 address. 



And you also have to set password for ArchISO.

```bash
passwd
```



You are now ready to log in to the cloud server with SSH from your computer.

```bash
ssh root@ip-address
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



Partition your hard drive (`/dev/sda` will be used in this guide) with **`fdisk`**, **`gdisk`**, or **`cfdisk`**, the partition numbers and order are at your discretion:

```bash
gdisk /dev/sda
```

**UEFI with** [GPT](https://wiki.archlinux.org/index.php/GPT)

| Mount point |           Partition           |    Partition type     |     Suggested size      |
| :---------: | :---------------------------: | :-------------------: | :---------------------: |
| `/mnt/boot` | `/dev/*efi_system_partition*` | EFI system partition  |          500 M          |
|   `/mnt`    |    `/dev/*root_partition*`    | Linux x86-64 root (/) | Remainder of the device |





## 0.8 Format the Partitions

```bash
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
```





## 0.8 Mount the File Systems

```bash
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
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
reflector -c China --save /etc/pacman.d/mirrorlist --sort rate
cat /etc/pacman.d/mirrorlist
pacman -Syy
```

If the command above does not work, edit it manually.

```bash
vim /etc/pacman.d/mirrorlist
```

`/etc/pacman.d/mirrorlist` will later be copied to the new system by `pacstrap`, so it is worth getting right.





## 1.2 Install Essential Packages

```bash
pacstrap /mnt base base-devel linux linux-firmware amd-ucode vim bash fish rsync reflector git openssh neofetch
```





## 1.3 Fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab # check
```







# 2. Configure the system

## 2.1 Change root

```bash
arch-chroot /mnt
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



### 2.2.2 Swap File

Use [dd](https://wiki.archlinux.org/index.php/Dd) to create a swap file the size of your choosing. 

For example:

```bash
dd if=/dev/zero of=/swapfile bs=1M count=512 status=progress # 512M

dd if=/dev/zero of=/swapfile bs=1G count=9 status=progress # 9G
```

Then

```bash
chmod 600 /swapfile; mkswap /swapfile; swapon /swapfile; echo "/swapfile none swap defaults 0 0" >> /etc/fstab; cat /etc/fstab # check
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
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
timedatectl set-ntp true
hwclock --systohc
date # check
```





## 2.4 Localization

**First edit `/etc/locale.gen`** and uncomment 

```bash
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

A quick way of doing so is using *sed* with the command below.

```bash
sed 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' -i /etc/locale.gen

sed 's/#zh_CN.UTF-8 UTF-8/zh_CN.UTF-8 UTF-8/' -i /etc/locale.gen
```

**Then **Generate the locales:

```
locale-gen
```

**Lastly** create the [locale.conf(5)](https://jlk.fjfi.cvut.cz/arch/manpages/man/locale.conf.5) file, and [set the LANG variable](https://wiki.archlinux.org/index.php/Locale#Setting_the_system_locale) accordingly:

```bash
LANG=en_US.UTF-8
LC_ADDRESS=en_US.UTF-8
LC_IDENTIFICATION=en_US.UTF-8
LC_MEASUREMENT=en_US.UTF-8
LC_MONETARY=en_US.UTF-8
LC_NAME=en_US.UTF-8
LC_NUMERIC=en_US.UTF-8
LC_PAPER=en_US.UTF-8
LC_TELEPHONE=en_US.UTF-8
LC_TIME=en_US.UTF-8
```





## 2.5 Network Configuration

**First** create your hostname:

```bash
echo "ArchLinux-VM" > /etc/hostname
```

**Then** add matching entries to [hosts(5)](https://jlk.fjfi.cvut.cz/arch/manpages/man/hosts.5):

```bash
cat > /etc/hosts << EOF
127.0.0.1    localhost
::1          localhost
127.0.1.1    ArchLinux-VM.localdomain ArchLinux-VM
EOF
```



### Server

The easiest way to configure networking on a server is through DHCP.

```bash
pacman -S dhcpcd
```

Make DHCP automatically run at boot.

```bash
systemctl enable systemd-networkd
systemctl enable dhcpcd
```

Make DNS resolution automatically run at boot.

```bash
systemctl enable systemd-resolved
```



#### ssh

By default, root login via ssh is not enabled. We need to change some settings inside the `sshd_config` file to enable root login.

Make a backup copy of the `sshd_config` file. You should be able to use tab auto-completion to make this easier on the web console.

```bash
cp /etc/ssh/sshd_config /etc/ssh/sshd_conf.bak
```

Edit  the `/etc/ssh/sshd_config` file. Find the following lines and make some changes to it.

```bash
vim /etc/ssh/sshd_config
```

```bash
#PermitRootLogin prohibit-password
PermitRootLogin yes
PasswordAuthentication yes
```

Then finally enable the SSH daemon on the console.

```bash
systemctl enable sshd
```







### Desktop

**Install network manager:**

```bash
pacman -S networkmanager network-manager-applet wpa_supplicant dialog 

systemctl enable NetworkManager
```







## 2.6 Set Root Password

```bash
passwd
```





## 2.7 Boot Loader

```bash
pacman -S grub efibootmgr dosfstools mtools ntfs-3g
pacman -S os-prober # for detecting other os
```

```bash
grub-install --target=x86_64-efi --efi-directory=/boot/ --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```



By default at boot, grub will wait for 5 seconds before choosing the default option. To disable this wait, use the following.

```bash
sed 's/^GRUB_TIMEOUT=5$/GRUB_TIMEOUT=1/' -i /etc/default/grub
```

**Note**: *If you still want access to the grub boot menu, you might want to set this to 1 second instead of 0.*



By default, grub gives the kernel the `quiet` option which `systemd` also follows. Use the following to show startup and shutdown messages.

```bash
sed 's/^GRUB_CMDLINE_LINUX_DEFAULT="quiet"$/GRUB_CMDLINE_LINUX_DEFAULT=""/' -i /etc/default/grub
```



Create the grub configuration.

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```







## 2.8 Setting up a new user and SSH authentication

You should now be connected to your new Arch server over SSH and the basic install is complete, but we are not quite done. 

For improved security and convenience, you should set up a new username for yourself and configure SSH keys to it.



Start by creating a new unprivileged username using the command below. Name the account as you see fit.

```bash
useradd -mG wheel lucas
```

Then set a password for the new user.

```bash
passwd lucas
```

Allow members in group `wheel` to use `sudo`.

```bash
cp /etc/sudoers /etc/sudoers.new

sed 's/# %wheel ALL=(ALL) ALL/%wheel ALL=(ALL) ALL/' -i /etc/sudoers.new

visudo -c -f /etc/sudoers.new && mv /etc/sudoers.new /etc/sudoers
```

Now, SSH daemon is already running **but the settings allow root to log in which can be insecure.** Revert the changes made to the sshd_config file by swapping it to the default config we backed up earlier.

```bash
cp /etc/ssh/sshd_conf.bak /etc/ssh/sshd_config
```

Then, restart the SSH daemon to apply the new configuration.

```bash
systemctl restart sshd
```



Next, generate an SSH key pair for your regular user **on your own computer.** You should preferably also secure the key with a password. On Linux systems, this can be achieved with the following command.

```bash
ssh-keygen
```

Once you have created a new SSH key pair, copy over the public part to your Arch Linux server. By default, the public SSH key is saved to `/home/username/.ssh/id_rsa.pub`.

Then on the cloud server, change into your new user account and create the following directory.

```bash
su lucas
mkdir ~/.ssh
```

The public SSH key should be saved to file at `/home/username/.ssh/authorized_keys`. Open a new file in a text editor, for example, by using `nano` and copy the public SSH key into this file.

```
nano ~/.ssh/authorized_keys
```

Then save the file and exit the editor.

Next, switch back to the root user by simply exiting your new username.

```
exit
```

You can then edit the `/etc/ssh/sshd_config` file according to your needs.

```
nano /etc/ssh/sshd_config
```

Here are some examples of changes you might wish to make:

- Disable the *sftp* subsystem if not needed by commenting out the line
- Disable password login by setting *PasswordAuthentication no*
- Speed up the login by setting SSH to only use IPv4 with *AddressFamily inet*

You can find more information about the available configuration options at the [ArchWiki](https://wiki.archlinux.org/index.php/OpenSSH#Protection).

After altering the configuration, restart the SSH daemon again as the *root* user.

```bash
systemctl restart sshd
```

Then test logging in by opening a new terminal window on your own machine, and connecting with the username you created earlier.

```bash
ssh lucas@ip-address
```





> ## Testing notes
>
> 
>
> After rebooting the system via SSH connection with `reboot` as the root user, **it might take a minute until the server accepts SSH connections again.** Booting itself is really fast, but the SSH daemon can take time to fully start.
>
> If SSH fails to connect after rebooting, logging in as any user via the web browser console connection should solve the issue and allow you to connect using SSH.





## 2.9 Configure time synchronization

For a lightweight time synchronization client with rough accuracy use the following.

```bash
systemctl enable --now systemd-timesyncd
```

If you would prefer better accuracy.

```bash
pacman -S ntp
systemctl enable --now ntpd
```





## Reboot
