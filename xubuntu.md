# Xubuntu Post-Install Notes

## Install Some Packages

### Core

```console
sudo apt install htop nmap tmux gamin zsh vim haveged uptimed bzr git build-essential aptitude hugo argyll icc-profiles feh scrot libimage-exiftool-perl conky-std faba-icon-theme moka-icon-theme
```

### Graphics

```console
sudo apt install create-resources gimp-data-extras gimp-gutenprint gimp-lensfun inkscape 
```

### Internet

```console
sudo apt install enigmail filezilla hexchat
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio btag youtube-dl gstreamer1.0-vaapi cmus
```

### Virtualization

```console
sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo adduser yourusernamehere libvirt
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Snap

Snap is enabled and setup by default in Ubuntu and its derivatives so there's
no installation and setup. We can just go ahead and install some Snaps.

### Install Chromium, Signal, Spotify, and Telegram

```console
snap install chromium chromium-ffmpeg signal-desktop spotify telegram-desktop
```

## Set Default Cursor Theme

```console
sudo update-alternatives --config x-cursor-theme
```

## Add Papirus PPA and Install Themes

```console
sudo add-apt-repository ppa:papirus/papirus
sudo apt update
sudo apt install papirus-icon-theme filezilla-theme-papirus libreoffice-style-papirus
```

## Services

### Disable

```console
sudo systemctl stop blueman-mechanism
sudo systemctl disable blueman-mechanism
sudo systemctl stop bluetooth
sudo systemctl disable bluetooth
sudo systemctl stop rsync
sudo systemctl disable rsync
sudo systemctl stop ufw
sudo systemctl disable ufw
