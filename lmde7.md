# LMDE 7 

## Install Some Packages

### Core

```console
sudo apt install htop nmap tmux memtest86+ zsh vim vim-gtk3 uuid-runtime tuned haveged uptimed git build-essential python3-pip argyll icc-profiles 
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
sudo apt install audacity cheese handbrake sound-juicer beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 mpd mpc ncmpcpp
```

### Virtualization

```console
sudo apt install virt-manager 
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

### Install Spotify

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
```

## Services

### Disable

```console
sudo systemctl disable bluetooth --now
sudo systemctl disable blueman-mechanism --now
sudo systemctl disable mpd --now
sudo systemctl disable openvpn --now
sudo systemctl disable ufw --now
```

## Disable `sudo` Password Feedback

```console
sudo cp /etc/sudoers.d/0pwfeedback /etc/sudoers.d/1nopwfeedback
sudo sed -i 's|pwfeedback|!pwfeedback|' /etc/sudoers.d/1nopwfeedback
```
