+++
title = 'Wie kompiliert man den Linux Kernel?'
date = 2025-03-10T20:21:34+01:00
tags = ['Linux']
draft = true
+++

**Warum?** 
Bessere Performance, Mehr Optionen & Controlle.

**Download & Extrahieren**  
Download (Kernel + Signatur), Decompress, Extrahieren (Tar) (erklärung)  
`unxz linux-*.tar.xz`

Signatur/Echtheit checken
`gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org`  
`gpg2 --verify linux-*.tar.sign`

Extrahieren (Tar)  
`tar -xf linux-*.tar`

**.config file**

`cd linux-*/`  
`sudo cp /boot/config-"$(uname -r)" .config`

`make olddefconfig` \*Aktualisiert die alte Kofigurationsdate  
`make defconfig` \*Erstell eine neue mit Standart einstellungen

**Module Signatur deaktivieren (Für Debian & Ubuntu basierenten Distrobution)**  
`./scripts/config --file .config --disable MODULE_SIG`  

Früher war es mal so: (pre ~6.5.x)
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

**DTBs (Device Tree Binaries) Nur für ARM & RISC-V**  
(Nicht im video!)

`make bzImage`  
`sudo make modules_install -j$(nproc)`  
`sudo make headers_install`

**DKMS \[Nvidia\]: Arch Wiki**  
`sudo dkms autoinstall -k` z.B. `sudo dkms autoinstall -k 6.12.18-Jakub`


`sudo make install`
Reboot in den neuen Kernel.

**Aufräumen: "make"** clean, mrproper oder distclean

**Deinstallation**

Entfernt kernel modules  
`sudo rm -rf /lib/modules/<kernel_version>`  
(Entfernt device-tree binaries)  
`sudo rm -rf /boot/dtb-<kernel_version>`  
Entfernt den Linux kernel  
`sudo rm -vf /boot/{config,System.map,initrd.img,vmlinuz}-<kernel_version>`
`sudo update-grub` # oder grubmkconfig

[Video]()
