---
title: "installing arch"
date: 2021-08-12T12:06:46-05:00
---

# installing arch  
August 12, 2021

## installation procedure  

### 1. boot to iso

### 2. partition disks  
- `fdisk -l` to identify disk
- `fdisk /dev/sdX` or `fdisk /dev/nvmeXXX`
- create partitions
  1. efi partition, +512M as #1 EFI System type
  2. root partition, +40G as #23 Linux x86_64
  3. home partition, remaining capacity as Linux Filesystem

### 3. format partitions  
  - efi partition `mkfs.fat -F32 /dev/sda1`
  - root and home `mkfs.ext4 /dev/sdaX`

### 4. networking  
  - wired should already work
  - wifi `wifi-menu`

### 5. update system and install reflector, update reflector  
  ```bash
  $ pacman -Syy
  $ pacman -S reflector
  # backup mirrorlist
  $ cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
  # update mirrorlist
  $ reflector -c "US" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist
  ```

### 6. mount partitions  
```bash
# mkdirs
$ mkdir -pv /mnt/home /mnt/boot/efi
# mount partitions
$ mount /dev/sda2 /mnt # root partition to /mnt
$ mount /dev/sda1 /mnt/boot/efi  # efi partition to /mnt/boot/efi
$ mount /dev/sda3 /mnt/home # home partition to /mnt/home
```

### 7. install arch system  
```bash
$ pacstrap /mnt base linux linux-firmware vim 
```

### 8. generate fstab  
`genfstab -U /mnt >> /mnt/etc/fstab`

### 9. arch-chroot and set up system  
```bash
# enter system as root
$ arch-chroot /mnt
# timezone
$ timedatectl set-timezone America/Chicago
# locale
$ vim /etc/local.gen # uncomment locale en_US-UTF.8
$ echo LANG=en_US-UTF.8 > /etc/locale/conf
$ export LANG=en_US-UTF.8
# hosts
$ echo myarch > /etc/hostname # customize hostname for myarch
$ vim /etc/hosts
  # ADD HOSTS
  127.0.0.1     localhost
  ::1           localhost
  127.0.1.1     myarch    #same as hostname
$ passwd # set root password
# update pacman.conf
$ vim /etc/pacman.conf
  # add ILoveCandy to the options section
  [Options]
  ...
  ILoveCandy

  # uncomment multilib section
  [multilib]
  Include=/etc/pacman.d/mirrorlist
```

### 10. install grub  
```bash
$ pacman -S grub efibootmgr
$ grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
$ grub-mkconfig -o /boot/grub/grub.cfg
```

### 11. install desktop  
```bash 
$ pacman -S xorg sddm plasma kde-applications
# if nvidia gpu
$ pacman -S nvidia nvidia-settings nvidia-utils nvidia-dkms lib32-nvidia-utils vulkan-icd-loader lib32-vulkan-icd-loader
# if amd cpu
$ pacman -S amd-ucode
# other utils
$ pacman -S nfs-utils neofetch firefox git base-devel ttf-fira-code ttf-fira-mono ttf-fira-sans ttf-dejavu
```

### 12. enable display server, network, config dm  
```bash
$ systemctl enable sddm
$ systemctl enable NetworkManager
$ vim /usr/lib/sddm/sddm.conf.d/default.conf
  # find [Theme] and add 'breeze'
  [Theme]
  Current=breeze
```

### 13. Reboot  
```bash
# exit arch-chroot
$ exit
$ systemctl reboot
```
---

## post-install config  

### install software  
  - **install aur/yay**

    ```bash  
    $ cd /opt
    $ sudo git clone https://aur.archlinux.org/yay-git.git
    $ sudo chown -R gavin:gavin ./yay-git
    $ cd yay-git
    $ makepkg -si
    $ sudo yay -Syu
    ```  
  - **install software pacman**  

    ```bash   
    pacman -S steam lutris newsboat mpv youtube-dl jq fzf ueberzug conky thunderbird signal-desktop virtualbox libreoffice-still rsync rclone wget
    ```  
    - steam
    - lutris
    - newsboat
    - mpv 
    - youtube-dl
    - jq fzf ueberzug
    - conky
    - signal
    - thunderbird
    - virtualbox
      - `sudo modprobe vboxdrv`
      - `sudo vim /etc/modules-load.d/virtualbox.conf`
        - add `vboxdrv`
      - `sudo usermod -aG vboxusers gavin`
      - https://www.virtualbox.org/ > downloads > extensions > all supported platforms
      - open virtualbox, add extension  
    - libreoffice
    - rsync
    - rclone
      - copy rclone config file
    - wget

  - **install AUR software yay**  

    ```bash   
    yay -S brave-bin vscodium bottom gwe ttf-ms-win10 synology-drive conky openrgb discord_arch_electron mangohud minecraft-launcher slack-desktop
    ```  
    - brave
    - codium 
    - bottom
    - conky
    - gwe (nvidia)
    - synology drive client
    - openRGB
      - `pacman -S i2c-tools`
      - Allowing access to SMBus:
        - Load the i2c-dev module: `sudo modprobe i2c-dev`
      - Load the i2c driver for your chipset:
      - AMD:
        - `modprobe i2c-piix4`
    - ytfzf
    - mangohud
    - discord
    - minecraft
    - slack

  - **install wine**  

    ```bash    
    $ sudo pacman -S wine-staging giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader
    ```
  - **install thinkorswim**  
    - `yay -S zulu-11-bin`
    - download installer script from [thinkorswim](https://www.tdameritrade.com/tools-and-platforms/thinkorswim/desktop/download.html)
    - run script
  - **install ssg**  
    - https://shibby.xyz/install-ssg.html  
    - dependencies: cpio lowdown  
      - `pacman -S cpio`  
      - `yay -S lowdown`  

---

## system configs  

- config fonts
  - `yay -S ttf-ubraille ttf-symbola`
  - `yay -Rs gnu-free-fonts`
  - `fc-cache -fv --really-force`
- copy dotfiles
  - alias
  - bash prompt
  - newsboat config
    - ~/.newsboat
  - conky
    - ~/.config/.conkyrc
  - mangohud config
    - `~/.config/MangoHud/MangoHud.conf`
    - `/usr/share/doc/mangohud/MangoHud.conf.example`
- config dolphin
  - disable zoom slider
  - show home directory on startup
  - disable folders expandable
- config konsole  
  - do not remember window size  
  - import profile   
    - location: `~/.local/share/konsole/`  
    <!-- - $ `cp share/unraid/files/salient/Salient\ OS.profile ~/.local/share/konsole/`   -->
  - import colors  
    - /usr/share/konsole/  
    - `sudo cp share/unraid/files/salient/Salient\ OS.colorscheme /usr/share/konsole/`  
  - hide menubar  
  - hide toolbar  
- thunderbird  
  - `~/.thunderbird` replace with backup   
- kde connect  
- create share folders   

  ```bash
  $ mkdir -pv ~/share/unraid/files ~/share/unraid/backups ~/share/unraid/isos ~/share/unraid/photos ~/share/unraid/downloads ~/share/unraid/sinefiles
  ```
- update fstab with shared drives
  - [fstab](../fstab) 

  - ```bash  
    192.168.50.148:/mnt/user/files  /home/gavin/share/unraid/urFiles  nfs  rw,dev,exec,auto,user,async  0  0
    ```

- ssh keys

---
## customize kde  
- global theme - breeze dark
- app style - breeze
- plasma - breeze dark
  - `plasma-apply-desktoptheme breeze-dark`
- colors - salient dark
  - `/usr/share/color-schemes/`
  - `sudo cp ~/share/unraid/files/salient/SalientDark.colors /usr/share/color-schemes/`
  - `plasma-apply-colorscheme SalientDark`
- window decorations
  - theme breeze
  - edit remove circle around close button
  - edit shadows 35%
- icons - tela dark/tela circle dark
- cursors - bibata-original-classic
- splash screen - none
- wallpaper
  - desktop
  - login screen
  - lock screen
- compositor - opengl 3.1
- general behavior
  - clicking files or folders selects them
- window management 
  - task switcher: large icons
  - window behavior > advanced > window placement > centered
- keyboard shortcuts
  - super+t terminal
  - super+e dolphin
  - super+d desktop
  - super+esc lock screen
  - super+a show all windows
    - toggle present windows current desktop
  - remap caps lock to esc
- krunner
  - position on screen: center
- panel
  - content
    - application menu - avalon menu
      - icon arch logo
    - show desktop
    - icons only task manager
    - separator
    - better inline clock
    - separator
    - system tray
  - position: top
  - width: 32
- system monitor widgets 
  - if blank, `pacman -S ksysguard` and reboot
- **IMPORT OPTIONS**  
  - install Plasma Customization Saver
  - import default.tar.gz

---
## references   
[its foss](https://itsfoss.com/install-arch-linux/)  
[kde sddm not wayland](https://debugpoint.com/2021/kde-plasma-arch-linux-install)  
[its foss (kde/wayland)](https://itsfoss.com/install-kde-arch-linux/)  
[install yay/aur](https://www.techmint.com/install-yay-aur-helper-in-arch-linux-and-manjaro)  
[install virtualbox on arch](https://linuxhint.com/install-virtualbox-arch-linux/) 
[btm font braille](https://github.com/LukeSmithxyz/voidrice/pull/901) 
[kde-configuration-files](https://github.com/shalva97/kde-configuration-files)
[kde config to kwriteconfig.kt](https://gist.github.com/shalva97/a705590f2c0e309374cccc7f6bd667cb)

## other  
packages installed but these failed on a previous install  
vulkan-icd-loader  
lib32-nvidia-utils  
lib32-vulkan-icd-loader  

---
## research/future items  

- arch disk encryption
  - [encrypted arch install](https://fribbledom.com/posts/encrypted-arch-install/)
  - [arch install disk encrypt](https://medium.com/hacker-toolbelt/arch-install-with-full-disk-encryption-6192e9635281)
- bash vs zsh
  - `pacman -S zsh zsh-completions grml-zsh-config`
  - [arch zsh](https://wiki.archlinux.org/title/Zsh)
  - [configure zsh](https://www.ubuntupit.com/how-to-install-and-configure-zsh-on-linux-distributions/)
  - [configure zsh ohmyzsh](https://hackernoon.com/how-to-trick-out-terminal-287c0e93fce0)
  - [bash vs zsh linuxhint](https://linuxhint.com/differences_between_bash_zsh/)
  - [bash vs zsh reddit](https://www.reddit.com/r/archlinux/comments/27wxkv/bash_or_zsh_what_are_you_using_and_why/)
- benefits of swap
  - https://wiki.archlinux.org/title/Swap
- update kde settings from cli OR through script file
- backup kde settings:
  - [backup kde](https://www.addictivetips.com/ubuntu-linux-tips/backup-kde-plasma-5-desktop-linux/)
- btm cpu freq
- pass 
  - [using pass](https://wiki.archlinux.org/title/Pass)
- disable GRUB boot menu
  - [disable boot menu](https://linoxide.com/hide-grub-boot-linux/)
  - [grup tips and hide grub unless shift is pressed](https://wiki.archlinux.org/title/GRUB/Tips_and_tricks#Hide_GRUB_unless_the_Shift_key_is_held_down)
- add grub boot splash screen
  - [bootsplash](https://github.com/charveey/bootsplash-systemd)
- copy from vim screen in konsole
- IRC/Matrix
- startup: conky
- add .local/bin to PATH
