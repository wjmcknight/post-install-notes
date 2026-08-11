# LMDE 7 Post-Install Notes

## Software

### Core

```console
sudo apt install tuned android-sdk-platform-tools-common htop nmap tmux memtest86+ zsh neovim git pipx bat fzf alacritty
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras libimage-exiftool-perl webp imagemagick
```

### Multimedia

```console
sudo apt install amberol audacity gnome-snapshot beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 cmus cmus-plugin-ffmpeg
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

#### Install Spotify, LocalSend, and Decibels

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub org.gnome.Decibels
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
sudo systemctl disable --now blueman-mechanism
sudo systemctl disable --now bluetooth
sudo systemctl disable --now open-iscsi
sudo systemctl disable --now openvpn
sudo systemctl disable --now ufw
```

## Disable `sudo` password feedback

```console
sudo cp /etc/sudoers.d/0pwfeedback /etc/sudoers.d/1nopwfeedback
sudo sed -i 's|pwfeedback|!pwfeedback|' /etc/sudoers.d/1nopwfeedback
```
