# Display

## Multiple Monitors

```bash
sudo pacman -S arandr
```





## Fix Screen Tearing

- **Solution 1**

  Create `/etc/X11/xorg.conf.d/20-amdgpu.conf`:

  ```bash
  Section "Device"
  	Identifier "AMD"
  	Driver "amdgpu" 
  	Option "TearFree" "true"
  EndSection
  ```

- **Solution 2**

  You can also enable TearFree temporarily with [xrandr](https://wiki.archlinux.org/index.php/Xrandr):

  ```bash
  xrandr --output output --set TearFree on
  ```

  Where `*output*` should look like `DisplayPort-0` or `HDMI-A-0` and can be acquired by running `xrandr -q`.







# Font

```bash
sudo pacman -S noto-fonts noto-fonts-emoji noto-fonts-cjk
yay -S font-manager
```

Create `~/.config/fontconfig/fonts.conf`:

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Noto Sans</family>
      <family>Noto Sans CJK SC</family>
      <family>Noto Sans CJK TC</family>
      <family>Noto Sans CJK JP</family>
    </prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Noto Sans Mono</family>
      <family>Noto Sans Mono CJK SC</family>
      <family>Noto Sans Mono CJK TC</family>
      <family>Noto Sans Mono CJK JP</family>
    </prefer>
  </alias>
</fontconfig>
```











# 2. Basic Packages

## 2.1 Editor

### 2.1.1 Sublime Text

```bash
yay -S sublime-text-dev
```

**License Key:**

```
----- BEGIN LICENSE -----
Member J2TeaM
Single User License
EA7E-1011316
D7DA350E 1B8B0760 972F8B60 F3E64036
B9B4E234 F356F38F 0AD1E3B7 0E9C5FAD
FA0A2ABE 25F65BD8 D51458E5 3923CE80
87428428 79079A01 AA69F319 A1AF29A4
A684C2DC 0B1583D4 19CBD290 217618CD
5653E0A0 BACE3948 BB2EE45E 422D2C87
DD9AF44B 99C49590 D2DBDEE1 75860FD2
8C8BB2AD B2ECE5A4 EFC08AF2 25A9B864
------ END LICENSE ------
```



### 2.1.2 i3wm Syntax Highlight

https://github.com/dcasella/i3wm-syntax

**Add this Repository:**

- `Ctrl/Command+Shift+P` to open the Command Palette
- Select `Package Control: Add Repository`
- Insert the URL `https://github.com/dcasella/i3wm-syntax`
- Press `Enter`
- `Ctrl/Command+Shift+P` to open the Command Palette
- Select `Package Control: Install Package`
- Search for `i3`
- Press `Enter`











## 2.2 Terminal

### 2.2.1 xfce4-terminal

```bash
sudo pacman -S xfce4-terminal
```

- Drop down mode

  ```bash
  xfce4-terminal --drop-down
  ```





## 2.3 App Launcher

### 2.3.1 dmenu

```bash
sudo pacman -S dmenu
```



### 2.3.2 rofi

```bash
sudo pacman -S rofi
```



### 2.3.3 gmrun

```bash
sudo pacman -S gmrun
```



### 2.3.4 xfce4-appfinder 

```bash
sudo pacman -S xfce4-appfinder
```





## 2.4 File Browser

```bash
sudo pacman -S pcmanfm file-roller p7zip unrar
sudo pacman -S ranger
```





## 2.5 Web Browser: chromium

```bash
sudo pacman -S chromium
```





## 2.6 Web Proxy

### 2.6.1 SwitchyOmega

- https://github.com/FelisCatus/SwitchyOmega/releases
- https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList
- https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt



### 2.6.2 qv2ray

```bash
sudo pacman -S v2ray qv2ray-dev-git
```

**Bug:**

![image-20200926114623440](Arch-Advanced-Configuration-Guide.assets/image-20200926114623440.png)

**Solution:**

Create `~/.config/qv2ray/init.sh `:

```bash
#!/bin/sh

killall v2ray &
sleep 2
exec qv2ray
```

Then

```bash
chmod u+x ~/.config/qv2ray/init.sh
```





## 2.7 Input Method: Fcitx

```bash
sudo pacman -S fcitx fcitx-configtool fcitx-qt5 fcitx-cloudpinyin
yay -S fcitx-sogoupinyin
```

And create `~/.pam_environment`:

```bash
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
```

And run

```bash
fcitx-autostart
fcitx-configtool
```

Then reboot.





## 2.8 Wallpaper

```bash
sudo pacman -S nitrogen
```





## 2.9 GTK Themes

```bash
sudo pacman -S lxappearance # theme manager

# gtk-theme
yay -S matcha-gtk-theme 

# icon-theme
sudo pacman -S papirus-icon-theme

# cursor-theme
sudo pacman -S bibata-cursor-theme
```

**Fix cursor size:**

- Edit `~/.Xresources`

  ```bash
  Xcursor.size: 24
  ```






## [TODO] 2.10 QT Themes

```bash
sudo pacman -S qt5ct kvantummanager
```

Edit `~/.profile`:

```bash
export QT_QPA_PLATFORMTHEME=qt5ct
```







# 3. Advance Packages

## 3.1 Image

### 3.1.2 Image Viewer

```bash
sudo pacman -S gpicview gthumb
```





## 3.2 PDF

```bash
sudo pacman -S okular

# sudo pacman -S simple-scan
```





## 3.3 Tools for Learning

### 3.3.1 Markdown

```bash
sudo pacman -S typora
```



### 3.3.2 Anki

```bash
sudo pacman -S anki mplayer mpv
```



### 3.3.3 Dictionary

```bash
sudo pacman -S goldendict hunspell hunspell-en_US
```

- https://freemdict.com/category/%e8%8b%b1%e8%af%ad/





## 3.4 Virtual Machine

### 3.4.1 Vmware

**First install vmware,**

```bash
sudo pacman -S vmware-workstation
```

**Then,** as desired, enable some of the following services:

- `vmware-networks.service` for guest network access
- `vmware-usbarbitrator.service` for connecting USB devices to guest
- `vmware-hostd.service` for sharing virtual machines

**And** load the VMware modules:

```bash
sudo modprobe -a vmw_vmci vmmon
```

**Lastly,** entering the Workstation Pro license key from a terminal:

```bash
sudo /usr/lib/vmware/bin/vmware-vmx-debug --new-sn XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
```

- If the above does not work, you can try:

  ```
  sudo /usr/lib/vmware/bin/vmware-enter-serial
  ```



#### 16.0 License Key

```bash
ZF3R0-FHED2-M80TY-8QYGC-NPKYF
YF390-0HF8P-M81RQ-2DXQE-M2UT6
ZF71R-DMX85-08DQY-8YMNC-PPHV8
```



#### **Fix `no 3D acceleration`**

`vim ~/.vmware/preferences`

```bash
mks.gl.allowBlacklistedDrivers = "TRUE"
```





## 3.5 System Information

```bash
sudo pacman -S htop gnome-system-monitor 
sudo pacman -S neofetch baobab hardinfo
sudo pacman -S mugshot
```





## 3.6 Clock

```bash
sudo pacman -S gnome-clocks
```





## 3.7 Music Player

```bash
sudo pacman -S netease-cloud-music playerctl
```





## 3.8 Screen Shot

```bash
sudo pacman -S flameshot
```





## 3.9 File Searcher

```bash
# sudo pacman -S catfish
```





## 3.10 Print

```bash
sudo pacman -S cups cups-pdf cups-pk-helper splix
sudo pacman -S system-config-printer

sudo systemctl enable org.cups.cupsd.service
```





## 3.11 Disk

```bash
# sudo pacman -S gnome-disk-utility
```





## 3.12 Bluetooth

```bash
# sudo pacman -S bluez blueman blueberry
```





## 3.13 Capture

```bash
sudo pacman -S simplescreenrecorder
sudo pacman -S peek # gif
sudo pacman -S guvcview cheese

sudo pacman -S screenkey
```





## 3.14 Download

```bash
sudo pacman -S uget aria2
sudo pacman -S qbittorrent
```





## 3.15 Stream

```bash
sudo pacman -S obs-studio
```







# 4. System Configuration

## 4.1 GRUB

**Theme**

```bash
yay -S arch-silence-grub-theme-git
```

> ==> To select the arch-silence theme you must:
> ==>  1. Open the file "`/etc/default/grub`" and inside it update the variable GRUB_THEME="`/boot/grub/themes/arch-silence/theme.txt`"
> ==>  2. Update the grub config by running "`sudo grub-mkconfig -o /boot/grub/grub.cfg`"



**Timeout**

Edit `/etc/default/grub`.

```bash
GRUB_TIMEOUT=2
```



**Alias**

```bash
alias update-grub=‘sudo grub-mkconfig -o /boot/grub/grub.cfg
```





## 4.2 Systemd

Edit ` /etc/systemd/system.conf`:

```bash
RebootWatchdogSec=3s
ShutdownWatchd1ogSec=3s
DefaultTimeoutStartSec=3s
DefaultTimeoutStopSec=3s
```





## 4.3 Pamac Manager

```bash
sudo pacman -S pamac-aur
```





## 4.4 Shell

### 4.4.1 Fish

```bash
sudo pacman -S fish
yay -S fisher

set -U fish_greeting ""

fish_config
```



**Plugins**

> - [awesome.fish](https://github.com/jorgebucaran/awesome.fish)

- #### [z](https://github.com/jethrokuan/z)

  ```bash
  fisher add jethrokuan/z
  ```

- [Bax](https://github.com/jorgebucaran/bax.fish)

  ```bash
  fisher add jorgebucaran/bax.fish
  ```

- [GitNow](https://github.com/joseluisq/gitnow)

  ```bash
  fisher add joseluisq/gitnow@2.5.0
  ```

- [fish-abbreviation-tips](https://github.com/Gazorby/fish-abbreviation-tips)

  ```bash
  fisher add Gazorby/fish-abbreviation-tips
  ```







### 4.4.2 ZSH

- Oh-my-zsh

  ```bash
  sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```

- Syntax highlight

  ```bash
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```

- Autojump

  ```bash
  sudo pacman -S autojump
  source /etc/profile.d/autojump.sh
  ```

- zsh-autosuggestions

  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```

  











## 4.5 Power Manager

```bash
sudo pacamn -S 
```









## 4.6 [Lenevo Xiaoxin Air 15 2020] Fix Touch Pad Not Working









## 4.7 Disable System Beep Alert

```bash
xset -b
```







## 4.8 Partition

```bash
sudo pacman -S gparted
```

