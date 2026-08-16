# openSUSE Tumbleweed Post-Install Notes: GNOME

## Set hostname and locale

```console
sudo hostnamectl hostname host.localdomain
sudo localectl set-locale "en_CA.UTF-8"
```

## Remove GNOME Games pattern then lock it

```console
sudo zypper rm -u patterns-gnome-gnome_games
sudo zypper al patterns-gnome-gnome_games
```

## Update system

```console
sudo zypper dup 
```

## Software

### Core

```console
sudo zypper in bluez-firmware sof-firmware android-tools android-udev-rules htop nmap tmux memtest86+ plocate zsh neovim git python313-pipx bat fzf fastfetch extension-manager gnome-calendar alacritty mint-y-icon-theme opi
```

### Graphics

```console
sudo zypper in perl-Image-ExifTool dcraw libwebp-tools
```

### Internet

```console
sudo zypper in geary transmission-gtk
```

### Multimedia

```console
opi codecs
sudo zypper in amberol audacity decibels beets flac lame mpg123 mpg123-pulse mpv normalize python313-eyed3 yt-dlp gstreamer-plugins-vaapi cmus cmus-plugin-ffmpeg cmus-plugin-pulse
```

### Virtualization

```console
sudo zypper in libvirt virt-manager
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running
libvirt tools.

### Flatpak

#### Install LibreWolf, Spotify, and LocalSend

```console
flatpak install flathub io.gitlab.librewolf-community
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
```

## Services

### Enable

```console
sudo systemctl enable --now fstrim.timer
sudo systemctl enable --now tuned
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now iscsi
```
