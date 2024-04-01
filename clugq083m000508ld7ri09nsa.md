---
title: "Preparing for Ubuntu 24.04 LTS."
datePublished: Mon Apr 01 2024 09:00:55 GMT+0000 (Coordinated Universal Time)
cuid: clugq083m000508ld7ri09nsa
slug: trailer-preparing-for-ubuntu-2404-lts
tags: ubuntu, linux, linux-distro, linux-community, ubuntu-installation, ubuntu-2404-lts, lts-release, ubuntu-release, ubuntu-experience, ubuntu-desktop, ubuntu-studio, ubuntu-2404, operating-system-upgrade, system-upgrade, tech-upgrade

---

# TL;DR.

Getting ready for the Ubuntu 24.04 LTS upgrade is a step-by-step process. It includes making a bootable USB on Windows, installing and securing Ubuntu, setting up Ubuntu Studio, and backing up my system with CloneZilla. The guide also talks about installing different package managers, browsers, important software, and LLM utilities to make sure I'm ready to use the new distribution. The upgrade to Ubuntu 24.04 LTS, happening on April 25, 2024, aims to offer a stable and reliable system for developers and users who need a predictable computing environment.

> **Attributions:**
> 
> [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)***↗.***

# An Introduction.

In 2015, I started using CentOS 7 which was my first Linux-based distribution. When it was released in 2016, I switched to Ubuntu 16.04 LTS. Since then, I have upgraded my LTS distro every 2 years. Like clockwork, it is time again to upgrade my systems:

> The purpose of this post is to define the installation process for Ubuntu 24.04 LTS, many of the development tools I use, and all of the system utilities I need.

# The Big Picture.

On Thursday 25<sup>th</sup> April 2024 (or thereabouts), I will install the new Ubuntu 24.04 LTS (long term service) distribution on every system in my home. These systems include:

* My Workstation PC,
    
* My Homelab NUC,
    
* My Bedroom NUC, and
    
* My Lounge Laptop.
    

My NAS has its own proprietary distribution.

For now, I will practice the installation process of 24.04 with Ubuntu 23.10.

> NOTE: Ubuntu23.10, code-named "Mantic Minotaur", is a regular release. This means Mantic is only supported until July 2024.

# Prerequisites.

* A NEW Linux-based distribution called Ubuntu 24.04 LTS (or Ubuntu 23.10 "Mantic Minotaur" if I'm practicing my installation process).
    

# Making a Bootable USB Installer.

* In Windows, I plug in a spare 8GB USB thumb drive.
    

> NOTE: I find quality 8GB thumb drives are cheaper than 4GB thumb drives. This makes sense to me. Making a tape-out of an 8GB die costs the same as a 4GB tape-out. Cooking the wafers costs the same, too. Finally, it's cheaper NOT to re-tool for 4GB chips and just make 8GB chips instead. So, my guess is there's no cost-effective reason for making 4GB thumb drives. As a result, I believe scarcity is the main factor that is driving up the prices for 4GB drives.

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

My Workstation is a Windows/Ubuntu dual-boot system while the rest of my home systems run Ubuntu only. Therefore, only my Workstation requires a carefully crafted installation process.

# Stabilizing the New Installation.

Throughout the rest of this post, I will be creating way-point images as recovery mechanisms, although nothing EVER goes wrong. I suggest saving (or writing down) the names of these images in a file (or on a piece of paper) and crossing these names off the list as each image is created. All the image names are highlighted in yellow throughout the rest of this post.

# Securing the APT Package Manager.

* After installing Ubuntu 24.04 LTS, I open the terminal.
    
* I install the secure transport protocol for the `APT` (advanced packaging tool), as well as `curl`, `wget`, `zip`, and `unzip`.
    

```plaintext
sudo apt install -y apt-transport-https \
curl wget zip unzip
```

> NOTE: Some of these utilities may already be installed by default.

* I update my system:
    

```plaintext
sudo apt clean && \
sudo apt update && \
sudo apt dist-upgrade -y && \
sudo apt --fix-broken install && \
sudo apt autoclean && \
sudo apt autoremove -y
```

# Using CloneZilla.

* I plug the CloneZilla USB thumb drive into my system.
    
* I (re)start my system.
    
* During POST (power-on self test), I boot into my UEFI BIOS (unified extensible firmware interface basic input output system) by continually tapping the F2 key as the POST runs.
    
* Within the UEFI BIOS, I set the bootable USB thumb drive as the first boot device.
    
* I (re)start the UEFI BIOS POST sequence.
    
* After booting into the USB thumb drive, I use CloneZilla to create an image of each stage of my installation process. Just in case. Although nothing EVER goes wrong.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-ubuntu</mark>.
    
* The last option in my imaging sequence is to power off the system after the image is created.
    

# Connecting my NAS.

> NOTE: This section of my post is unique to my systems and can be safely ignored. If you have a NAS (network attached server) then you probably already have a plan for connecting it to your system(s).

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From the terminal, I change the owner of the `Ubuntu` image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-ubuntu
```

* I install the CIFS utilities:
    

```plaintext
sudo apt install -y cifs-utils smbclient
```

> NOTE: CIFS is a dialect of SMB.

* I make a hidden file called `.cred_smb` in my home directory:
    

```bash
sudo touch ~/.cred_smb
```

* I change the access permissions for the `.cred_smb` file:
    

```bash
sudo chmod 600 ~/.cred_smb
```

* I open the `.cred_smb` file using the Nano text editor:
    

```bash
sudo nano ~/.cred_smb
```

* I copy the following, add it (CTRL + SHIFT + V) to the file, save (CTRL + S) the changes, and exit (CTRL + X) Nano:
    

```plaintext
username=yt
password=super-secret-password
domain=WORKGROUP
```

* I create these directories where the remote shares will mount:
    

```plaintext
sudo mkdir /media/yt/AI && \
sudo mkdir /media/yt/Downloads && \
sudo mkdir /media/yt/Drawings && \
sudo mkdir /media/yt/Images && \
sudo mkdir /media/yt/Media && \
sudo mkdir /media/yt/Music && \
sudo mkdir /media/yt/MyDocs && \
sudo mkdir /media/yt/MyDrive && \
sudo mkdir /media/yt/Photos && \
sudo mkdir /media/yt/Public && \
sudo mkdir /media/yt/Screencasts && \
sudo mkdir /media/yt/Screenshots && \
sudo mkdir /media/yt/Templates && \
sudo mkdir /media/yt/Videos
```

* I make a copy the `fstab` file as `fstab.bak`:
    

```plaintext
sudo cp /etc/fstab /etc/fstab.bak
```

* I open the `fstab` file:
    

```plaintext
sudo nano /etc/fstab
```

Add the following to the bottom of the fstab file:

```plaintext
//192.168.0.2/ai             /media/brian/AI cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/downloads      /media/brian/Downloads cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/drawings       /media/brian/Drawings cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/images         /media/brian/Images cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/multimedia     /media/brian/Media cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/music          /media/brian/Music cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/mydocs         /media/brian/MyDocs cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/mydrive        /media/brian/MyDrive cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/photos         /media/brian/Photos cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/public         /media/brian/Public cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/screencasts    /media/brian/Screencasts cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/screenshots    /media/brian/Screenshots cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/templates      /media/brian/Templates cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
//192.168.0.2/videos         /media/brian/Videos cifs vers=3.0,uid=1000,gid=1000,credentials=/home/yt/.cred_smb
```

* I reboot my system.
    
* I check the `/media/yt` directory to see if the shares are mounted.
    
* I create symlinks (symbolic links) in my Home directory:
    

```plaintext
sudo ln -s "/media/yt/AI" "~/AI"  && \
sudo ln -s "/media/yt/Downloads" "~/Downloads" && \
sudo ln -s "/media/yt/Drawings" "~/Drawings" && \
sudo ln -s "/media/yt/Images" "~/Images" && \
sudo ln -s "/media/yt/Media" "~/Media" && \
sudo ln -s "/media/yt/Music" "~/Music" && \
sudo ln -s "/media/yt/MyDocs" "~/MyDocs" && \
sudo ln -s "/media/yt/MyDrive" "~/MyDrive" && \
sudo ln -s "/media/yt/Photos" "~/Photos" && \
sudo ln -s "/media/yt/Public" "~/Public" && \
sudo ln -s "/media/yt/Screencasts" "~/Screencasts" && \
sudo ln -s "/media/yt/Screenshots" "~/Screencasts" && \
sudo ln -s "/media/yt/Templates" "~/Templates" && \
sudo ln -s "/media/yt/Videos" "~/Videos"
```

> NOTE: Some of these symlinks will replace system directories, e.g. the Downloads directory.

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-nas</mark>.
    

# Installing Other Package Managers.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From a terminal, I change the owner of the Nas image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-nas
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
    
* From a terminal, I change the owner of the Pacman image:
    

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

* I download a list of packages:
    

```bash
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list
```

* I update the APT package manager to integrate the new list of packages:
    

```bash
sudo apt update
```

* I install the Brave browser:
    

```bash
sudo apt install brave-browser
```

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-browsers</mark>.
    

# Installing Ubuntu Studio.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* I change the owner of the Browsers image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-browsers
```

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

* I reboot my system with the CloneZilla USB thumb drive installed.
    
* I use CloneZilla to create an image of the distribution called <mark>[date]-img-work-studio</mark>.
    

# System Updates and App Installs.

* I power up my Workstation.
    
* In the file manager, I navigate to the `img-work` drive.
    
* I open the `img-work` drive in a terminal (`Right-click > Open in Terminal` from the popup menu).
    
* From a terminal, I change the owner of the Studio image:
    

```plaintext
sudo chown -R $USER:$USER [date]-img-work-studio
```

I update the following system settings and install the following apps (unless they're already pre-installed).

## Changing the Settings.

* I change the volumes visibility on the Dock at `Settings > Appearance > Dock > Configure dock behaviour`, if required.
    
* I change where new icons appear at `Settings > Appearance > Position of New Icons`, if required.
    
* I change to Dark Mode at `Settings > Appearance > Style`, if required.
    
* I change the monitor layout at `Settings > Displays`, if required.
    

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
$ sudo apt update && sudo apt upgrade -y
$ timedatectl set-local-rtc 1 --adjust-system-clock
$ timedatectl
$ timedatectl set-local-rtc 0 --adjust-system-clock
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

## Installing DaVinci Resolve Studio 18.

* I change to the Downloads directory:
    

```bash
cd ~/Downloads
```

* I install these libraries:
    

```bash
sudo apt install libfuse2 libapr1 libaprutil1 libxcb-cursor0
```

* I go to the DaVinci Resolve Studio 18 downloads page:
    

```bash
https://www.blackmagicdesign.com/event/davinciresolvedownload
```

* I download the installer.
    
* I unpack the DaVinci Resolve Studio 18 zip file:
    

```bash
unzip ./DaVinci_Resolve_18.6.4_Linux.zip
```

> NOTE: At the time of writing, 18.6.4 is the latest version.

* I change into the unzipped directory:
    

```bash
cd ./DaVinci_Resolve_18.6.4_Linux
```

* I list the contents of the directory:
    

```bash
ls
```

* I change the permission of the \[file-name\].run file to an executable:
    

```bash
sudo chmod +x [file-name].run
```

* I install the \[file-name\].run file:
    

```bash
sudo ./[file-name].run -i
```

## Installing Spotify.

* I use Snap to install Spotify:
    

```bash
$ snap install spotify
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

* I install Rust,
    

```bash
$ sudo apt update && sudo apt upgrade
$ sudo apt install build-essential
$ curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh
$ source "$HOME/.cargo/env"
$ rustup --version
$ rustup doc
$ rustup self uninstall
```

## Installing Screenkey.

* I install Screenkey:
    

```bash
$ sudo apt update && sudo apt upgrade -y
$ sudo snap install screenkey --beta
```

## Installing HydraPaper.

* I install HydraPaper:
    

```bash
$ sudo apt update && sudo apt upgrade -y
$ sudo apt install flatpak
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
$ flatpak install flathub org.gabmus.hydrapaper
$ flatpak uninstall org.gabmus.hydrapaper
```

> NOTE: There is a known bug in HydraPaper running on Ubuntu 21.10 and later. Here is the (Dark Mode) fix.

* I find the `wallpaper_`[`merger.py`](http://merger.py) module:
    

```bash
$ sudo find / -name "*wallpaper_merger.py*"
```

* I change to that directory.
    
* I use VS Code to open the `wallpaper_`[`merger.py`](http://merger.py) module:
    

```bash
$ sudo code wallpaper_merger.py
```

* I find the `set_wallpaper_gnome` function.
    
* I remove the `if` part so all that is left is `wp_key='picture-uri-dark',` (WITH the ending comma.)
    
* I run HydraPaper in Dark Mode.
    

## Installing Elgato Stream Deck for Linux.

* I install streamdeck-ui:
    

```bash
https://timothycrosley.github.io/streamdeck-ui/
```

## Other Media Apps to Install.

* VLC Media Player
    
* Audacity
    
* Kdenlive
    
* Inkscape
    
* Blender
    
* GIMP
    
* Spotify
    
* OBS Studio
    

## Other Development Tools to Install.

* VS Code:
    
* [Docker](https://solodev.app/installing-docker)
    
* [Docker Desktop](https://solodev.app/4-of-10-installing-docker-desktop)
    
* Python 3:
    

```plaintext
sudo apt install -y python3
```

* Python packages:
    

```plaintext
sudo apt install -y python3-pip python3-dev python3-venv build-essential libssl-dev libffi-dev zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libreadline-dev
```

* Modular CLI:
    

```plaintext
curl https://get.modular.com | sh - && \
modular auth mut_511d5ea22b594cd385f8216af63b2d73
```

* Modular update: `sudo apt install -y modular`
    
* Mojo: `modular install mojo`
    
* Mojo update: `modular update mojo`
    
* Rust: `curl --proto '=https' --tlsv1.2 -sSf`[`https://sh.rustup.rs`](https://sh.rustup.rs)`| sh`
    

## Other Utilities to Install.

* Thunar: `sudo apt install -y thunar`
    
* LibreOffice
    
* HydraPaper
    
* Screenkey
    
* Remmina
    
* Tweaks
    
* I update the system:
    

```plaintext
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt --fix-broken install && sudo apt autoremove -y
```

* I use CloneZilla to create an image of the distro called <mark>[date]-img-work-apps</mark>
    

## Installing LLM Utilities.

* [LXD](https://solodev.app/2-of-10-lxd-on-the-homelab)
    
* [Distrobox](https://solodev.app/installing-distrobox)
    
* [Miniconda](https://solodev.app/installing-miniconda)
    
* [Ollama](https://solodev.app/installing-ollama)
    
* LiteLLM
    
* AutoGen
    
* MemGPT
    
* LangChain
    
* AutoGen Studio
    

# Saving the Final Image.

* I update the system:
    

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

Transitioning to Ubuntu 24.04 LTS promises to be a tiresome journey, filled with the promise of new features yet settling on the same ol' stable distribution. This guide has walked through the detailed process of installation, from preparing a bootable USB on a Widows 11 system to securing the APT package manager, and even connecting to a NAS. I've also covered the installation of various package managers, browsers, and essential software like Ubuntu Studio, ensuring I'm well-equipped for work. Additionally, I've tackled system updates, app installs, and even the integration of LLM utilities, demonstrating the versatility of Ubuntu 24.04 LTS. By creating system images at each significant step, I've ensured a safety net, making the process forgiving and stress-free. As I look forward to the official release, it's clear that Ubuntu 24.04 LTS is set to underwhelm my computing experience, reaffirming Ubuntu's commitment to providing a bland, though user-friendly operating system. Whether for professional development, creative endeavors, or everyday use, Ubuntu 24.04 LTS is poised to become another mediocre upgrade for enthusiasts and newcomers alike. Safe. Boring. Predictable. Stable. Exactly what a developer needs in her systems.

# In Conclusion.

I'm looking forward to the Ubuntu 24.04 LTS launch! It's coming out on April 25, 2024, and I plan to update all my devices at home to this new version. This includes everything from my main PC to the laptop in my living room.

I've been using Ubuntu 23.10, also known as "Mantic Minotaur," to get ready for what's next. Even though it's only supported until July 2024, it's a great way to prepare for the big update.

Why choose Ubuntu 24.04 LTS? It has stability, security, and many features that will improve my computers. My experience involved making a bootable USB on Windows 11, securing the APT package manager, and setting up Ubuntu Studio for my creative work. Plus, I made sure to back up all my systems with CloneZilla images at every step. Because, honestly, nothing ever goes wrong... right?

I've also installed LLM utilities, from LXD to AutoGen Studio, to make sure I'm ready for anything. As the release gets closer, I feel both excited and nostalgic. Ubuntu 24.04 LTS will be a stable and predictable upgrade, great for developers like me who prefer reliability over extra features.

What about you? Are you planning to upgrade to Ubuntu 24.04 LTS, or will you be watching from the sidelines? Let's discuss in the comments below!

Until next time: Be safe, be kind, be awesome.

#UbuntuInstallation #Ubuntu2404LTS #Ubuntu2404 #LTSRelease #UbuntuRelease #UbuntuExperience #UbuntuDesktop #UbuntuStudio #Ubuntu #Linux #LinuxDistro #LinuxCommunity #OperatingSystemUpgrade #SystemUpgrade #TechUpgrade #TechJourney #TechGuide #CloneZilla #SystemBackup #TechPreparation #SoftwareInstallation #DeveloperTools #OpenSource #LLMUtilities