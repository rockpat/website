---
title: 'TruckersMP auf Linux'
date: 2026-09-05
cover:
    image: images/2026-thumbs/TruckersMP.webp
tags: ['Linux']
draft: false
---

### Vorab

Da ich **Debian Testing nutze**, konnte ich leider nicht (zumindestens nicht einfach) den **[Steam Guide](https://steamcommunity.com/sharedfiles/filedetails/?id=2800355160)** wie man **TruckersMP unter Linux** zum laufen bekommt nutzen, da Debian's Testing Repositorium eine wichtige Dependancy für **SteamTinkerClient**, yad, anderes "versioniert" (0.40.x ≠ 14.x), wodurch ich es anderes gemacht habe, **grundsätzlich** würde ich aber eigentlich den Steam Guide empfehlen, so lange du nicht, dass selbe Problem hast wie ich. Die Entscheidung liegt bei dir :-)


## 1. Downloade TruckersMP

Melde dich mit deinem **[TruckersMP Account](https://truckersmp.com/auth/login)** an & lade den .exe **Installer [herunter.](https://truckersmp.com/download)**

## 2. Füge es unter "Add non-Steam game" hinzu

![](/images/2026/Steam-add_non_steam_game.webp)

Klick danach den "durchsuchen" Button an, füge die TruckersMP installer .exe hinzu & führe den Installer aus, in du auf Spiel starten drückst **& warte, geduldig :-)**

![](/images/2026/TruckersMP_starting.webp)

## 3. Fine tuning & Aufräumen

![](/images/2026/TruckersMP.webp)

Wenn dir das **TruckersMP Menu** angezeigt wird, müssen wir **TMP** noch final konfigurieren und noch am besten aufräumen, in dem du:

zum **Darkmode** wechselst, in dem du in den TMP Einstellungen unter "Theme" mit den Pfeiltasten hoch oder runter navigierst.

**Passe die Steam Paths in TruckersMP an.**

Bei mir schaut es z.B. so aus:

```
Z:\home\jakub\.steam\debian-installation\steamapps\common\Euro Truck Simulator 2
```

**Sehr WICHTIG: ändere das unter Steam**

> `TruckersMP > Manage > Properties > Shortcut (Target)`
```sh
$HOME/.steam/debian-installation/steamapps/compatdata/3197606417/pfx/drive_c/users/steamuser/AppData/Local/TruckersMP/TruckersMP-Launcher.exe
```

Damit Steam das installierte TruckersMP ausführt, nicht immer den installer, den du nach diesem Schritt löschen kannst.

> `TruckersMP > Manage > Properties > Customaztion > Artwork`

Zu aller letzt, ändere & passe alle 4 Artworks, damit TruckersMP in deiner Steam Bibliothek nicht schäbig aussieht.

**Et voila, jetzt läuft TruckersMP auf deinem Linux!**

# Video

{{< youtube "EFYdM61Jqmk" >}}
