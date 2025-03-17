+++
title = 'Wie kompiliert man den Linux Kernel?'
date = 2025-03-15
tags = ['Linux']
draft = true
+++

## Warum?

Wahrscheinlich willst du den Linux Kernel selber Kompilieren. Es gibt auch mehrere Gründe wie z.B.:
- **Bessere Performance*** (* = jenach Konfiguration & Hardware)
- Mehr Optionen/Flexibilität
- **Mehr Controlle über dein System**
- usw.

Egal welcher Grund es ist, hier hast du eine Anleitung wie du den Linux Kernel kompilieren kannst!

## Prerequisiten & Vorab

**Als aller erstes musst du dein System und dich selbst vorbereiten.** 

Für dein System musst du einfach essentiale Bibliotheken (Libaries) und Programme installieren, wie genau diese heißen variiert jenach auch welcher Distribution du ihn Kompilierst. Für dich, einmal Tief ein atmen, ein bisschen geduld (am meisten bei schwacher Hardware) und bisschem mehr Zeit als du dir vorstellst. Btw, dieses Tutorial gilt am meisten für Debian (Testing), alles andere hat noch mal einen größeren Spielraum. ;-)

```
# Debian und darauf basierenten Distrobution (z.B. Ubuntu & Linux Mint)
sudo apt-get install bc binutils bison dwarves flex gcc git gnupg2 gzip libelf-dev libncurses5-dev libssl-dev make openssl pahole perl-base rsync tar xz-utils
```

**Mein Setup:**

**Distro:** Debian Testing, btw :-) & Kernel 6.12.19 (zum Kompilieren)

**PC:**

- AMD Ryzen 9 5950x
- MSI X570 MEG UNIFY
- Nvidia RTX 3070Ti
- Kingston Fury 64GB RAM
- Samsung NVME 970 EVO Plus 1TB
- EVGA 750GQ (750W Modular)
- 2 Monitore

Btw, du brauchst nicht starke Hardware, das Kompilieren geht aber schneller auf stärkerer Hardware! ;-)


### Download, Dekomprimieren, Extrahieren & Echtheit checken

Um den Linux Kernel zu Kompilieren brauchst du seinen Quellcode (btw, das gilt für alles was du kompilieren willst).
Den Quellcode bekommst von der [kernel.org](https://kernel.org/) Webseite. ;-)

![](/images/2025/Linux-Kernel-Website.png)

In meinem fall habe ich mir den neuesten Longterm Kernel gedownloaded (6.12.19, in meinem fall), weil die Nvidia Treibern aktuell von Debians Repositorien (Stable & Testing) nicht mit 6.13.x kooperieren wollen! Meistens kann du einfach den Neuesten Mainline Kernel 

Download (Kernel + Signatur), Decompress, Extrahieren (Tar) (erklärung)  
`unxz linux-*.tar.xz`

Signatur/Echtheit checken
`gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org` 
`gpg2 --verify linux-*.tar.sign`

Extrahieren (Tar)  
`tar -xf linux-*.tar`

### Optimieren des Linux Kernels (für dein System)

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

### DTBs (Device Tree Binaries)

**Nur für ARM & RISC-V, also bei x86-64 Komputern kann man diesen Teil skippen!**  

(Nicht im video!)


### Weiteres Kompilieren

`make bzImage`  
`sudo make modules_install -j$(nproc)`  
`sudo make headers_install`

### DKMS (Nvidia & ZFS Nutzer!)

"Dynamic Kernel Modules"

`sudo dkms autoinstall -k` z.B. `sudo dkms autoinstall -k 6.12.19-Jakub`

Für mehr info lies dir die [DKMS Archwiki](https://wiki.archlinux.org/title/Dynamic_Kernel_Module_Support) Artikel durch!

`sudo make install`
Reboot in den neuen Kernel.


### Deinstallation

**Aufräumen: "make"** clean, mrproper oder distclean

**Entfernt Kernel modules**

`sudo rm -rf /lib/modules/<kernel_version>`  

**(Entfernt device-tree binaries)**

`sudo rm -rf /boot/dtb-<kernel_version>`  

**Entfernt den Linux Kernel** 
```
sudo rm -vf /boot/{config,System.map,initrd.img,vmlinuz}-<kernel_version>

sudo update-grub # oder sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Quellen
[It's Foss Guide: How to compile the Linux Kernel (6.5.5)](https://itsfoss.com/compile-linux-kernel/) (Englisch)

Meine eigene Erfahrung ;-)


## Video

{{< youtube "aIJaxy4r_uA" >}}
