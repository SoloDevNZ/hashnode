---
title: "Preparing for Ubuntu 24.04 LTS."
datePublished: Mon Apr 01 2024 09:00:55 GMT+0000 (Coordinated Universal Time)
cuid: clugq083m000508ld7ri09nsa
slug: preparing-for-ubuntu-2404-lts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712058056624/4b22c162-8156-40d1-8927-79851f665284.png
tags: ubuntu, linux, linux-distro, linux-community, ubuntu-installation, ubuntu-2404-lts, lts-release, ubuntu-release, ubuntu-experience, ubuntu-desktop, ubuntu-studio, ubuntu-2404, operating-system-upgrade, system-upgrade, tech-upgrade

---

# TL;DR.

Getting ready for the Ubuntu 24.04 LTS upgrade is a step-by-step process. It includes making a bootable USB thumb drive on Windows 11, installing and securing Ubuntu, setting up Ubuntu Studio, and imaging my system with CloneZilla. This post also covers installing different package managers, browsers, various software, and LLM utilities. The upgrade to Ubuntu 24.04 LTS, happening on April 25, 2024, aims to offer me a stable system with a predictable computing environment.

> **Attributions:**
> 
> [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)***↗.***

# An Introduction.

In 2015, I started using CentOS 7 which was my first Linux-based distribution. When it was released in 2016, I switched to Ubuntu 16.04 LTS. Since then, I have upgraded my LTS distros every 2 years, like clockwork. It is time again to upgrade my systems:

> The purpose of this post is to define the installation process for Ubuntu 24.04 LTS, many of the development tools I use, and all of the system utilities I need.

# The Big Picture.

On Thursday 25<sup>th</sup> April 2024 (or thereabouts), I will install the new Ubuntu 24.04 LTS (long term service) distribution on every system in my home. These systems include:

* My Workstation PC (which is a dual-boot system),
    
* My Homelab NUC (which sits next to my Workstation),
    
* My Bedroom NUC (for streaming content from my NAS), and
    
* My Lounge Laptop (for streaming content when I'm not in bed).
    

My NAS has its own proprietary OS.

For now, I will practice the installation process with Ubuntu 23.10.

> NOTE: Ubuntu 23.10, code-named "Mantic Minotaur", is a regular release. This means Mantic is only supported until July 2024. Looking at the version number, I can tell that Mantic was released in October (23.`10`) of 2023 (`23`.10).

# Prerequisites.

* A NEW Linux-based distribution called [Ubuntu 24.04 LTS](https://ubuntu.com/download/desktop)***↗.***
    

# Making a Bootable USB Installer.

* In Windows, I plug in a spare 8GB USB thumb drive.
    

> NOTE: I find quality 8GB thumb drives are cheaper than 4GB thumb drives. This makes sense to me. Making a tape-out of an 8GB die costs the same as a 4GB tape-out. Cooking the wafers costs the same, too. Finally, it's cheaper NOT to re-tool for 4GB chips and just make 8GB chips instead. So, my guess is there's no cost-effective reason for making 4GB thumb drives. As a result, I believe scarcity is the main factor that is driving up the prices of 4GB thumb drives.

* I download the Ubuntu 24.04 LTS ISO file:
    

```plaintext
https://ubuntu.com/download/desktop
```

* I download, and install, an ISO-to-USB utility like [Rufus](https://rufus.ie/en/) or [balenaEtcher](https://etcher.balena.io/).
    
* I use the ISO-to-USB utility to install the Ubuntu 24.04 ISO file onto the 8GB USB thumb drive as a bootable image.
    

# Installing Ubuntu 24.04 LTS.

> NOTE: Ubuntu 24.04 LTS is installed on a 1TB M.2 SSD (solid-state drive) within my system. The boot loader is installed on the nvmeOn1p1 (500MB) partition, and the system files are installed on the nvmeOn1p2 (999.5GB) partition.

* I plug the bootable Ubuntu 24.04 LTS USB thumb drive into my system.
    
* I (re)start my system.
    
* During POST (power-on self test), I boot into my UEFI BIOS (unified extensible firmware interface basic input output system).
    
* I set the bootable USB thumb drive as the first boot device.
    
* I (re)start the UEFI BIOS POST sequence.
    
* After booting into the USB thumb drive, I begin installing the Ubuntu 24.04 LTS distribution.
    

# During the Installation Process.

My Workstation is a Windows/Ubuntu dual-boot system. The rest of my home systems run Ubuntu only. Therefore, only my Workstation requires a carefully crafted installation process, i.e. installing Ubuntu without destroying Windows. If things go wrong then I'd have to re-install Windows as well. Upgrading Ubuntu is painful enough. There's no point in shoving a thumb in my eye while I'm at it.

# Securing the APT Package Manager.

* After installing Ubuntu 24.04 LTS, I open the terminal.
    
* I install the secure transport protocol for the `APT` (advanced packaging tool), as well as `curl`, `wget`, `zip`, and `unzip`.
    

```plaintext
sudo apt install -y apt-transport-https \
curl wget zip unzip
```

> NOTE: Some of these utilities may already be installed by default.

# Changing Some Settings.

* I change the location of the Dock at `Settings > Ubuntu Desktop > Dock > Position on screen > Bottom`,
    
* I change the volumes visibility on the Dock at `Settings > Ubuntu Desktop > Dock > Configure dock behaviour > Show Volumes and Devices`, if required.
    
* I change where new icons appear at `Settings > Ubuntu Desktop > Position of New Icons`, if required.
    
* I change to Dark Mode at `Settings > Appearance > Style`, if required.
    
* I change the monitor layout at `Settings > Displays`, if required.
    
* I reduce the animation in the UI at `Settings > Accessibility > Reduce Animation`.
    
* I change the size of all text in the UI at `Settings > Accessibility > Large Text`.
    
* I change the cursor size in the UI at `Settings > Accessibility > Cursor Size`.
    
* I turn off the blinking cursor in the terminal at `Settings > Accessibility > Typing > Cursor Blinking`.
    
* At the terminal `Hamburger Menu > Preferences > Unnamed > Text tab`, I perform the following actions:
    
    * Change the terminal size to 92 columns and 24 rows,
        
    * Never allow blinking text within the terminal,
        
    * Disable the cursor blinking within the terminal, and
        
    * Activate the custom font and change the size to 18pt.
        

> NOTE: These settings are used so that screenshots and screencasts are clearly captured. Also, my eyesight is that of someone in their mid-50s. Oh, wait...

* I update my system:
    

```plaintext
sudo apt clean && \
sudo apt update && \
sudo apt dist-upgrade -y && \
sudo apt --fix-broken install && \
sudo apt autoclean && \
sudo apt autoremove -y
```

# Securing the New Installation.

Throughout the rest of this post, I will be creating way-point images as recovery mechanisms, although nothing EVER goes wrong. I suggest saving (or writing down) the names of these images in a file (or on a piece of paper) and crossing these names off the list as each image is created. All the image names are highlighted in yellow throughout the rest of this post.

# Using CloneZilla.

* I plug the CloneZilla USB thumb drive into my system.
    
* I (re)start my system.
    
* During POST (power-on self test), I boot into my UEFI BIOS (unified extensible firmware interface basic input output system) by continually tapping the F2 key as the POST runs.
    
* Within the UEFI BIOS, I set the bootable USB thumb drive as the first boot device.
    
* I (re)start the UEFI BIOS POST sequence.
    
* After booting into the USB thumb drive, I use CloneZilla to create an image of each stage of my installation process. Just in case. Although nothing EVER goes wrong. Right?
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-ubuntu</mark>.
    

> NOTE: The `[date]-img` is automatically generated by CloneZilla, `work` refers to the Workstation, and `ubuntu` is a reference to the contents of the image file. For instance, another system might use a name like `[date]-img-bed-porn` (which stands for `processes operating registration networks`, of course).

* The last option in my imaging sequence is to power off the system after the image is created.
    

# Connecting my NAS.

> NOTE: This section of my post is unique to my systems and can be safely ignored. If you have a NAS (network attached server) then you probably already have a plan for connecting it/them to your system(s).

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From the terminal, I change the owner of the `Ubuntu` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-ubuntu
```

---

> NOTE: I need to access my NAS early on in this post because of the high number of CloneZilla images I generate. I will occasionally need to off-load these images from the 2GB external HDD to the NAS so I can continue using CloneZilla.

* I install the CIFS utilities:
    

```plaintext
sudo apt install -y cifs-utils smbclient
```

> NOTE: CIFS is a dialect of SMB.

* I remove these directories:
    

```bash
sudo rm -r ~/Desktop ~/Documents ~/Downloads \
~/Music ~/Pictures ~/Public ~/Templates ~/Videos
```

> NOTE: It is now VITAL to continue this section of the post until the very end as these directories, which no longer exist, are vital to running this distro.

* I make a hidden file called `.cred_smb` in my home directory:
    

```bash
sudo touch /home/yt/.cred_smb
```

* I open the `.cred_smb` file using the Nano text editor:
    

```bash
sudo nano /home/yt/.cred_smb
```

* I copy the following, add it (CTRL + SHIFT + V) to the `.cred_smb` file, save (CTRL + S) the changes, and exit (CTRL + X) Nano:
    

```plaintext
username=yt
password=super-secret-password
domain=WORKGROUP
```

* I change the access permissions for the `.cred_smb` file:
    

```bash
sudo chmod 600 ~/.cred_smb
```

* I make a copy the `fstab` file as `fstab.bak`:
    

```plaintext
sudo cp /etc/fstab /etc/fstab.bak
```

* I use the Nano text editor to open the `fstab` file:
    

```plaintext
sudo nano /etc/fstab
```

* I copy the following, add it (CTRL + SHIFT + V) to the bottom of the `fstab` file, save (CTRL + S) the changes, and exit (CTRL + X) Nano:
    

```plaintext
//192.168.0.2/ai             /media/yt/AI cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/desktop        /media/yt/Desktop cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/mydocs         /media/yt/Documents cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/downloads      /media/yt/Downloads cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/drawings       /media/yt/Drawings cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/images         /media/yt/Images cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/multimedia     /media/yt/Media cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/music          /media/yt/Music cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/mydocs         /media/yt/MyDocs cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/mydrive        /media/yt/MyDrive cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/photos         /media/yt/Pictures cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/public         /media/yt/Public cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/screencasts    /media/yt/Screencasts cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/screenshots    /media/yt/Screenshots cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/templates      /media/yt/Templates cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/videos         /media/yt/Videos cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
```

* I create these directories where the remote shares will mount:
    

```plaintext
sudo mkdir /media/yt/AI && \
sudo mkdir /media/yt/Desktop && \
sudo mkdir /media/yt/Documents && \
sudo mkdir /media/yt/Downloads && \
sudo mkdir /media/yt/Drawings && \
sudo mkdir /media/yt/Images && \
sudo mkdir /media/yt/Media && \
sudo mkdir /media/yt/Music && \
sudo mkdir /media/yt/MyDocs && \
sudo mkdir /media/yt/MyDrive && \
sudo mkdir /media/yt/Pictures && \
sudo mkdir /media/yt/Public && \
sudo mkdir /media/yt/Screencasts && \
sudo mkdir /media/yt/Screenshots && \
sudo mkdir /media/yt/Templates && \
sudo mkdir /media/yt/Videos
```

* I create these symlinks (symbolic links) where the remote shares will display:
    

```plaintext
ln -s "/media/yt/AI" "/home/yt/AI"  && \
ln -s "/media/yt/Desktop" "/home/yt/Desktop" && \
ln -s "/media/yt/MyDocs" "/home/yt/Documents" && \
ln -s "/media/yt/Downloads" "/home/yt/Downloads" && \
ln -s "/media/yt/Drawings" "/home/yt/Drawings" && \
ln -s "/media/yt/Images" "/home/yt/Images" && \
ln -s "/media/yt/Media" "/home/yt/Media" && \
ln -s "/media/yt/Music" "/home/yt/Music" && \
ln -s "/media/yt/MyDocs" "/home/yt/MyDocs" && \
ln -s "/media/yt/MyDrive" "/home/yt/MyDrive" && \
ln -s "/media/yt/Pictures" "/home/yt/Pictures" && \
ln -s "/media/yt/Public" "/home/yt/Public" && \
ln -s "/media/yt/Screencasts" "/home/yt/Screencasts" && \
ln -s "/media/yt/Screenshots" "/home/yt/Screenshots" && \
ln -s "/media/yt/Templates" "/home/yt/Templates" && \
ln -s "/media/yt/Videos" "/home/yt/Videos"
```

> NOTE: Some of these symlinks will replace system directories, e.g. the Downloads directory.

* I reboot my system to check the NAS symlinks.
    
* I use Nano to open my `.bashrc` file:
    

```bash
sudo nano /home/yt/.bashrc
```

* I copy the following, add it (CTRL + SHIFT + V) to the bottom of the `.bashrc` file, save (CTRL + S) the changes, and exit (CTRL + X) Nano:
    

```bash
cd ~
```

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-nas</mark>.
    

# Installing Ubuntu Studio.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* I change the owner of the `Nas` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-nas
```

---

* I download, and install, Ubuntu Studio:
    

```plaintext
sudo apt install -y ubuntustudio-installer
```

* I run the installer:
    

```plaintext
ubuntustudio-installer
```

> NOTE: I can also run the installer from the Apps menu (𐄡).

* When the installer completes, I update my system:
    

```plaintext
sudo apt clean && \
sudo apt update && \
sudo apt dist-upgrade -y && \
sudo apt --fix-broken install && \
sudo apt autoclean && \
sudo apt autoremove -y
```

* I reboot my system to check the installation.
    

> NOTE: I had a small issue with the 535 GPU driver. Here's my fix: I switched to the X.Org open source driver, rebooted my system, re-installed the 535 GPU driver with `sudo ubuntu-drivers install nvidia:535`, and re-booted my system again.

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-studio</mark>.
    

# Installing Other Package Managers.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From a terminal, I change the owner of the `Studio` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-studio
```

## The Snap Package Manager.

* From the terminal, I use the `APT` to `install` the Snap daemon:
    

```plaintext
sudo apt install -y snapd
```

* I use `Snap` to `install` the `Snap` package manager `core`:
    

```plaintext
sudo snap install core
```

* I create a symbolic (`-s`) link (`ln`) to the `Snapd` daemon:
    

```plaintext
sudo ln -s /var/lib/snapd/snap /snap
```

## The Flatpak Package Manager.

* I use the `APT` to `install` the `Flatpak` package manager:
    

```plaintext
sudo apt install -y flatpak
```

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-pacman</mark>.
    

# Installing the Browsers.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From the terminal, I change the owner of the `Pacman` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-pacman
```

## The Firefox Browser.

* From the terminal, I download the Firefox browser, if required:
    

```plaintext
sudo snap install firefox
```

* I sign-in to my Firefox account.
    
* I setup the Firefox browser.
    

## The Chrome Browser.

* I change to the Downloads directory:
    

```bash
cd ~/Downloads
```

* I download the Chrome browser:
    

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

* I install the Chrome browser:
    

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

* I run the Chrome browser:
    

```bash
google-chrome
```

* I remove the Chrome browser installer:
    

```bash
sudo apt autoremove google-chrome-stable
```

## The Brave Browser.

* I download the GPG (GNU Privacy Guard) keyring:
    

```bash
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
```

* I download the repo:
    

```bash
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list
```

* I add the repo to my local system:
    

```bash
sudo apt update
```

* I install the Brave browser:
    

```bash
sudo apt install brave-browser
```

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-browsers</mark>.
    

# Installing the Container and Environment Managers.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From the terminal, I change the owner of the `Browsers` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-browsers
```

---

> NOTE: These posts describe how to install the container and environment managers.

* [LXD/LXC](https://solodev.app/installing-lxd-and-using-lxcs)
    
* [Distrobox](https://solodev.app/installing-distrobox)
    
* [Miniconda](https://solodev.app/installing-miniconda)
    
* [Docker](https://solodev.app/installing-docker)
    
* [Docker Desktop](https://solodev.app/4-of-10-installing-docker-desktop)
    
* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-envman</mark>.
    

# Installing DaVinci Resolve Studio 18.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From the terminal, I change the owner of the `EnvMan` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-envman
```

---

* I change to the Downloads directory:
    

```bash
cd ~/Downloads
```

* I install these libraries:
    

```bash
sudo apt install -y libfuse2 libapr1 libaprutil1 libglu1-mesa \
libnuma1 libxcb-composite0 libxcb-cursor0 libxcb-damage0 \
libxcb-icccm4 libxcb-image0 libxcb-keysyms1 libxcb-render-util0 \
libxcb-util1 libxcb-xinerama0 libxcb-xinput0 libxcb-xkb1 \
libxkbcommon-x11-0 ocl-icd-libopencl1 libasound2 libxcb-shape0 libxtst6
```

* I go to the DaVinci Resolve Studio 18 downloads page:
    

```bash
https://www.blackmagicdesign.com/event/davinciresolvedownload
```

* I download the installer.
    
* I unpack the DaVinci Resolve Studio 18 zip file:
    

```bash
unzip ./DaVinci_Resolve_18.6.5_Linux.zip
```

> NOTE: At the time of writing, 18.6.5 is the latest version.

* I change into the unzipped directory:
    

```bash
cd ./DaVinci_Resolve_18.6.5_Linux
```

* I list the contents of the directory:
    

```bash
ls
```

* I change the permission of the DaVinci\_Resolve\_Studio\_18.6.5\_Linux.run file to an executable:
    

```bash
sudo chmod +x ./DaVinci_Resolve_Studio_18.6.5_Linux.run
```

* I install the \[file-name\].run file:
    

```bash
sudo ./DaVinci_Resolve_Studio_18.6.5_Linux.run -i
```

* This is the displayed output after the installation:
    

```bash
Using a pre-existing Data dir : /var/BlackmagicDesign/DaVinci Resolve
DaVinci Resolve installed to /opt/resolve
```

# Installing Apps & Changing Settings.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From a terminal, I change the owner of the Studio image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-browsers
```

---

I install the following apps and change the following settings.

## Checking the GPU Driver.

* I check the GPU driver from the `Software & Updates > Additional Drivers` tab, if required.
    
* I install the GPU device driver (Metapackage from nvidia-driver-535), if required:
    

```bash
sudo ubuntu-drivers install nvidia:535
```

* Or I can let the system decide which drivers to install (not recommended):
    

```bash
sudo ubuntu-drivers autoinstall
```

> [https://ubuntu.com/server/docs/nvidia-drivers-installation](https://ubuntu.com/server/docs/nvidia-drivers-installation)

## Fixing the Dual-Boot Time Issue.

* I adjust the clock to fix the dual-boot time issue:
    

```bash
sudo apt update && sudo apt upgrade -y
timedatectl set-local-rtc 1 --adjust-system-clock
timedatectl
timedatectl set-local-rtc 0 --adjust-system-clock
```

## Installing VS Code.

* I use APT to install VS Code:
    

```bash
sudo apt install -y code
```

* Alternatively, I can instead use Snap to install VS Code:
    

```bash
sudo snap install code --classic
```

* I run VS Code:
    

```bash
code
```

* I can uninstall the APT-installed VS Code:
    

```bash
sudo apt purge --auto-remove code
```

* I can uninstall the Snap-installed VS Code:
    

```bash
sudo snap remove code
```

I can also install the following VS Code Extensions:

* Live Server,
    
* Geo Data Viewer,
    
* Code Spell Checker,
    
* JavaScript (ES6) Code Snippets,
    
* Prettier - Code formatter,
    
* Markdown All in One,
    
* rust-analyzer,
    
* C/C++,
    
* ES Lint,
    
* Colorize,
    
* Beautify,
    
* Rainbow Tags,
    
* Auto Rename Tag,
    
* Markdown Table Prettifier,
    

> NOTE: These extensions are OS-independent.

## Installing Spotify.

* I use Snap to install Spotify:
    

```bash
snap install spotify
```

## Updating Blender.

* I check the Blender version:
    

```bash
blender --version
```

* I remove Blender:
    

```bash
sudo apt purge --auto-remove blender
```

* I use Snap to install Blender:
    

```bash
sudo snap install blender --classic
```

* I check the Blender version:
    

```bash
blender --version
```

* I can remove Blender, if required:
    

```bash
sudo snap remove blender
```

## Installing Rust.

* I install the essential build tools:
    

```bash
sudo apt install build-essential
```

* I install Rust:
    

```bash
curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh
```

* I add the Cargo environment to my system:
    

```bash
source "$HOME/.cargo/env"
```

* I check the Rust version:
    

```bash
rustup --version
```

* I open the Rust documents:
    

```bash
rustup doc
```

* I can uninstall Rust, if required:
    

```bash
rustup self uninstall
```

## Installing Audacity.

* I install Audacity:
    

```bash
sudo apt install audacity
```

## Installing Screenkey.

* I install Screenkey:
    

```bash
sudo snap install screenkey --beta
```

## Installing HydraPaper.

* I install HydraPaper:
    

```bash
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub org.gabmus.hydrapaper
flatpak uninstall org.gabmus.hydrapaper
```

> NOTE: There is a known bug in HydraPaper running on Ubuntu 21.10 and later. Here is the (Dark Mode) fix.

* I find the `wallpaper_`[`merger.py`](http://merger.py) module:
    

```bash
sudo find / -name "*wallpaper_merger.py*"
```

* I change to that directory.
    
* I use VS Code to open the `wallpaper_`[`merger.py`](http://merger.py) module:
    

```bash
sudo code wallpaper_merger.py
```

* I find the `set_wallpaper_gnome` function.
    
* I remove the `if` part so all that is left is `wp_key='picture-uri-dark',` (WITH the ending comma.)
    
* I run HydraPaper in Dark Mode.
    

## Installing Elgato Stream Deck for Linux.

* I install streamdeck-ui:
    

```bash
https://timothycrosley.github.io/streamdeck-ui/
```

## Installing Tweaks.

* I install the GNOME Tweaks tool:
    

```bash
sudo apt install gnome-tweaks
```

## Installing Python.

* I install Python 3:
    

```plaintext
sudo apt install -y python3
```

* I install the Python packages:
    

```plaintext
sudo apt install -y python3-pip python3-dev python3-venv build-essential libssl-dev libffi-dev zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libreadline-dev
```

## Installing the Modular Tools.

* I download the Modular CLI:
    

```plaintext
curl https://get.modular.com | sh - && \
modular auth mut_511d5ea22b594cd385f8216af63b2d73
```

* I install the Modular CLI:
    

```bash
sudo apt install -y modular
```

* I install Mojo:
    

```bash
modular install mojo
```

* I update Mojo:
    

```bash
modular update mojo
```

## Installing LLM Utilities.

> NOTE: There are missing posts related to some of these LLM (large language model) utilities. These missing posts should be available by the end of April 2024.

* [Ollama](https://solodev.app/installing-ollama)
    
* LiteLLM
    
* MemGPT
    
* LangChain
    
* AutoGen Studio
    

# Saving the Final Image.

* I update my system:
    

```bash
sudo apt clean && \
sudo apt update && \
sudo apt dist-upgrade -y && \
sudo apt --fix-broken install && \
sudo apt autoclean && \
sudo apt autoremove -y
```

* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-system</mark>
    

# The Results.

Transitioning to Ubuntu 24.04 LTS promises to be a tiresome journey, filled with the promise of new features yet still ending up with the same ol' stable distribution. This guide has walked through the installation process, from preparing a bootable USB on a Widows 11 system to securing the APT package manager, and even connecting to a NAS. I've also covered the installation of various package managers, browsers, and essential software like Ubuntu Studio, ensuring I'm well-equipped for work. I've tackled system updates, app installs, and even the integration of LLM utilities, demonstrating the versatility of Ubuntu 24.04 LTS for app development. By creating system images at each significant step, I've ensured a safety net exists that makes for a time-consuming, yet stress-free process.

As I look forward to the official release, it's clear that Ubuntu 24.04 LTS is set to underwhelm my computing expectations, reaffirming Ubuntu's commitment to providing a bland, though user-friendly operating system. Whether for professional development, creative endeavours, or everyday use, Ubuntu 24.04 LTS is poised to become another mediocre upgrade for enthusiasts and newcomers alike. Safe. Boring. Predictable. Stable. Exactly what a developer needs in her systems.

# In Conclusion.

I'm looking forward to the Ubuntu 24.04 LTS launch! It's coming out on April 25, 2024, and I plan to update all of my home devices to this new version. This includes my Workstation PC, my Homelab NUC, the bedroom NUC, and the living room laptop.

I've been practising my installation process with Ubuntu 23.10, also known as "Mantic Minotaur," to ensure I'm ready for the launch. Even though it's only supported until July 2024, it's a great way to prepare for the big update. (I also needed content for this blog.)

Why choose Ubuntu 24.04 LTS? It has stability, security, and many features that will improve my computing experience. My current, yet continually evolving, process involves making a bootable USB on Windows 11, securing the APT package manager, and setting up Ubuntu Studio for my creative work. Plus, I made sure to back up all my systems with CloneZilla images at every step. Because, honestly, nothing ever goes wrong... right?

I've also installed LLM utilities, from LXD/LXC to AutoGen Studio, so I can make sure I'm ready for work. As the release gets closer, I feel excited and inspired. Ubuntu 24.04 LTS will be a stable and predictable upgrade, great for developers like me who prefer reliability over extra features.

What about you? Are you planning to upgrade to Ubuntu 24.04 LTS, or will you be watching from the sidelines? Let's discuss in the comments below!

Until next time: Be safe, be kind, be awesome.

#UbuntuInstallation #Ubuntu2404LTS #Ubuntu2404 #LTSRelease #UbuntuRelease #UbuntuExperience #UbuntuDesktop #UbuntuStudio #Ubuntu #Linux #LinuxDistro #LinuxCommunity #OperatingSystemUpgrade #SystemUpgrade #TechUpgrade #TechJourney #TechGuide #CloneZilla #SystemBackup #TechPreparation #SoftwareInstallation #DeveloperTools #OpenSource #LLMUtilities