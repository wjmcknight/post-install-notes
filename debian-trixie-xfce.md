# Debian Trixie Post-Install Notes: Xfce

## Enable contrib and non-free Repos

```console
sudo sed -i 's|main non-free-firmware|main non-free-firmware non-free contrib|' /etc/apt/sources.list
```

## Switch Security Repo to Better Mirror

```console
sudo sed -i 's|security.debian.org|mirror.csclub.uwaterloo.ca|' /etc/apt/sources.list
```

## Enable Deb Multimedia

```console
wget http://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2024.9.1_all.deb
sudo dpkg -i deb-multimedia-keyring_2024.9.1_all.deb
echo -e "# Multimedia\ndeb http://mirror.csclub.uwaterloo.ca/debian-multimedia/ trixie main non-free\ndeb-src http://mirror.csclub.uwaterloo.ca/debian-multimedia/ trixie main non-free" | sudo tee /etc/apt/sources.list.d/multimedia.list
```

## Enable Backports

```console
echo -e "# Backports\ndeb http://mirror.csclub.uwaterloo.ca/debian/ trixie-backports main non-free-firmware non-free contrib\ndeb-src http://mirror.csclub.uwaterloo.ca/debian/ trixie-backports main non-free-firmware non-free contrib" | sudo tee /etc/apt/sources.list.d/backports.list
```

## Update System

```console
sudo apt update
sudo apt full-upgrade
```

## Package Clean Up

Especially because we're using Deb Multimedia, you'll probably get a message
about some packages that can be removed. Let's do that then run `dpkg` as a
sanity check.

```console
sudo apt autoremove
sudo dpkg -C
```

## Install Some Packages

### Core

```console
sudo apt install firmware-linux amd64-microcode htop nmap tmux memtest86+ plocate zsh vim vim-gtk3 tuned haveged uptimed git build-essential aptitude python3-pip argyll icc-profiles conky-std rofi galculator gvfs-backends faba-icon-theme moka-icon-theme papirus-icon-theme greybird-gtk-theme
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras inkscape feh scrot libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install chromium filezilla geary transmission-gtk
```

### Multimedia

```console
sudo apt install asunder audacity guvcview handbrake-gtk pragha beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 yt-dlp gstreamer1.0-vaapi mpd mpc ncmpcpp 
```

### Virtualization

```console
sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install and Enable

```console
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed before being able to install anything from Flatpak.

### Install Spotify

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
```

## Services

### Disable

```console
sudo systemctl disable mpd --now
sudo systemctl disable open-iscsi --now
```
