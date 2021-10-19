# Basic


## File Browser

```bash
sudo pacman -S thunar file-roller p7zip unrar
```



### zip 压缩包乱码

> 避免方法：非 utf8 编码环境下（一般 windows 下的中文环境即是）不使用 zip 进行压缩（建议使用 [7z](https://wiki.archlinux.org/index.php/7z_(简体中文)))。 解决方案：安装使用 [unzip-iconv](https://aur.archlinux.org/packages/unzip-iconv/)AUR 或者 [unzip-natspec](https://aur.archlinux.org/packages/unzip-natspec/)AUR 取代原版的 [unzip](https://archlinux.org/packages/?name=unzip) 来解压缩，示例：
>
> ```
> $ unzip -O gbk file.zip
> ```
>
> `file.zip` 是压缩文件，`gbk` 是该文件的编码格式，以 `-O` 指定（原版 unzip 无 `-O` 选项）。

```bash
yay -S unzip-natspec p7zip-natspec
```

Then

```bash
unzip -O gbk file.zip
```





## Downloader

### BaiduNetdisk

```bash
yay -S baidunetdisk-bin
sudo ln -s /usr/lib/baidunetdisk/baidunetdisk /usr/bin/baidunetdisk
```

Tip: Change zoom level with `Ctrl+Shift+=` or `Ctrl+Shift+-`.



### Xunlei

```bash
yay -S xunlei-bin
```





## Image

### Viewer

```bash
sudo pacman -S feh imagemagick
```



### Editor

#### Gimp

```bash
sudo pacman -S gimp
```







## PDF

### General

```bash
sudo pacman -S okular
```



### Editor

```bash
sudo pacman -S masterpdfeditor
```







## Clock

```bash
sudo pacman -S gnome-clocks
```





## Printer

```bash
sudo pacman -S cups hplip 
sudo pacman -S system-config-printer cups-pdf cups-pk-helper gutenprint splix foomatic-db
sudo systemctl enable cups
```







## Bluetooth

```bash
sudo pacman -S bluez bluez-utils
sudo systemctl enable bluetooth
```











## Office

### LibreOffice

```bash
yay -S libreoffice-fresh libreoffice-fresh-zh-cn
```



> The Document Foundation [wiki](https://wiki.documentfoundation.org/Fonts) mentions various fonts that are packaged by default with LibreOffice on Windows and macOS. 
>
> Also see [Fonts#Font packages](https://wiki.archlinux.org/index.php/Fonts#Font_packages).

```bash
yay -S ttf-caladea ttf-carlito ttf-dejavu ttf-gentium-basic ttf-liberation ttf-linux-libertine-g noto-fonts adobe-source-code-pro-fonts adobe-source-sans-pro-fonts adobe-source-serif-pro-fonts
```









### WPS

```bash
# yay -S wps-office-cn ttf-wps-fonts wps-office-mine-cn wps-office-mui-zh-cn
```













# System Management

## Partition

```bash
sudo pacman -S gparted
```





## System Information

```bash
sudo pacman -S htop
sudo pacman -S neofetch 
sudo pacman -S hardinfo 
sudo pacman -S baobab # display disk usage in graph
```









# Multi Media

## Music Player

```bash
# sudo pacman -S netease-cloud-music
sudo pacman -S netease-cloud-music-gtk
```





## Video Player

**THE BEST VIDEO PLAYER EVER: MPV**

```bash
sudo pacman -S mpv
```



Others

```bash
sudo pacman -S mplayer vlc
```





## Capture

```bash
sudo pacman -S simplescreenrecorder # record
sudo pacman -S obs-studio # record and stream
sudo pacman -S peek # gif
sudo pacman -S guvcview cheese # camera
sudo pacman -S screenkey # print the keys on the screen you entered 
sudo pacman -S flameshot # screenshot
```









# Display

## Multiple Monitors

```bash
sudo pacman -S arandr
```





## Fix Screen Tearing

### Create `/etc/X11/xorg.conf.d/20-amdgpu.conf`:

```bash
Section "Device"
	Identifier "AMD"
	Driver "amdgpu" 
	Option "TearFree" "true"
EndSection
```



### Picom

Try this setting in `picom.conf`:

```
vsync = true;
```





## Wallpaper

```bash
sudo pacman -S nitrogen
```





## GTK Themes

```bash
sudo pacman -S lxappearance # theme manager

# gtk-theme
yay -S arc-gtk-theme dracula-gtk-theme

# icon-theme
sudo pacman -S papirus-icon-theme 
yay -S surfn-icons-git

# cursor-theme
sudo pacman -S bibata-cursor-theme
```






## QT Themes

```bash
sudo pacman -S qt5ct
# sudo pacman -S kvantummanager
```

Edit `~/.xinitrc` OR `~/.xprofile`:

```bash
export QT_QPA_PLATFORMTHEME=qt5ct
```





## Picom

```bash
#################################
#
# Backend
#
#################################

# Backend to use: "xrender" or "glx".
# GLX backend is typically much faster but depends on a sane driver.
backend = "glx";
#backend = "xrender"

#################################
#
# GLX backend
#
#################################

glx-no-stencil = true;

# GLX backend: Copy unmodified regions from front buffer instead of redrawing them all.
# My tests with nvidia-drivers show a 10% decrease in performance when the whole screen is modified,
# but a 20% increase when only 1/4 is.
# My tests on nouveau show terrible slowdown.
glx-copy-from-front = false;

# GLX backend: Use MESA_copy_sub_buffer to do partial screen update.
# My tests on nouveau shows a 200% performance boost when only 1/4 of the screen is updated.
# May break VSync and is not available on some drivers.
# Overrides --glx-copy-from-front.
# glx-use-copysubbuffermesa = true;

# GLX backend: Avoid rebinding pixmap on window damage.
# Probably could improve performance on rapid window content changes, but is known to break things on some drivers (LLVMpipe).
# Recommended if it works.
# glx-no-rebind-pixmap = true;

# GLX backend: GLX buffer swap method we assume.
# Could be undefined (0), copy (1), exchange (2), 3-6, or buffer-age (-1).
# undefined is the slowest and the safest, and the default value.
# copy is fastest, but may fail on some drivers,
# 2-6 are gradually slower but safer (6 is still faster than 0).
# Usually, double buffer means 2, triple buffer means 3.
# buffer-age means auto-detect using GLX_EXT_buffer_age, supported by some drivers.
# Useless with --glx-use-copysubbuffermesa.
# Partially breaks --resize-damage.
# Defaults to undefined.
#glx-swap-method = "undefined";

#################################
#
# Shadows
#
#################################

# Enabled client-side shadows on windows.
shadow = true;
# The blur radius for shadows. (default 12)
shadow-radius = 5;
# The left offset for shadows. (default -15)
shadow-offset-x = -5;
# The top offset for shadows. (default -15)
shadow-offset-y = -5;
# The translucency for shadows. (default .75)
shadow-opacity = 0.5;

log-level = "warn";
#change your username here
#log-file = "/home/erik/.config/compton.log";

# Set if you want different colour shadows
# shadow-red = 0.0;
# shadow-green = 0.0;
# shadow-blue = 0.0;

# The shadow exclude options are helpful if you have shadows enabled. Due to the way compton draws its shadows, certain applications will have visual glitches
# (most applications are fine, only apps that do weird things with xshapes or argb are affected).
# This list includes all the affected apps I found in my testing. The "! name~=''" part excludes shadows on any "Unknown" windows, this prevents a visual glitch with the XFWM alt tab switcher.
shadow-exclude = [
    "name = 'Notification'",
    "name = 'Plank'",
    "name = 'Docky'",
    "name = 'Kupfer'",
    "name = 'xfce4-notifyd'",
    "name *= 'VLC'",
    "name *= 'compton'",
    "name *= 'picom'",
    "name *= 'Chromium'",
    "name *= 'Chrome'",
    "class_g = 'Firefox' && argb",
    "class_g = 'Conky'",
    "class_g = 'Kupfer'",
    "class_g = 'Synapse'",
    "class_g ?= 'Notify-osd'",
    "class_g ?= 'Cairo-dock'",
    "class_g = 'Cairo-clock'",
    "class_g ?= 'Xfce4-notifyd'",
    "class_g ?= 'Xfce4-power-manager'",
    "_GTK_FRAME_EXTENTS@:c",
    "_NET_WM_STATE@:32a *= '_NET_WM_STATE_HIDDEN'"
];
# Avoid drawing shadow on all shaped windows (see also: --detect-rounded-corners)
shadow-ignore-shaped = false;

#################################
#
# Opacity
#
#################################

inactive-opacity = 1;
active-opacity = 1;
frame-opacity = 1;
inactive-opacity-override = false;

# Dim inactive windows. (0.0 - 1.0)
# inactive-dim = 0.2;
# Do not let dimness adjust based on window opacity.
# inactive-dim-fixed = true;
# Blur background of transparent windows. Bad performance with X Render backend. GLX backend is preferred.
# blur-background = true;
# Blur background of opaque windows with transparent frames as well.
# blur-background-frame = true;
# Do not let blur radius adjust based on window opacity.
blur-background-fixed = false;
blur-background-exclude = [
    "window_type = 'dock'",
    "window_type = 'desktop'",
    "_GTK_FRAME_EXTENTS@:c"
];

#################################
#
# Fading
#
#################################

# Fade windows during opacity changes.
fading = true;
# The time between steps in a fade in milliseconds. (default 10).
fade-delta = 4;
# Opacity change between steps while fading in. (default 0.028).
fade-in-step = 0.03;
# Opacity change between steps while fading out. (default 0.03).
fade-out-step = 0.03;
# Fade windows in/out when opening/closing
# no-fading-openclose = true;

# Specify a list of conditions of windows that should not be faded.
fade-exclude = [ ];

#################################
#
# Other
#
#################################

# Try to detect WM windows and mark them as active.
mark-wmwin-focused = true;
# Mark all non-WM but override-redirect windows active (e.g. menus).
mark-ovredir-focused = true;
# Use EWMH _NET_WM_ACTIVE_WINDOW to determine which window is focused instead of using FocusIn/Out events.
# Usually more reliable but depends on a EWMH-compliant WM.
use-ewmh-active-win = true;
# Detect rounded corners and treat them as rectangular when --shadow-ignore-shaped is on.
detect-rounded-corners = true;

# Detect _NET_WM_OPACITY on client windows, useful for window managers not passing _NET_WM_OPACITY of client windows to frame windows.
# This prevents opacity being ignored for some apps.
# For example without this enabled my xfce4-notifyd is 100% opacity no matter what.
detect-client-opacity = true;

# Specify refresh rate of the screen.
# If not specified or 0, picom will try detecting this with X RandR extension.
refresh-rate = 0;

# Vertical synchronization: match the refresh rate of the monitor
# this breaks transparency in virtualbox - put a "#" before next line to fix that
vsync = true;

# Enable DBE painting mode, intended to use with VSync to (hopefully) eliminate tearing.
# Reported to have no effect, though.
dbe = false;

# Limit picom to repaint at most once every 1 / refresh_rate second to boost performance.
# This should not be used with --vsync drm/opengl/opengl-oml as they essentially does --sw-opti's job already,
# unless you wish to specify a lower refresh rate than the actual value.
#sw-opti = true;

# Unredirect all windows if a full-screen opaque window is detected, to maximize performance for full-screen windows, like games.
# Known to cause flickering when redirecting/unredirecting windows.
unredir-if-possible = false;

# Specify a list of conditions of windows that should always be considered focused.
focus-exclude = [ ];

# Use WM_TRANSIENT_FOR to group windows, and consider windows in the same group focused at the same time.
detect-transient = true;
# Use WM_CLIENT_LEADER to group windows, and consider windows in the same group focused at the same time.
# WM_TRANSIENT_FOR has higher priority if --detect-transient is enabled, too.
detect-client-leader = true;

#################################
#
# Window type settings
#
#################################

wintypes:
{
  tooltip = { fade = true; shadow = true; opacity = 0.9; focus = true;};
  dock = { shadow = false; }
  dnd = { shadow = false; }
  popup_menu = { opacity = 0.9; }
  dropdown_menu = { opacity = 0.9; }
};

######################
#
# XSync
# See: https://github.com/yshui/compton/commit/b18d46bcbdc35a3b5620d817dd46fbc76485c20d
#
######################

# Use X Sync fence to sync clients' draw calls. Needed on nvidia-drivers with GLX backend for some users.
xrender-sync-fence = true;
```









# Font

```bash
yay -S noto-fonts noto-fonts-emoji noto-fonts-cjk 
yay -S ttf-dejavu ttf-liberation ttf-symbola 
yay -S wqy-microhei wqy-microhei-lite adobe-source-han-sans-otc-fonts adobe-source-han-serif-otc-fonts adobe-source-han-serif-cn-fonts adobe-source-han-sans-cn-fonts wqy-zenhei wqy-bitmapfont ttf-arphic-ukai 
yay -S font-manager
```





## Fontconfig

**Template:**

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>

  settings go here -->

</fontconfig>
```





## Fix Displaying Japanese Characters While `Noto Sans CJK` Installed

```xml
<!-- ~/.config/fontconfig/fonts.conf -->

<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
	<!-- Set preferred serif, sans serif, and monospace fonts. -->
    <alias>
        <family>sans-serif</family>
        <prefer>
            <family>Noto Sans</family>
            <family>Noto Sans CJK SC</family>
            <family>Noto Sans CJK TC</family>
            <family>Noto Sans CJK JP</family>
            <family>Noto Sans CJK KR</family>
            <family>Droid Sans</family>
        </prefer>
    </alias>
    <alias>
        <family>serif</family>
        <prefer>
            <family>Noto Serif</family>
            <family>Noto Serif CJK SC</family>
            <family>Noto Serif CJK TC</family>
            <family>Noto Serif CJK JP</family>
            <family>Noto Serif CJK KR</family>
            <family>Droid Serif</family>
        </prefer>
    </alias>
    <alias>
        <family>monospace</family>
        <prefer>
            <family>Noto Sans Mono</family>
            <family>Noto Sans Mono CJK SC</family>
            <family>Noto Sans Mono CJK TC</family>
            <family>Noto Sans Mono CJK JP</family>
            <family>Noto Sans Mono CJK KR</family>
            <family>Droid Sans Mono</family>
        </prefer>
    </alias>
    <alias>
        <family>mono</family>
        <prefer>
            <family>Noto Sans Mono</family>
            <family>Noto Sans Mono CJK SC</family>
            <family>Noto Sans Mono CJK TC</family>
            <family>Noto Sans Mono CJK JP</family>
            <family>Noto Sans Mono CJK KR</family>
            <family>Droid Sans Mono</family>
        </prefer>
    </alias>
</fontconfig>
```







# Proxy

## Chromium

- Download `SwitchyOmega_Chromium.crx` from [SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega) and rename it to `SwitchyOmega_Chromium.zip`.

    ```bash
    wget https://github.com/FelisCatus/SwitchyOmega/releases/download/v2.5.20/SwitchyOmega_Chromium.crx
    ```

- Go to chrome://extensions/ and enable Developer Mode.

- Then put `SwitchyOmega_Chromium.zip` into there.

    - Autoproxy: https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt





## Clash

```bash
sudo pacman -S clash

cd .config/clash

wget -O config.yaml https://d.cloudso.club/link/????????????clash=1&log-level=info

clash -d ~/.config/clash
```







## V2ray

```bash
sudo pacman -S v2ray
```



### NO GUI

My workflow:

- First export client side config file from `v2rayN` or `qv2ray`.

  - socks and http

    ```json
        "inbounds": [
            {
                "tag": "proxy",
                "port": 1081,
                "listen": "127.0.0.1",
                "protocol": "socks",
                "sniffing": {
                    "enabled": true,
                    "destOverride": [
                        "http",
                        "tls"
                    ]
                },
                "settings": {
                    "auth": "noauth",
                    "udp": true,
                    "ip": null,
                    "address": null,
                    "clients": null,
                    "decryption": null
                },
                "streamSettings": null
            },
            {
                "tag": "proxy",
                "port": 1082,
                "listen": "127.0.0.1",
                "protocol": "http",
                "sniffing": {
                    "enabled": true,
                    "destOverride": [
                        "http",
                        "tls"
                    ]
                },
                "settings": {
                    "auth": "noauth",
                    "udp": false
                }
            }
        ],
    ```

- Then upload the config file.

  And check the config file.

  ```bash
  v2ray -test config.json
  ```

- Then

  ```bash
  sudo cp /etc/v2ray/config.json /etc/v2ray/config.json.bak
  sudo cp config.json /etc/v2ray/config.json
  sudo systemctl enable v2ray 
  sudo systemctl start v2ray
  ```

- Now test it.

  ```bash
  lucas@arch ~/Downloads> export http_proxy=http://127.0.0.1:1082/; export https_proxy=$http_proxy
  ```

  Note that your proxy port might be different.

  ```bash
  lucas@arch ~/Downloads> wget google.com
  --2021-02-05 00:43:55--  http://google.com/
  Connecting to 127.0.0.1:1082... connected.
  Proxy request sent, awaiting response... 301 Moved Permanently
  Location: http://www.google.com/ [following]
  --2021-02-05 00:43:56--  http://www.google.com/
  Reusing existing connection to 127.0.0.1:1082.
  Proxy request sent, awaiting response... 200 OK
  Length: unspecified [text/html]
  Saving to: ‘index.html’
  
  index.html                                 [ <=>                                                                         ]  13.79K  --.-KB/s    in 0.009s
  
  2021-02-05 00:43:56 (1.48 MB/s) - ‘index.html’ saved [14117]
  ```

  





### GUI

```bash
sudo pacman -S qv2ray
```

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







## Command line proxy

### By proxychains

```bash
sudo pacman -S proxychains
```

Edit `/etc/proxychains.conf`:

```bash
socks5 127.0.0.1 1080
```

> These "127.0.0.1 1080" stuff depends on your own proxy softwares' setting.
>
> Like:
>
> ![image-20210125211926100]([03]Arch-Desktop-Configuration-Guide.assets/image-20210125211926100.png)

Then, `proxychains-ng` can be launched with

```bash
proxychains program
```

- You can even proxy `pacman`, like this

  ```bash
  su
  proxychains pacman -Syyu
  ```






### By setting environment variables

In terminal:

```bash
export http_proxy=http://127.0.0.1:2080/; export https_proxy=$http_proxy
```

![image-20210125212255597]([03]Arch-Desktop-Configuration-Guide.assets/image-20210125212255597.png)







# Coding and Learning

## Editor: Sublime Text

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



### i3wm Syntax Highlight

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





## Terminal

### xfce4-terminal

```bash
sudo pacman -S xfce4-terminal
```

- Drop down mode

  ```bash
  xfce4-terminal --drop-down
  ```





## Markdown

```bash
sudo pacman -S typora
```

![image-20210125085145323]([03]Arch-Desktop-Configuration-Guide.assets/image-20210125085145323.png)





## Xmind

```bash
yay -S xmind-2020
```

Crack: https://ghpym.lanzous.com/b00zd6odc

> Windows crack file works in linux though.

```bash
lucas@arch ~/R/I/W/XmindZen> 7z x XMind_2020_10.3.1_Linux_补丁.7z 

lucas@arch ~/R/I/W/XmindZen> cd XMind_2020_10.3.1_Linux_补丁/
lucas@arch ~/R/I/W/X/XMind_2020_10.3.1_Linux_补丁> ls
app.asar  使用说明.txt

lucas@arch ~/R/I/W/X/XMind_2020_10.3.1_Linux_补丁> 
sudo mv /opt/XMind/resources/app.asar /opt/XMind/resources/app.asar.bak
lucas@arch ~/R/I/W/X/XMind_2020_10.3.1_Linux_补丁> 
sudo cp app.asar /opt/XMind/resources/app.asar 
```







## App Launcher

```bash
sudo pacman -S rofi # run rofi-theme-selector to select theme
sudo pacman -S gmrun
sudo pacman -S xfce4-appfinder
```





## Anki

```bash
sudo pacman -S anki 
```





## Dictionary

```bash
sudo pacman -S goldendict hunspell hunspell-en_US
```

- https://freemdict.com/category/%e8%8b%b1%e8%af%ad/



​	



# Virtual Machine

## Vmware

> - [VMware - ArchWiki](https://wiki.archlinux.org/title/VMware)
> - [VMware/Install Arch Linux as a guest](https://wiki.archlinux.org/title/VMware/Install_Arch_Linux_as_a_guest)



**First install vmware,**

```bash
sudo pacman -S vmware-workstation
```

**Then,** as desired, enable some of the following services:

- `vmware-networks.service` for guest network access
- `vmware-usbarbitrator.service` for connecting USB devices to guest

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



### 16.0 License Key

```bash
ZF3R0-FHED2-M80TY-8QYGC-NPKYF
YF390-0HF8P-M81RQ-2DXQE-M2UT6
ZF71R-DMX85-08DQY-8YMNC-PPHV8
```



### **Fix `no 3D acceleration`**

`vim ~/.vmware/preferences`

```bash
mks.gl.allowBlacklistedDrivers = "TRUE"
```





### Arch Linux as a guest

If you are installing Arch Linux as a virtual machine,

```bash
sudo pacman -S open-vm-tools

# Service responsible for the Virtual Machine status report.
sudo systemctl enable vmtoolsd.service 

# Filesystem utility. Enables drag & drop functionality between host and guest through FUSE (Filesystem in Userspace).
sudo systemctl enable vmware-vmblock-fuse.service 
```

For more, read https://wiki.archlinux.org/title/VMware/Install_Arch_Linux_as_a_guest#Open-VM-Tools



#### Fix `drag and drop` and `copy and paste` not working

Try running:

```bash
vmware-user # Tool to enable clipboard sharing (copy/paste) between host and guest.
vmware-vmblock-fuse # Filesystem utility. Enables drag & drop functionality between host and guest through FUSE
```

To make this permanent

```bash
echo "vmware-user &" >> ~/.xprofile
echo "vmware-vmblock-fuse &" >> ~/.xprofile
```





#### Setup Shared Folder

- `vmhgfs-fuse` - Utility for mounting vmhgfs shared folders.

Share a folder by selecting *Edit virtual machine settings > Options > Shared Folders > Always enabled*, and creating a new share.

The shared folders should be visible with:

```bash
vmware-hgfsclient
```

Now the folder can be mounted:

```bash
mkdir <shared folders root directory>
vmhgfs-fuse -o allow_other -o auto_unmount .host:/<shared_folder> <shared folders root directory>
```

Example:

```bash
mkdir $HOME/SHARED
vmhgfs-fuse -o allow_other -o auto_unmount .host:/D $HOME/SHARED
```



##### fstab

Add a rule for each share:

```bash
sudo echo '.host:/<shared_folder> <shared folders root directory> fuse.vmhgfs-fuse nofail,allow_other 0 0' >> /etc/fstab
```

Example:

```bash
sudo echo '.host:/D $HOME/SHARED fuse.vmhgfs-fuse nofail,allow_other 0 0' >> /etc/fstab
```







## Virtual Box

> [Install](https://wiki.archlinux.org/index.php/Install) the [virtualbox](https://www.archlinux.org/packages/?name=virtualbox) package. You will need to choose a package to provide host modules:
>
> - for the [linux](https://www.archlinux.org/packages/?name=linux) kernel choose [virtualbox-host-modules-arch](https://www.archlinux.org/packages/?name=virtualbox-host-modules-arch)
> - for any other [kernel](https://wiki.archlinux.org/index.php/Kernel) (including [linux-lts](https://www.archlinux.org/packages/?name=linux-lts)) choose [virtualbox-host-dkms](https://www.archlinux.org/packages/?name=virtualbox-host-dkms)

```bash
sudo pacman -S virtualbox virtualbox-host-modules-arch
```



### Load the VirtualBox kernel modules

```bash
sudo modprobe vboxdrv
```

The following modules are only required in advanced configurations:

- `vboxnetadp` and `vboxnetflt` are both needed when you intend to use the [bridged](https://www.virtualbox.org/manual/ch06.html#network_bridged) or [host-only networking](https://www.virtualbox.org/manual/ch06.html#network_hostonly) feature.

```bash
sudo modprobe vboxnetadp vboxnetflt
```







# System

## GRUB

**Timeout**

```bash
sudo vim /etc/default/grub
```

```bash
GRUB_TIMEOUT=2
```



**Alias**

```bash
alias update-grub='sudo grub-mkconfig -o /boot/grub/grub.cfg'
```





## Systemd

```bash
sudo vim /etc/systemd/system.conf
```

```bash
RebootWatchdogSec=10s
ShutdownWatchd1ogSec=10s
DefaultTimeoutStartSec=5s
DefaultTimeoutStopSec=5s
```







## Disable System Beep Alert

```bash
xset -b
# sudo rmmod pcspkr
```





## Hibernate: Suspend to Disk

### 1. Get Block Device Name by UUID

#### 1.1 Swap Partition

```bash
┌─[lucas@ArchLinux] - [~] - [Fri Nov 06, 11:16]
└─[$] <> lsblk                  
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0 476.9G  0 disk 
├─nvme0n1p1 259:1    0   500M  0 part /boot
├─nvme0n1p2 259:2    0    12G  0 part [SWAP]
└─nvme0n1p3 259:3    0 386.3G  0 part /

┌─[lucas@ArchLinux] - [~] - [Fri Nov 06, 11:16]
└─[$] <> ls -l /dev/disk/by-uuid

total 0
lrwxrwxrwx 1 root root 15 Nov  6 10:52 29cab58f-f852-4239-8687-885533b5e7e4 -> ../../nvme0n1p3
lrwxrwxrwx 1 root root 15 Nov  6 10:52 66050937-2e5f-4508-bd21-f4335ee86c00 -> ../../nvme0n1p2
lrwxrwxrwx 1 root root 15 Nov  6 10:52 98DA-BD3E -> ../../nvme0n1p1
```



#### 1.2 Swap File

```bash
sudo swap-offset /swapfile
```





### 2. Configure Kernel Parameters

Edit `/etc/default/grub` and append your kernel options between the quotes in the `GRUB_CMDLINE_LINUX_DEFAULT` line:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="... resume=UUID=66050937-2e5f-4508-bd21-f4335ee86c00"
```

And then automatically re-generate the `grub.cfg` file with:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```


The kernel parameters will only take effect after rebooting. 



#### Swapfile

Using a swap file requires also setting a `resume_offset=*swap_file_offset*` kernel parameters. 

The value of `*swap_file_offset*` can be obtained by running `filefrag -v *swap_file*`, the output is in a table format and the required value is located in the first row of the `physical_offset` column. For example:

```
# filefrag -v /swapfile
Filesystem type is: ef53
File size of /swapfile is 4294967296 (1048576 blocks of 4096 bytes)
 ext:     logical_offset:        physical_offset: length:   expected: flags:
   0:        0..       0:      38912..     38912:      1:            
   1:        1..   22527:      38913..     61439:  22527:             unwritten
   2:    22528..   53247:     899072..    929791:  30720:      61440: unwritten
...
```

In the example the value of `*swap_file_offset*` is the first `38912` with the two periods.





### 3. Configure Initramfs

Edit `/etc/mkinitcpio.conf` and append `resume` to `HOOKS=(...)`:

```bash
HOOKS=(base udev ... resume)
```

Then regenerate the initramfs:

```bash
sudo mkinitcpio -p linux
```

Now reboot.

