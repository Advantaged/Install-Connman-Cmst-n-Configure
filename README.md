# Install-Connman-Cmst-n-Configure
Use Connman instead of NetworkManager

**Merits** **&** **Notes:** [Artix-Linux_Connaman](https://wiki.artixlinux.org/Main/Installation#OpenRC), [CMST-Git](https://github.com/andrew-bibb/cmst), [CMST-Settings](https://manpages.ubuntu.com/manpages/bionic/man1/cmst.1.html), [OpenRC-Wiki (including "SystemD")](https://wiki.archlinux.org/title/OpenRC).

**Why:** I use `connman`, `cmst` and correlated "daemon" `connmand` for switching off the network-connection without to detach the cable or shutdown the PC. This most useful by notebooks not having a cable. You can use as front-end-GUI even `connman-gtk` instead of `cmst`.

### 1. Important Notes/Advices
1. These instructions are valid for every Linux-OS and every "Init-System". See in your Forum to get the additional packages if your OS is not an Arch or a such spin-off/derivate.
2. To accomplish the target with/under another "Init-System", see correlated init-system-wiki. Correlated commands for "SystemD" are in the fourth link above.
3. At end we replace `networkmanager` and `NetworkManager` or `libnm` and `libnma`, but, PLEASE, DON'T UNINSTALL/REMOVER THESE PACKAGES, NEVER!
4. A lot of packages have `networkmanager` as dependency, hence, the system can live without `connman` but not (yet) without `networkmanager`.

### 2. Install apps:
[code] as user [/code]
```
paru -S connman-openrc cmst
```
### 3. Add, start & check services/daemon
[code] as admin, use `sudo -i` [/code]
```
rc-update -v show | grep connmand

rc-update add connmand default

rc-update -v show | grep connmand

rc-service connmand start

rc-status --servicelist -s | grep connmand
```
### 4. Configure GUI-Starter
[code] use editor as user [/code]
- All I have done is to add option `-m` to exec-line
- In case the file not yet exist, create it.
- Complete configuration of `~/.config/autostart/cmst-autostart.desktop`

```
[Desktop Entry]
Type=Application
Version=1.0
Name=Connman UI Setup
GenericName=Network Configuration
Comment=QT GUI frontend for connman
Categories=Settings;System;Qt;Network;
Icon=cmst
Exec=cmst -w5 -m
Terminal=false
StartupNotify=false
X-GNOME-Autostart-enabled=true
Keywords=Network;Wireless;Wi-Fi;Wifi;IP;LAN;Proxy;WAN;Broadband;Bluetooth;vpn;DNS;

Name[de]=Netzwerk-Konfiguration

```
### 5. Stop, Remove & Check "NetworkManager"
[code] admin only with `sudo -i` [/code]
- Each command (4 totally) contain two lines, the 1st is the command same and the 2nd the output you should get as answer.

```
rc-service NetworkManager stop
NetworkManager    | * Stopping NetworkManager ...

rc-update del NetworkManager
 * service NetworkManager removed from runlevel default
 
rc-update -v show | grep NetworkManager
       NetworkManager |   
       
rc-status --servicelist -s | grep NetworkManager
 NetworkManager                                                    [  stopped  ]

```

[x] **Done!** **&** **Enjoy**
