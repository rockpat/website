---
title: 'TruckersMP auf Linux'
date: 2026-09-01
image: images/2025-thumbs/TruckersMP-Linux.webp
tags: ['Linux']
draft: true
---

![](/images/s)

Hier ist ein kurzes Tutorial wie ich TruckersMP unter Linux zum laufen bekommen habe:


### Vorab

Da ich **Debian Testing nutze**, konnte ich leider nicht (zumindestens nicht einfach) den **[Steam Guide](https://steamcommunity.com/sharedfiles/filedetails/?id=2800355160)** wie man **TruckersMP unter Linux** zum laufen bekommt nutzen, da Debian's Testing Repositorium eine wichtige Dependancy für **SteamTinkerClient**, yad, anderes "versioniert" (0.40.x ≠ 14.x), wodurch ich es anderes gemacht habe, **grundsätzlich** würde ich aber eigentlich den Steam Guide empfehlen, so lange du nicht, dass selbe Problem hast wie ich. Die Entscheidung liegt bei dir :-)


## 1. Downloade TruckersMP

Melde dich mit deinem **[TruckersMP Account](https://truckersmp.com/auth/login)** an & lade den .exe **Installer [herunter.](https://truckersmp.com/download)**

## 2. Füge es unter "Add non-Steam game" hinzu

![](/images/screenshot)

**Führe den Installer aus & warte, geduldig :-)**


![](/images/screenshot)


## 3. Fine tuning & Aufräumen

Wenn dir das **TruckersMP Menu** angezeigt wird, müssen wir **TMP** noch final konfigurieren & noch am besten aufräumen, in dem wir:

Du kannst zum **Darkmode** wechseln, in dem du im Menu über die Optionen zum Theme scrollst.

Passe die Steam Paths in TruckersMP an.

![](/images/screenshot)


> `TruckersMP > Manage > Properties > Shortcut (Target)`
```sh
$HOME/.steam/debian-installation/steamapps/compatdata/3197606417/pfx/drive_c/users/steamuser/AppData/Local/TruckersMP/TruckersMP-Launcher.exe
```

Damit Steam das installierte TruckersMP ausführt

> `TruckersMP > Manage > Properties > Customaztion > Artwork`

Ändere & pass alle 4 Artworks, damit TruckersMP in deiner Steam Bibliothek nicht schäbig aussieht.

Et voila, jetzt läuft TruckersMP auf deinem Linux!

# Video

{{< youtube "EFYdM61Jqmk" >}}
