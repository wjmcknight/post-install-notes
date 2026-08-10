# openSUSE Leap 16 Post-Install Notes: GNOME

## Update system

```console
sudo zypper up
```

## Software

### Add repo for Alacritty

The version of Alacritty shipped with Leap 16 is outdated and using an
older configuration format that causes errors with my config so we get a
newer version here.

```console
sudo zypper ar https://download.opensuse.org/repositories/home:jlkDE/16.0/home:jlkDE.repo
sudo zypper ref
```

### Core

```console
sudo zypper in android-tools htop nmap tmux memtest86+ plocate zsh neovim  git meson ninja python313-pipx bat fzf fastfetch alacritty mint-y-icon-theme opi
```

### Graphics

```console
sudo zypper perl-Image-ExifTool dcraw libwebp-tools
```

### Internet

```console
sudo zypper in geary
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
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now firewalld
sudo systemctl disable --now iscsi
```
