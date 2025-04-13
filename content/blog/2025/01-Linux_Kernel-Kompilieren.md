+++
title = 'Wie kompiliert man den Linux Kernel?'
date = 2025-04-13
image = 'images/2025-thumbs/Kernel.webp'
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
:q
**Als aller erstes musst du dein System und dich selbst vorbereiten.** 

Für dein System musst du einfach essentiale Bibliotheken (Libaries) und Programme installieren, wie genau diese heißen variiert jenach auf welcher Distribution du ihn Kompilierst. Für dich, einmal Tief ein atmen, ein bisschen geduld (am meisten bei schwacher Hardware) und bisschem mehr Zeit als du dir vorstellst. Btw, dieses Tutorial gilt am meisten für Debian (Testing), alles andere hat noch mal ein größeren Spielraum. ;-)

```
# Debian und darauf basierenten Distrobution (z.B. Ubuntu & Linux Mint)
sudo apt-get install bc binutils bison dwarves flex gcc git gnupg2 gzip libelf-dev libncurses5-dev libssl-dev make openssl pahole perl-base rsync tar xz-utils
```

**Mein Setup:**

**Distro:** Debian Testing, btw :-) & Kernel 6.14.3 (zum Kompilieren)

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

In meinem fall habe ich mir den neuesten Longterm Kernel gedownloaded (6.12.19, in meinem fall), weil die Nvidia Treibern aktuell von Debians Repositorien (Stable & Testing) nicht mit 6.13.x kooperieren wollen! Meistens kannst du einfach den **Neuesten Mainline Kernel** (Der in dem Großen Gelben Button!) nutzen außer du weißt er funktioniert mit deiner aktuellen Konfiguration an Software nicht!

Als nächstes musst du den Kernel Dekompremieren (mit XZ).
```sh
unxz linux-*.tar.xz
```

Danach ist es gut die Echtheit deines Kernels zu checken, aber um sie nach zu weißen brauchst du noch die Signatur deines Kernels.

```sh
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.14.2.tar.sign
```

*btw, du musst den Link anpassen an deine Kernel Version!* Danach kannst du fortfahren mit der Verifizierung. (Btw, evtl. musst du `gpg2` oder `gpg` installieren!)

```
gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org 
gpg2 --verify linux-*.tar.sign
```

Wenn du diese Nachricht siehst, dann ist die Signatur gut. (Auch wenn diese Warnung darunter steht! Am wichtigsten ist ob über `WARNING:`, **Good signatur** steht!)

```sh
jakub@Zuhause:~/Downloads$ gpg2 --verify linux-*.tar.sign                                         
gpg: assuming signed data in 'linux-6.14.1.tar'
gpg: Signature made Mon Apr  7 10:11:54 2025 CEST
gpg:                using RSA key 647F28654894E3BD457199BE38DBBDC86092693E
gpg: Good signature from "Greg Kroah-Hartman <gregkh@kernel.org>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 647F 2865 4894 E3BD 4571  99BE 38DB BDC8 6092 693E
```


Jetzt musst deinen Kernel Extrahieren.

```sh
tar -xf linux-*.tar
```

Als letztes für diesen Schritt muss du noch in den Ordner deines Kernels gehen.
```sh
cd linux-*/
```  

### Optimieren des Linux Kernels (für dein System)

[Mental Outlaws Tutorial](https://www.youtube.com/watch?v=NVWVHiLx1sU)
[Denshis Tutorial](https://www.youtube.com/watch?v=APQY0wUbBow)

**.config file**


```sh
sudo cp /boot/config-"$(uname -r)" .config
```

\*Aktualisiert die alte Kofigurationsdate
```sh
make olddefconfig
``` 

\*Erstell eine neue mit Standart einstellungen
```sh
make defconfig
``` 

**Module Signatur deaktivieren (Für Debian & Ubuntu basierenten Distrobution)**  
`./scripts/config --file .config --disable MODULE_SIG`  

Früher war es mal so: (pre ~6.5.x) 
> `$ scripts/config --disable SYSTEM_TRUSTED_KEYS`  
> `$ scripts/config --disable SYSTEM_REVOCATION_KEYS`

Menü zum konfiguren des Kernels

```sh
make menuconfig
```

**Kernel (Um)benennen.** \*Damit sich Kernels nicht in Konflikt kommen  
`./scripts/config --file .config --set-str LOCALVERSION "-Jakub"`

**Das Kompilieren**  
```sh
make -j$(nproc) 2>&1 | tee log
```

`\-j` = wieviele Kompilier jobs gleichzeitig laufen können.  
`$(nproc)` = Gibt an Wieviele jobs zuverfügung stehen.  
Also `-j$(nproc)` bedeutet: nutz so viele CPU-Threads, wie viele CPU-Threads ich habe.
`tee log` = Erstellt eine Log-Datei

### DTBs (Device Tree Binaries)

**Nur für ARM & RISC-V, also bei x86-64 Komputern kann man diesen Teil skippen!**  

(Nicht im video!)


### Weiteres Kompilieren

`make bzImage`  
`sudo make modules_install -j$(nproc)`  
`sudo make headers_install`

### DKMS (Nvidia & ZFS Nutzer!)

"**D**ynamic **K**ernel **M**odule**s**", sind wie der Name frei gibt, "Dynamische" Kernel Module die außerhalb des Kernel eingebunden werden können. Die bekannteste anwendung für DKMS sind die Nvidia Treiber. **Wenn du eine Nvidia Grafikkarte hast und die Treiber von ihr installiert hast und die DKMS nicht baust, dann wir dein Kernel nicht Funktionieren!**

```sh
sudo dkms autoinstall -k #VERSION-NAME
``` 
in Praktischer nutzung würde es so Aussehen.

```sh
sudo dkms autoinstall -k 6.14.2-Jakub
```

**Für mehr infos, lies dir den [DKMS Archwiki](https://wiki.archlinux.org/title/Dynamic_Kernel_Module_Support) Artikel durch!**

```sh
sudo make install
```
Reboot in deinen neuen Kernel. ;-)


### Deinstallation

Wenn du dein selbst Kompilierten Kernel entfernen willst, nutzt folgene Kommands.
!!!! **Vergiss nicht** `<kernel_version>` mit deiner echten Kernel Version zu tauschen.

**Entfernt Kernel modules**

```sh
sudo rm -rf /lib/modules/<kernel_version>
```

**(Entfernt device-tree binaries)**

```sh
sudo rm -rf /boot/dtb-<kernel_version>
```

**Entfernt den Linux Kernel und generiert eine neue GRUB konfiguration, ohne den eben entfernten Kernel.** 
```
sudo rm -vf /boot/{config,System.map,initrd.img,vmlinuz}-<kernel_version>

sudo update-grub # oder sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Quellen
[It's Foss Guide: How to compile the Linux Kernel (6.5.5)](https://itsfoss.com/compile-linux-kernel/) (In Englisch)


Meine eigene Erfahrung ;-)


## Video

{{< youtube "aIJaxy4r_uA" >}}
