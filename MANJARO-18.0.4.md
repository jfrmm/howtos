[< Back](./README.md)

# Manjaro 18.0.4

# Table of contents

- [Git](#git)
- [Docker](#docker)
- [Nice to haves](#nice-to-haves)
  - [Enable AUR](#enable-aur)
  - [Backups](#backups)
  - [VS Code](#vs-code)
  - [Fonts](#fonts)

## Git

Install Git and Git Flow

```
sudo pacman -Syu git
pamac build gitflow-avh
```

To avoid "ghost changes" when dual-booting Windows/Linux, boot to Linux and

```
git config --global core.autocrlf input
```

Then, boot to Windows and

```
git config --global core.autocrlf true
```

## Docker

Install and enable Docker service

```
sudo pacman -Syu docker docker-compose
sudo systemctl start docker
sudo systemctl enable docker
```

Add your user to the `docker` group, and restart

```
sudo usermod -aG docker $USER
```

## Nice to haves

### Enable AUR

Enable AUR in `Package Manager > Preferences > AUR > Enable > Check for updates`

> Learn more about AUR [here](https://wiki.manjaro.org/index.php?title=Arch_User_Repository)

### Backups

Install Timeshift (may use defaults only)

```
sudo pacman -Syu timeshift
```

and enable cron

```
sudo systemctl start cronie
sudo systemctl enable cronie
```

### VS Code

Install VS Code from AUR

```
pamac build visual-studio-code-bin
```

### Fonts

Install Fira Code, a nice ligature compatible font

```
sudo pacman -Syu otf-fira-code
```

Copyright 2019 jfrmm.
