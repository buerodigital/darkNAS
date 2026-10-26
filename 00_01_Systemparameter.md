# 00_01 Systemparameter und Leseregeln

<!-- id: 00_01; version: 1.3; updated: 2026-10-26 -->

Diese Anleitung führt von einem Windows-PC bis zum privaten NAS mit Samba und optionalen Webcontainern. Jedes Stichwort liegt als `README.md` in einem eigenen Unterpunktordner vor. Befehle und Konfigurationen stehen in kopierbaren Codeblöcken; gemeinsame Parameter werden zentral gepflegt; jede Stichwortausarbeitung enthält ihre Quellen-URLs direkt. Die beschriebenen Abläufe wurden anhand der Dokumentation geprüft, aber nicht vollständig auf einem realen DarkNAS ausgeführt. Erwartete Ausgaben sind Beispiele und können je nach Paketstand abweichen.

## Verbindliche Entscheidungen

- Hostname: darknas. Einziges Administrationskonto: darkroot, Home: /home/darkroot.
- Im Installer bleibt das root-Passwort leer. Debian sperrt den direkten root-Zugang und richtet sudo für darkroot ein. Die Einrichtung und Prüfung stehen vollständig in 01_02 Grundinstallation Debian; ein separates Kapitel zur sudo-Installation entfällt. [Q02] [Q08]
- Hostadministration erfolgt als darkroot mit sudo, ohne root-Login und ohne dauerhafte root-Shell. Bestätigte Ausnahme: rootless Podman, systemctl --user und Arbeiten an eigenen Containerdateien laufen als darkroot OHNE sudo. Sonst würden andere, rootful Container und Volumes angesprochen. PowerShell und Installer-Menüs benötigen kein Linux-sudo. [Q26]
- Bestehende Daten bleiben erhalten. Ein LVM-Neuaufbau ist ein gesonderter, löschender Weg ausschließlich für eine leere Datenplatte.
- Nginx Proxy Manager (NPM) ist die bestätigte Lesart von „Network Proxy Manager“. Nur die Webdienste werden veröffentlicht; SSH und Samba bleiben im Heimnetz.
- Die beiden Überschriften „Voraussetzungen“ bleiben absichtlich gleich. Abschnitt 2 enthält die Vorbereitung, Abschnitt 3 den Sicherheitscheck.

## Lese- und Ausführungsregeln

Befehle einzeln und in der angegebenen Reihenfolge ausführen. Bei Fehlern stoppen. In Codeblöcken stehen keine Shell-Prompts; Ausgabe-Blöcke sind nicht auszuführen. &lt;…&gt; immer VOR dem Kopieren ersetzen: Winkelklammern können von der Shell als Umleitung verstanden werden. Keine typografischen Anführungszeichen in Befehle übernehmen.

Neue Dateien sind jeweils vollständig gezeigt. Vor Bearbeitung bestehender Konfigurationen wird ein datiertes Backup angelegt. Ohne angegebene Spezialprüfung gilt: Inhalt kontrollieren, Dienstprotokoll prüfen und Funktion testen; es gibt keinen universellen Syntaxprüfer.

Alle Linux-Systempakete stammen aus trixie, trixie-updates oder trixie-security. --no-install-recommends lässt empfohlene Zusatzpakete weg; ausdrücklich benötigte Helfer werden trotzdem installiert. Containerimages kommen begründet aus den offiziellen Projekten bzw. Docker Official Images. Keine Fremd-APT-Repositories und kein Docker-Daemon.

## Platzhalterverzeichnis

| Platzhalter | Bedeutung / Beispiel |
| --- | --- |
| &lt;ISO-PFAD&gt; | Windows-Pfad zur Debian-13-amd64-netinst-ISO. |
| &lt;SHA512SUMS-PFAD&gt; | Windows-Pfad zur passenden Prüfsummendatei. |
| &lt;IP-ADRESSE&gt; | Reservierte IPv4 des NAS, z. B. 192.168.178.20. |
| &lt;NETZWERK&gt; | Heimnetz in CIDR-Schreibweise, z. B. 192.168.178.0/24; /24 beschreibt die Netzmaske. |
| &lt;ADMIN-IP&gt; | IPv4 des eigenen Administrations-PCs. |
| &lt;DOMAIN&gt; | Öffentlicher WordPress-Hostname, z. B. blog.example.net. Kein Schema und kein Pfad. |
| &lt;DATENPLATTE&gt; | Ganze Datenplatte, bevorzugt /dev/disk/by-id/…; niemals Systemplatte. |
| &lt;DATENPARTITION&gt; | Bestehende Dateisystempartition oder neue LVM-Partition, z. B. /dev/disk/by-id/…-part1. |
| &lt;UUID&gt; | Eindeutige Kennung des Dateisystems, aus blkid; nicht die LVM-PV-UUID. |
| &lt;DATEISYSTEM&gt; | Erkannter Typ, z. B. ext4 oder xfs. |
| &lt;DATENPFAD&gt; | Einhängepunkt der Datenplatte, empfohlen /srv/daten. Keine Leerzeichen. |
| &lt;BACKUPPFAD&gt; | Einhängepunkt einer SEPARATEN Backupplatte, empfohlen /srv/backup. Keine Leerzeichen. |
| &lt;BACKUP-UUID&gt; | Dateisystem-UUID dieser Backupplatte. |
| &lt;BACKUP-STAND&gt; | Vollständiger datierter Sicherungsordner unter &lt;BACKUPPFAD&gt;/darknas bzw. darknas-podman. |
| &lt;SAMBA-USER&gt; | Kleingeschriebener Linux-/Samba-Nutzer, ohne Leerzeichen; z. B. anna. |
| &lt;REGELNUMMER&gt; / &lt;SPERR-IP&gt; | UFW-Regelnummer bzw. konkrete IP für einen Sperrtest. |
| &lt;VOLUME&gt; | Existierender benannter Podman-Datenspeicher aus podman volume ls. |
| &lt;SUBID-START&gt; / &lt;SUBID-ENDE&gt; | Freier UID-/GID-Bereich, Ende = Start + 65535; nur bei fehlender Standardzuweisung. |
| &lt;PORTGRENZE-ALT&gt; | Vorheriger Kernelwert für niedrige Ports, üblicherweise 1024. |
| &lt;TESTDATEI&gt; / &lt;TESTDATEINAME&gt; | Relativer Pfad einer kleinen Restore-Testdatei und deren reiner Dateiname. |
| &lt;Freigabe&gt; | Name der gewünschten Samba-Freigabe, z. B. Gemeinsam. |
| &lt;USB-Gerät&gt; | Im Firmware-Bootmenü angezeigter USB-Stick; kein Linux-Gerätename. |
| &lt;NPM-IMAGE&gt; | docker.io/jc21/nginx-proxy-manager:&lt;freigegebener-Tag&gt; oder Repository@sha256:… |
| &lt;WP-IMAGE&gt; | docker.io/library/wordpress:&lt;freigegebener-Tag&gt;-apache oder Repository@sha256:… |
| &lt;DB-IMAGE&gt; | docker.io/library/mariadb:&lt;freigegebener-11.4-Tag&gt; oder Repository@sha256:… |

Feste zusätzliche Namen: Gruppe nasusers; LVM-Gruppe vg_darknas; Logical Volume lv_data; Container darknas-npm, darknas-wordpress, darknas-db; Quadlet-Netze darknas-proxy und darknas-dbnet. Standardports am NAS: SSH 22/tcp, Samba 445/tcp, NPM HTTP 8080/tcp, HTTPS 8443/tcp, Verwaltung ausschließlich 127.0.0.1:8181. Datenbank und WordPress veröffentlichen keine eigenen Hostports.

## Reihenfolge und Abhängigkeiten

01_02 Grundinstallation Debian einschließlich sudo-Prüfung → 03_01 System – Update. Danach Datenplatten über 03_04 Datenträger – Temporär einbinden und 03_05 Datenträger – Dauerhaft einbinden (LVM) vorbereiten, BEVOR Samba-Freigaben angelegt werden. 02_02 UFW – Installation vor dem Freigeben von Diensten. 02_05 Podman – Installation vor NPM und WordPress. Vor Internetfreigabe: beide Containerkapitel einschließlich Prüfungen und 03_12 Backup – Podman abschließen. Chrony, Fail2ban und alle Dienste sind optional; sudo ist für diese Anleitung verpflichtend.

## Ordnerstruktur und spätere Updates

- Die Kapitelordner heißen `01_Grundinstallation`, `02_Erweiterte_Installation` und `03_Administration`.
- Jeder fachliche Unterpunkt liegt in einem eigenen Ordner `KK_UU_Stichwort/`. Seine Ausarbeitung heißt immer `README.md`.
- Jeder Kapitelordner enthält zusätzlich eine `README.md` als nicht verlinkte Kapitelübersicht.
- Die vier bestehenden Dateien `00_00_Inhalt.md`, `00_01_Systemparameter.md`, `00_02_Quellen.md` und `00_03_Aenderungen.md` bleiben im Stammverzeichnis. Die Stammdatei `README.md` ist der Einstieg für das GitHub-Repository.
- In Unterpunktordnern können später Skripte, Konfigurationsbeispiele oder Bilder neben der `README.md` ergänzt werden. In dieser Fassung sind keine zusätzlichen Hilfsdateien enthalten.
- Interne Dokumentverknüpfungen und Navigationsleisten sind entfernt. Fachliche Querverweise stehen als Stichwort und ID im Text. Die Repository-Ordnernavigation übernimmt den Wechsel zwischen Ausarbeitungen.
- Quellenkennungen wie `[Q01]` sind reine Textverweise; direkt in derselben Ausarbeitung stehen die vollständigen Quellen-URLs. `00_02_Quellen.md` enthält zusätzlich die vollständige Gesamtliste.
- Die sechs Pflichtabschnitte einer fachlichen Ausarbeitung bleiben erhalten. Die Quellenliste steht nach dem sechsten Abschnitt als Unterüberschrift.
- Ab Version 1.3 IDs beibehalten. Neue Unterpunkte erhalten die nächste freie Nummer ihres Kapitels; bei späteren Löschungen die Nummer nicht neu verwenden.
- Bei Änderungen die Version und das Datum im Kommentar der betroffenen `README.md` sowie den Änderungsnachweis im Stamm aktualisieren. Neue Unterpunkte zusätzlich in der Kapitelübersicht und `00_00_Inhalt.md` als Text aufführen.
- Gemeinsame Parameter ausschließlich in dieser Datei pflegen und betroffene Anleitungen auf Auswirkungen prüfen.
- Bestehende Quellenkennungen nicht verschieben; neue Quellen mit der nächsten freien Kennung ergänzen.

## Quellen

- [Q01] Debian 13 Installationshandbuch: `https://www.debian.org/releases/trixie/amd64/`
- [Q02] sudo – Debian Installationsverhalten: `https://wiki.debian.org/sudo`
- [Q08] Debian Installer: Komponenten: `https://www.debian.org/releases/trixie/amd64/ch06s03.de.html`
- [Q26] podman(1), Trixie: `https://manpages.debian.org/trixie/podman/podman.1.en.html`
