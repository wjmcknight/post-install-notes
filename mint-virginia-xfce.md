# Linux Mint: XFCE Post-Install Notes

## Update System

```console
sudo apt update
sudo apt full-upgrade
```

## Install Some Packages

### Core

```console
sudo apt install htop nmap tmux memtest86+ gamin zsh vim vim-gtk3 tuned haveged uptimed git build-essential python3-pip ruby hugo argyll icc-profiles rofi conky-std faba-icon-theme moka-icon-theme greybird-gtk-theme
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun inkscape feh scrot libimage-exiftool-perl
```

### Internet

```console
sudo apt install chromium filezilla
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 cmus
```

### Virtualization

```console
sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install Spotify

```console
flatpak install flathub com.spotify.Client
```

## Install Papirus Icon Themes

```console
sudo add-apt-repository ppa:papirus/papirus
sudo apt update
sudo apt install papirus-icon-theme filezilla-theme-papirus
```

## Change System Cursor

```console
sudo update-alternatives --config x-cursor-theme
```

## Services

### Disable

```console
sudo systemctl disable --now blueman-mechanism
sudo systemctl disable --now bluetooth
sudo systemctl disable --now openvpn
sudo systemctl disable --now ufw
