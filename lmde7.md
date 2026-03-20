# LMDE 7 

## Software

### Core

```console
sudo apt install htop nmap tmux memtest86+ zsh vim vim-gtk3 haveged uptimed git build-essential python3-pip argyll icc-profiles bat fzf 
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras inkscape feh scrot libimage-exiftool-perl
```

### Internet

```console
sudo apt install chromium filezilla
```

### Multimedia

```console
sudo apt install audacity cheese handbrake sound-juicer beets python3-flask ffmpeg flac lame mpg123 mpv normalize-audio eyed3 mpd mpc ncmpcpp
```

### Virtualization

```console
sudo apt install virt-manager 
```

#### Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

### Flatpak

#### Install Spotify and Zola

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
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
sudo systemctl enable fstrim.timer --now
```

### Disable

```console
sudo systemctl disable mpd --now
sudo systemctl disable openvpn --now
sudo systemctl disable ufw --now
```

## Disable `sudo` Password Feedback

```console
sudo cp /etc/sudoers.d/0pwfeedback /etc/sudoers.d/1nopwfeedback
sudo sed -i 's|pwfeedback|!pwfeedback|' /etc/sudoers.d/1nopwfeedback
```
