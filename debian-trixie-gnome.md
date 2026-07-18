# Debian Trixie Post-Install Notes: GNOME

## Enable contrib and non-free repos

```console
sudo sed -i 's|main non-free-firmware|main non-free-firmware non-free contrib|' /etc/apt/sources.list
```

## Switch security repo to better mirror

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

## Update system

```console
sudo apt update
sudo apt full-upgrade
```

## Software

### Core

```console
sudo apt install bluez-firmware firmware-linux android-sdk-platform-tools-common htop nmap tmux memtest86+ plocate zsh neovim uptimed git aptitude build-essential pipx bat fzf fastfetch uuid-runtime gnome-shell-extension-manager mint-y-icons
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install filezilla geary transmission-gtk
```

### Multimedia

```console
sudo apt install amberol audacity blanket beets python3-flask ffmpeg flac lame mpg123 mpv normalize-audio eyed3 yt-dlp gstreamer1.0-vaapi cmus cmus-plugin-ffmpeg 
```

### Virtualization

```console
sudo apt install virt-manager
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running
libvirt tools.

### Flatpak

#### Install and enable

```console
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed before being able to install anything from Flatpak.

#### Install Spotify and LocalSend

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
```

### LibreWolf

```console
sudo apt install extrepo
sudo extrepo enable librewolf && sudo extrepo update librewolf
sudo apt update && sudo apt install librewolf
```

## Services

### Enable

```console
sudo systemctl enable --now fstrim.timer
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now open-iscsi
```
