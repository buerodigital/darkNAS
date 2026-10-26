# 00_01 Systemparameter und Leseregeln

<!-- id: 00_01; version: 1.2; updated: 2026-10-26 -->

Diese Anleitung führt von einem Windows-PC bis zum privaten NAS mit Samba und optionalen Webcontainern. Jedes Stichwort liegt als eigene Markdown-Datei vor. Befehle und Konfigurationen stehen in kopierbaren Codeblöcken; gemeinsame Parameter und Quellen werden zentral gepflegt. Die beschriebenen Abläufe wurden anhand der Dokumentation geprüft, aber nicht vollständig auf einem realen DarkNAS ausgeführt. Erwartete Ausgaben sind Beispiele und können je nach Paketstand abweichen.

## Verbindliche Entscheidungen

- Hostname: darknas. Einziges Administrationskonto: darkroot, Home: /home/darkroot.
- Im Installer bleibt das root-Passwort leer. Debian sperrt den direkten root-Zugang und richtet sudo für darkroot ein. Die Einrichtung und Prüfung stehen vollständig in [01_02 Grundinstallation Debian](01_Grundinstallation/01_02_Grundinstallation_Debian.md); ein separates Kapitel zur sudo-Installation entfällt. [Q02](00_02_Quellen.md#q02) [Q08](00_02_Quellen.md#q08)
- Hostadministration erfolgt als darkroot mit sudo, ohne root-Login und ohne dauerhafte root-Shell. Bestätigte Ausnahme: rootless Podman, systemctl --user und Arbeiten an eigenen Containerdateien laufen als darkroot OHNE sudo. Sonst würden andere, rootful Container und Volumes angesprochen. PowerShell und Installer-Menüs benötigen kein Linux-sudo. [Q26](00_02_Quellen.md#q26)
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

[01_02 Grundinstallation Debian](01_Grundinstallation/01_02_Grundinstallation_Debian.md) einschließlich sudo-Prüfung → [04_01 System – Update](04_Administration/04_01_System_Update.md). Danach Datenplatten über [04_04 Datenträger – Temporär einbinden](04_Administration/04_04_Datentraeger_temporaer_einbinden.md) und [04_05 Datenträger – Dauerhaft einbinden (LVM)](04_Administration/04_05_Datentraeger_dauerhaft_einbinden_LVM.md) vorbereiten, BEVOR Samba-Freigaben angelegt werden. [03_01 UFW – Installation](03_Erweiterte_Installation_Dienste/03_01_UFW_Installation.md) vor dem Freigeben von Diensten. [03_04 Podman – Installation](03_Erweiterte_Installation_Dienste/03_04_Podman_Installation.md) vor NPM und WordPress. Vor Internetfreigabe: beide Containerkapitel einschließlich Prüfungen und [04_12 Backup – Podman](04_Administration/04_12_Backup_Podman.md) abschließen. Chrony, Fail2ban und alle Dienste sind optional; sudo ist für diese Anleitung verpflichtend.

## Dateibenennung und spätere Updates

- `KK_UU_Stichwort.md`: `KK` = Kapitelnummer, `UU` = Unterpunktnummer, jeweils zweistellig. Die Datei liegt im Unterordner des zugehörigen Kapitels.
- `00_00` ist der zentrale Index; `00_01` enthält gemeinsame Systemparameter, `00_02` Quellen, `00_03` Änderungen. Alle vier Dateien mit Präfix `00_` bleiben im Stammverzeichnis.
- `01_00` bis `04_00` sind Kapitelübersichten innerhalb des jeweiligen Kapitelordners; `UU >= 01` bezeichnet ein eigenständig pflegbares Stichwort. Ordner: `01_Grundinstallation`, `02_Erweiterte_Installation_Zeit`, `03_Erweiterte_Installation_Dienste`, `04_Administration`.
- Die sechs Pflichtabschnitte bleiben innerhalb jeder Stichwortdatei unverändert; sie erzeugen keine weiteren Dateien.
- IDs bei späteren Updates beibehalten. Neue Stichwörter erhalten die nächste freie Nummer; für einen entfernten Unterpunkt wird die Nummer später nicht erneut vergeben.
- Bei inhaltlichen Änderungen Version/Datum im Kommentar der betroffenen Datei und `00_03_Aenderungen.md` im Stamm aktualisieren. Neue Dateien im passenden Kapitelordner anlegen und zusätzlich im zentralen Index sowie der Kapitelübersicht verlinken. Links aus Kapiteldateien zu Stammdateien beginnen mit `../`; Links zu anderen Kapiteln mit `../Kapitelordner/`.
- Gemeinsame Parameter ausschließlich hier ändern und anschließend alle Befehle/Querverweise auf Auswirkungen prüfen.
- Bestehende Quellenkennungen `Q01`, `Q02` usw. nicht neu nummerieren; neue Quellen im zentralen Quellenverzeichnis mit der nächsten freien Kennung ergänzen.

[Zur Inhaltsübersicht](00_00_Inhalt.md)
