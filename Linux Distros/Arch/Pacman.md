# Arch Linux Packages

## Introduction  
Arch linux repo configuration.  
**journalctl** is a command-line utility in Linux used to view and manage logs stored by the systemd systemd-journald service.  



## Fix arch linux plasma cannot login after update

Reinstall the packages
```sh
sudo pacman -Syu --overwrite \* $(pacman -Qnq)
```

1. `sudo pacman -Syu`: This command synchronizes the package databases, updates all the packages on the system, and upgrades the packages to the latest versions.

2. `--overwrite \*`: This option tells `pacman` to overwrite any existing files, regardless of which package owns them. This can be risky because it may overwrite important system files and potentially break your system.
- will force the update even if there are file conflicts. This can solve some issues where files from different packages conflict, but it can also create new issues if important files are replaced incorrectly.

3. `$(pacman -Qnq)`: This command gets a list of all the explicitly installed packages (i.e., packages that were manually installed and not installed as dependencies) in a format suitable for passing as arguments to another command.
- Reinstalling all explicitly installed packages: $(pacman -Qnq) will list all explicitly installed packages, and pacman -Syu will reinstall them.

## Reference  
1. [Systemd wiki](https://en.wikipedia.org/wiki/Systemd)
2. [Systemd timers ArchWiki](https://wiki.archlinux.org/title/Systemd/Timers)
3. [Systemd service ArchWiki](https://wiki.archlinux.org/title/Systemd)
4. [Arch Linux packages](https://archlinux.org/packages/)
5. [Journal ArchWiki](https://wiki.archlinux.org/title/Systemd/Journal)
