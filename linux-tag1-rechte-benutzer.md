# Linux Tag 1: Setup, Rechte und Benutzer

## Lernumgebung
- MacBook Air M4 (ARM-Chip)
- Homebrew, Git, VS Code, GitHub
- UTM mit **Ubuntu Server ARM64** (nicht AMD/x86!)
- Snapshot als sauberer Ausgangsstand
- Immer auf den Prompt achten: `name@ubuntu-lab` = VM, `...@codespaces-...` = Codespace

## Erste Befehle
| Befehl | Bedeutung |
|--------|-----------|
| `whoami` | aktueller Benutzer |
| `pwd` | aktueller Ordner |
| `ls`, `ls -la` | Inhalt anzeigen (mit Details und versteckten Dateien) |
| `mkdir`, `cd` | Ordner anlegen, wechseln |
| `touch`, `nano`, `cat` | Datei anlegen, bearbeiten, anzeigen |
| `cp`, `mv`, `rm` | kopieren, verschieben/umbenennen, löschen (endgültig!) |
| `sudo apt update` / `upgrade` | Paketlisten aktualisieren / Updates installieren |

nano: **Strg+O** speichern (Enter bestätigen), **Strg+X** beenden.

## Rechte lesen
```
-rw-r--r-- 1 anna anna 120 Okt 29 08:30 notiz.txt
```
- 1. Zeichen: Typ (`-` Datei, `d` Ordner, `l` Link)
- danach drei Dreiergruppen: **Besitzer**, **Gruppe**, **andere**
- `r` = lesen, `w` = schreiben, `x` = ausführen

## Zahlenrechte
r = 4, w = 2, x = 1, pro Personenkreis addieren.

| Zahl | Rechte | typisch für |
|------|--------|-------------|
| 644 | rw-r--r-- | normale Datei |
| 664 | rw-rw-r-- | Datei bei umask 0002 |
| 600 | rw------- | private Datei |
| 640 | rw-r----- | Besitzer schreibt, Gruppe liest |
| 755 | rwxr-xr-x | Skripte, Ordner |
| 700 | rwx------ | privater Ordner |

## Rechte und Besitzer ändern
```
chmod u+x datei          # Besitzer darf ausführen
chmod 600 datei          # nur Besitzer darf lesen/schreiben
sudo chown test datei    # Besitzer ändern
sudo chown test:gruppe datei
```
- `chmod 777` ist fast nie die Lösung (Sicherheitsrisiko)
- Vorsicht mit `-R` (rekursiv)

## Benutzer
```
id                         # Benutzer-ID und Gruppen
groups                     # eigene Gruppen
sudo adduser test          # Benutzer anlegen
su - test                  # zu Benutzer test wechseln
exit                       # zurück
sudo deluser --remove-home test   # Benutzer samt Home löschen
```
- **root** = Super-Admin, **sudo** = einzelnen Befehl mit Admin-Rechten ausführen

## Ordnerrechte
- `r`: Inhalt auflisten
- `w`: Dateien anlegen, umbenennen, löschen
- `x`: hineinwechseln und auf Dateien zugreifen
- Ohne `x` auf dem Ordner: `ls` zeigt nur Namen mit Fragezeichen, Zugriff gibt "Permission denied"

## umask
Bestimmt die Standardrechte neuer Dateien.
- umask 0022 → Dateien 644, Ordner 755 (Codespace)
- umask 0002 → Dateien 664, Ordner 775 (meine Ubuntu-VM)

Prüfen mit `umask`.

## Ausführrecht (Skript)
```
nano skript.sh           # Inhalt: #!/bin/bash  und  echo Hallo vom Skript
./skript.sh              # Permission denied (kein x)
chmod +x skript.sh
./skript.sh              # läuft
```
`./` = im aktuellen Ordner ausführen.

## Was ich gelernt habe
- "Permission denied" kann das **richtige** Ergebnis sein
- Immer prüfen, in welchem System und als welcher Benutzer ich arbeite
- Vor Experimenten einen Snapshot machen
- Backslash und Sonderzeichen in der VM: Tastaturlayout prüfen oder per SSH vom Mac arbeiten

## Als Nächstes
Pakete und Dienste: `apt install`, `systemctl status/start/stop`, Webserver (nginx) ausprobieren.
