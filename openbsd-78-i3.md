# OpenBSD 7.8 Post-Install Notes: i3

## Fix a syspatch Failure and Enable doas Access

First log in as root and then we'll never have to again after this.

```console
sed -e 's|.checkfs|#checkfs|g' /usr/sbin/syspatch > /root/syspatch
ksh /root/syspatch
syspatch
rm /root/syspatch
dev_mkdb
echo "permit persist :wheel" >> /etc/doas.conf
```

Log out then log back in with your regular user.

## Update System

```console
doas syspatch
```

## Install Some Packages

### Core

```console
doas pkg_add htop nano nmap zsh vim i3 i3lock pulseaudio pavucontrol xfce4-settings xfce4-terminal thunar nitrogen picom git py3-pip xautolock galculator faba-icon-theme papirus-icon-theme greybird
```

### Graphics

```console
doas pkg_add gimp gimp-lensfun-plugin inkscape ristretto feh scrot p5-Image-ExifTool dcraw
```

### Internet

```console
doas pkg_add ungoogled-chromium filezilla firefox-esr geary transmission-gtk
```

### Multimedia

```console
doas pkg_add deadbeef audacity handbrake parole beets ffmpeg mpv normalize eyeD3 yt-dlp gst-plugins-ugly mpd mpc ncmpcpp
```

### Office

```console
doas pkg_add libreoffice atril
```

## Manage Services

### Enable

```console
doas rcctl enable apmd
doas rcctl enable messagebus
doas rcctl enable xenodm
```

### Disable

```console
doas pfctl -d
doas rcctl disable smtpd
```
