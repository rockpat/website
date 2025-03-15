+++
title = 'Wie kompiliert man den Linux Kernel?'
date = 2025-03-15
tags = ['Linux']
draft = true
+++

## Warum? 
Bessere Performance, Mehr Optionen & Controlle.

## Prerequisiten & Vorab
Dependencies!!!

**Mein Setup:**

**Distro:** Debian Testing & Kernel 6.12.19 (zum Kompilieren)

**PC:**

- AMD Ryzen 9 5950x
- MSI X570 MEG UNIFY
- Nvidia RTX 3070Ti
- Kingston Fury 64GB RAM
- Samsung NVME 970 EVO Plus 1TB
- EVGA 750GQ (750W Modular)
- 2 Monitore

Btw, du brauchst nicht starke Hardware, das Kompilieren geht aber schneller auf stärkerer Hardware! ;-)


**Download & Extrahieren**  
Download (Kernel + Signatur), Decompress, Extrahieren (Tar) (erklärung)  
`unxz linux-*.tar.xz`

Signatur/Echtheit checken
`gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org` 
`gpg2 --verify linux-*.tar.sign`

Extrahieren (Tar)  
`tar -xf linux-*.tar`

## Optimieren des Linux Kernels (für dein System)

**.config file**

`cd linux-*/`  
`sudo cp /boot/config-"$(uname -r)" .config`

`make olddefconfig` \*Aktualisiert die alte Kofigurationsdate  
`make defconfig` \*Erstell eine neue mit Standart einstellungen

**Module Signatur deaktivieren (Für Debian & Ubuntu basierenten Distrobution)**  
`./scripts/config --file .config --disable MODULE_SIG`  

Früher war es mal so: (pre ~6.5.x) no language code,
> `$ scripts/config --disable SYSTEM_TRUSTED_KEYS`  
> `$ scripts/config --disable SYSTEM_REVOCATION_KEYS`

`make menuconfig` \*Menü zum konfiguren des Kernels

**Kernel benennen** \*Damit sich Kernels nicht in Konflikt kommen  
`./scripts/config --file .config --set-str LOCALVERSION "-Jakub"`

**Das Kompilieren**  
`make -j$(nproc) 2>&1 | tee log`

\-j = wieviele Kompilier jobs gleichzeitig laufen können.  
nproc = Gibt an Wieviele jobs zuverfügung stehen.  
Also "-j$(nproc)" bedeutet: also „nutz so viele parallele Kompilierungsjobs, wie viele CPU-Threads ich habe“.  
tee log = Log file

## DTBs (Device Tree Binaries)

**Nur für ARM & RISC-V, also bei x86-64 Komputern kann man diesen Teil skippen!**  

(Nicht im video!)


## Weiteres Kompilieren
`make bzImage`  
`sudo make modules_install -j$(nproc)`  
`sudo make headers_install`

## DKMS (Nvidia & ZFS Nutzer!)

"Dynamic Kernel Modules"

`sudo dkms autoinstall -k` z.B. `sudo dkms autoinstall -k 6.12.19-Jakub`

Für mehr info lies dir die [DKMS Archwiki](https://wiki.archlinux.org/title/Dynamic_Kernel_Module_Support) Artikel durch!

`sudo make install`
Reboot in den neuen Kernel.


## Deinstallation

**Aufräumen: "make"** clean, mrproper oder distclean

Entfernt kernel modules  
`sudo rm -rf /lib/modules/<kernel_version>`  
(Entfernt device-tree binaries)  
`sudo rm -rf /boot/dtb-<kernel_version>`  
Entfernt den Linux kernel  
```
sudo rm -vf /boot/{config,System.map,initrd.img,vmlinuz}-<kernel_version>

sudo update-grub # oder sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Quellen:
[It's Foss Guide: How to compile the Linux Kernel (6.5.5)](https://itsfoss.com/compile-linux-kernel/) (Englisch)

Meine eigene Erfahrung ;-)


## Video

{{< youtube "aIJaxy4r_uA" >}}
