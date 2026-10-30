# Linux Tag 3-4: Pakete, Dienste, nginx und Logs

## Pakete mit apt
Ein **Paket** ist Software samt Abhängigkeiten. `apt` ist der Paketmanager von Ubuntu.

| Befehl | Wirkung |
|--------|---------|
| `sudo apt update` | Paketlisten aktualisieren (holt nur Informationen) |
| `apt list --upgradable` | verfügbare Updates anzeigen |
| `sudo apt upgrade` | verfügbare Updates installieren |
| `apt search name` | nach Paketen suchen |
| `apt show name` | Infos zu einem Paket |
| `sudo apt install name` | Paket installieren |
| `sudo apt remove name` | Paket entfernen |

**Merksatz:** Erst `update` (Liste holen), dann `upgrade` oder `install` (Aktion ausführen).

Fehler "Unable to locate package": meistens `sudo apt update` vergessen.

## Dienste mit systemctl
Ein **Dienst** (Service) ist ein Programm, das im Hintergrund läuft, z. B. ein Webserver oder SSH.

| Befehl | Wirkung |
|--------|---------|
| `systemctl status name` | läuft der Dienst? (mit **q** verlassen) |
| `sudo systemctl start name` | starten |
| `sudo systemctl stop name` | stoppen |
| `sudo systemctl restart name` | neu starten |
| `sudo systemctl enable name` | automatisch beim Systemstart |
| `sudo systemctl disable name` | nicht mehr automatisch starten |

`start` und `stop` wirken sofort, `enable` und `disable` betreffen den nächsten Systemstart.

## Praxis: nginx
```
sudo apt install nginx
systemctl status nginx          # active (running)
curl localhost                  # Startseite als HTML-Text
sudo systemctl stop nginx
curl localhost                  # Connection refused
sudo systemctl start nginx
```

Eigene Startseite:
```
sudo nano /var/www/html/index.nginx-debian.html
```

Vom Mac im Browser: IP der VM mit `ip a` ermitteln (Zeile `inet` bei `enp0s...`) und im Browser aufrufen.

## Logs lesen (Fehlersuche)
```
journalctl -u nginx             # Log des Dienstes
journalctl -u nginx -n 20       # letzte 20 Zeilen
sudo tail /var/log/nginx/access.log   # Zugriffe auf den Webserver
```
Dort tauchen meine `curl`- und Browser-Zugriffe auf.

## Vorgehen bei Problemen
1. Status prüfen: `systemctl status name`
2. Logs lesen: `journalctl -u name`
3. Erst dann Änderungen vornehmen oder neu starten

## Was ich gelernt habe
- Software installiere ich mit `apt`, Dienste steuere ich mit `systemctl`
- Ein Webserver läuft als Dienst im Hintergrund und lässt sich starten, stoppen und neu starten
- Logs sind der erste Anlaufpunkt bei Fehlern: erst lesen, dann raten

## Als Nächstes
Dateien durchsuchen und verarbeiten: `grep`, `find`, `less`, `head`, `tail`, Pipes (`|`) und Umleitungen (`>`, `>>`), geübt an den nginx-Logs.
