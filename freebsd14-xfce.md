# FreeBSD 14 Post-Install Notes: XFCE

## Update System and Reboot

```console
su -
freebsd-update fetch
freebsd-update install
reboot
```

## Initialize pkg then Install and Setup `sudo`

```console
su -
pkg update
pkg install sudo
```

Next we'll need to run `visudo` and uncomment the following line:

```console
# %wheel ALL=(ALL:ALL) ALL
```

Logging out then back in and we'll use `sudo` from here on out for anything
requiring elevated privilges.

## Install Some Packages

### Xorg and XFCE

```console
sudo pkg install xorg xfce dbus drm-kmod xf86-input-evdev xf86-video-amdgpu
sudo sysrc kld_list="amdgpu"
sudo sysrc dbus_enable="yes"
sudo service dbus start
```

### Additional Step for VM Usage

I couldn't get the mouse to work by itself when installing in QEMU but this
fixes it:

```console
sudo pkg install utouch-kmod
echo 'utouch_load="yes"' | sudo tee -a /boot/loader.conf
```

### Core

```console
sudo pkg install htop nano nmap tmux plocate zsh vim vim-gtk3 uptimed lightdm lightdm-gtk-greeter pulseaudio pavucontrol git py311-pip ruby32-gems zola argyllcms icc-profiles-adobe-cs4 icc-profiles-basiccolor icc-profiles-openicc conky rofi xfce4-places-plugin xfce4-pulseaudio-plugin xfce4-whiskermenu-plugin galculator papirus-icon-theme
```

### Graphics

```console
sudo pkg install gimp gimp-data-extras gimp-lensfun-plugin inkscape feh scrot p5-Image-ExifTool dcraw ristretto
```

### Internet

```console
sudo pkg install chromium filezilla firefox geary transmission-gtk
```

### Multimedia

```console
sudo pkg install audacity handbrake parole pragha sound-juicer beets ffmpeg flac lame mpg123 mpv normalize py311-eyed3 yt-dlp gstreamer1-plugins-good gstreamer1-plugins-pulse gstreamer1-plugins-ugly gstreamer1-vaapi cmus
```

## Services

### Enable

```console
sudo sysrc cupsd_enable="yes"
sudo service cupsd start
sudo sysrc lightdm_enable="yes"
sudo service lightdm start
sudo sysrc uptimed_enable="yes"
sudo service uptimed start
```
