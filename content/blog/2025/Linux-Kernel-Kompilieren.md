---
title: "Wie kompiliert man den Linux Kernel?"
date: 2025-04-23
image: images/2025-thumbs/Kernel.png
tags: ['Linux']
draft: true
---

![](/images/2025-thumbs/Kernel.webp)

## Warum?

Wahrscheinlich willst du den Linux Kernel selber Kompilieren. Es gibt auch mehrere Gründe wie z.B.:
- **Bessere Performance*** (* = jenach Konfiguration & Hardware)
- Mehr Optionen/Flexibilität
- **Mehr Controlle über dein System**
- Neuere Kernel Version

Egal welcher Grund es ist, hier hast du eine Anleitung wie du den Linux Kernel kompilieren kannst!

## Prerequisiten & Vorab

**Als aller erstes musst du dein System und DICH vorbereiten.** 

Für dein System musst du einfach essentiale Bibliotheken (Libaries) und Programme installieren, wie genau diese heißen variiert jenach auf welcher Distribution du ihn Kompilierst. Für dich, einmal Tief ein atmen, ein bisschen geduld (am meisten bei schwacher Hardware) und bisschem mehr Zeit als du dir vorstellst. Btw, dieses Tutorial gilt am meisten für Debian (Testing), alles andere hat noch mal ein größeren Spielraum. ;-)

```
# Debian und darauf basierenten Distrobution (z.B. Ubuntu & Linux Mint)
sudo apt-get install bc binutils bison dwarves flex gcc git gnupg2 gzip libelf-dev libncurses5-dev libssl-dev make openssl pahole perl-base rsync tar xz-utils
```
Wenn du auf einer anderen Linux Distrobution bist, z.B. Fedora die einen anderen Package-manager hat, heißen die selben Programme wahrscheinlich ein bisschen anderes. In diesem Fall lies dir den Guide wie man den Linux Kernel Kompiliert von [IT'SFOSS](https://itsfoss.com/) der in den Quellen verlinkt ist durch!

**Mein Setup:**

**Distro:** Debian Testing (amd64), btw :-) & Kernel 6.14.3 (zum Kompilieren)

**PC:**

- CPU: AMD Ryzen 9 5950x
- MoB: MSI X570 MEG UNIFY
- GPU: Nvidia RTX 3070Ti
- RAM: Kingston Fury 64GB RAM
- SSD: Samsung NVME 970 EVO Plus 1TB
- PSU: EVGA 750GQ (750W Modular)
- 2 Monitore

Btw, du brauchst nicht starke Hardware, das Kompilieren geht aber schneller auf stärkerer Hardware! ;-)


### Download, Dekomprimieren, Extrahieren & Echtheit checken

Um den Linux Kernel zu Kompilieren brauchst du seinen Quellcode (btw, das gilt für alles was du kompilieren willst).
Den Quellcode bekommst von der [kernel.org](https://kernel.org/) Webseite. ;-)

![](/images/2025/Linux-Kernels-Website.png)

Du kannst einfach den **Neuesten Mainline Kernel**, der immer im großen **Gelben Button** ist downloaden!

Dadurch, dass ich Debian Testing nutze und ungefähr um die 60+ Linux Kernels kompiliert habe bis jetzt, habe ich natürlich bisschen Erfahrung gesammelt: *Eine Notiz von mir bevor 6.14.x erschien!*

> *In meinem fall habe ich mir den neuesten Longterm Kernel gedownloaded (6.12.19, in meinem fall), weil die Nvidia Treibern aktuell von Debians Repositorien (Stable & Testing) nicht mit 6.13.x kooperieren wollen! Meistens kannst du einfach den **Neuesten Mainline Kernel** (Der in dem Großen Gelben Button!) nutzen außer du weißt dieser funktioniert mit deiner aktuellen Konfiguration an Software nicht!*

Als nächstes musst du den Kernel Dekompremieren (mit XZ).
```sh
unxz linux-*.tar.xz
```

Danach ist es gut die Echtheit deines Kernels zu checken, aber um sie nach zu weißen brauchst du noch die Signatur deines Kernels.

```sh
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.14.3.tar.sign
```

*btw, du musst den Link anpassen an deine Kernel Version!* Danach kannst du fortfahren mit der Verifizierung. (Btw, evtl. musst du `gpg2` oder `gpg` installieren!)

```
gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org 
gpg2 --verify linux-*.tar.sign
```

Wenn du diese Nachricht siehst, dann ist die Signatur gut. (Auch wenn diese Warnung darunter steht! Am wichtigsten ist ob über `WARNING:`, **Good signatur** steht!)

```sh
jakub@Zuhause:~/Downloads$ gpg2 --verify linux-*.tar.sign
gpg: assuming signed data in 'linux-6.14.3.tar'
gpg: Signature made Sun Apr 20 10:24:30 2025 CEST
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

Die `.config` Datei ist die Konfigurations Datei für deinen Linux Kernel, wo Sachen wie z.B. welche Treiber soll der Kernel insich drinne haben, welche nur als Module oder welche Garnicht angegeben wird.

```sh
sudo cp /boot/config-"$(uname -r)" .config
```
Damit du jetzt deinen Kernel Optimieren kannst, kannst du mit diesen Kommands **entweder** deine grad eben Kopierte `.config` aktualisieren oder einfach eine Komplett neue erstellen.

```sh
# Aktualisiert die alte Kofigurationsdate
make olddefconfig

# Erstell eine neue .config mit standart Einstellungen
make defconfig
``` 

**Module Signatur deaktivieren (Für Debian & Ubuntu basierende Distrobution)**  
`./scripts/config --file .config --disable MODULE_SIG`  

Früher war es mal so: (pre ~6.5.x) 
> `./scripts/config --disable SYSTEM_TRUSTED_KEYS`  
> `./scripts/config --disable SYSTEM_REVOCATION_KEYS`

Menü zum konfiguren des Kernels

```sh
make menuconfig
```

Außerdem wenn genauere Video's über die Konfiguration und Optimierung des Linux Kernels sehen willst, sind hier 3 Gute Videos, natürlich auf Englisch für dich ;-) 

[Mental Outlaws Tutorial](https://www.youtube.com/watch?v=NVWVHiLx1sU) [Denshis Tutorial](https://www.youtube.com/watch?v=APQY0wUbBow) [DJ Wares Tutorial](https://www.youtube.com/watch?v=s95hjvFAE5g)


**Kernel (Um)benennen.** Damit sich Kernels nicht in Konflikt kommen  
`./scripts/config --file .config --set-str LOCALVERSION "-Jakub"`

Btw, du kannst gerne einfach deinen Namen/Username nutzten, deine Vorstellung ist hier das Limit, grundsätzlich. "-MrBean" 

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
sudo dkms autoinstall -k 6.14.3-Jakub
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
