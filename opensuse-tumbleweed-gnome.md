# openSUSE Tumbleweed Post-Install Notes: GNOME

## Set hostname

```console
sudo hostnamectl hostname host.localdomain`
```

## Update system

```console
sudo zypper dup
```

## Software

### Core

```console
sudo zypper in android-tools htop nmap tmux memtest86+ plocate zsh neovim  git meson ninja python313-pipx bat fzf fastfetch alacritty mint-y-icon-theme opi gnome-calendar
```

### Graphics

```console
sudo zypper perl-Image-ExifTool dcraw libwebp-tools
```

### Internet

```console
sudo zypper in geary transmission-gtk
```

### Multimedia

```console
sudo opi codecs
sudo zypper in audacity decibels beets flac lame mpg123 mpg123-pulse mpv normalize python313-eyed3 yt-dlp cmus cmus-plugin-ffmpeg cmus-plugin-pulse
```

### Virtualization

```console
sudo zypper in libvirt-daemon virt-manager
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

#### Install LibreWolf, Spotify, LocalSend, Amberol, and Extension Manager

```console
flatpak install flathub io.gitlab.librewolf-community
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub io.bassi.Amberol
flatpak install flathub com.mattjakeman.ExtensionManager
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
sudo systemctl disable --now iscsi
```
