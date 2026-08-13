# openSUSE Leap 16 Post-Install Notes: GNOME

## Upgrade system and reboot

```console
sudo zypper up
```

## Remove GNOME Games pattern then lock it

```console
sudo zypper rm -u patterns-gnome-gnome_games
sudo zypper al patterns-gnome-gnome_games
```

## Add repo for newer Alacritty

```console
sudo zypper ar https://download.opensuse.org/repositories/home:jlkDE/16.0/home:jlkDE.repo
sudo zypper ref
```

## Software

### Core

```console
sudo zypper in android-tools htop nmap tmux memtest86+ plocate zsh neovim git python313-pipx bat fzf fastfetch extension-manager alacritty mint-y-icon-theme opi gnome-calendar
```

### Graphics

```console
sudo zypper in perl-Image-ExifTool dcraw libwebp-tools
```

### Internet

```console
sudo zypper in geary
```

### Multimedia

```console
opi codecs
sudo zypper in audacity decibels beets flac lame mpg123 mpg123-pulse mpv normalize python313-eyed3 yt-dlp cmus cmus-plugin-ffmpeg cmus-plugin-pulse
```

### Virtualization

```console
sudo zypper in libvirt
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running
libvirt tools.

### Flatpak

#### Enable

```console
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Install LibreWolf, Spotify, LocalSend, and Amberol

```console
flatpak install flathub io.gitlab.librewolf-community
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub io.bassi.Amberol
```

## Services

### Enable

```console
sudo systemctl enable --now fstrim.timer
sudo systemctl enable --now libvirtd
sudo systemctl enable --now tuned
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now firewalld
sudo systemctl disable --now iscsi
```
