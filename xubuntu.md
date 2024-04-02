# Xubuntu Post-Install Notes

## Install Some Packages

### Core

```console
sudo apt install htop nmap tmux gamin zsh vim vim-gtk3 haveged uptimed bzr git build-essential aptitude python3-pip ruby hugo argyll icc-profiles rxvt-unicode rofi conky-std faba-icon-theme moka-icon-theme
```

### Graphics

```console
sudo apt install create-resources gimp-data-extras gimp-gutenprint gimp-lensfun inkscape feh scrot libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install enigmail filezilla
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 youtube-dl gstreamer1.0-vaapi cmus
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

### Install Chromium and Spotify

```console
snap install chromium chromium-ffmpeg spotify
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
sudo systemctl disable --now blueman-mechanism
sudo systemctl disable --now bluetooth
sudo systemctl disable --now rsync
sudo systemctl disable --now ufw
