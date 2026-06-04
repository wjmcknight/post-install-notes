# LMDE 7 Post-Install Notes

## Software

### Core

```console
sudo apt install htop nmap tmux memtest86+ zsh neovim haveged uptimed git pipx argyll icc-profiles alacritty bat fzf
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras inkscape libimage-exiftool-perl
```

### Internet

```console
sudo apt install chromium filezilla
```

### Multimedia

```console
sudo apt install audacity cheese beets python3-flask ffmpeg flac lame mpg123 mpv normalize-audio eyed3 mpd mpc ncmpcpp
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
sudo systemctl disable --now mpd
sudo systemctl disable --now openvpn
sudo systemctl disable --now ufw
```

## Disable `sudo` password feedback

```console
sudo cp /etc/sudoers.d/0pwfeedback /etc/sudoers.d/1nopwfeedback
sudo sed -i 's|pwfeedback|!pwfeedback|' /etc/sudoers.d/1nopwfeedback
```
