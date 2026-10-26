# 00_00 DarkNAS – Inhaltsübersicht

Debian 13 „Trixie“ · amd64/UEFI · Oracle VirtualBox · Arbeitsfassung 1.2 · 26.10.2026

Die Anleitung besteht aus 22 eigenständig editierbaren Stichwortdateien. `darkroot` wird während der Debian-Installation als erster normaler Benutzer mit sudo-Rechten eingerichtet; das eigenständige sudo-Installationskapitel entfällt. Einzelheiten stehen in [01_02 Grundinstallation Debian](01_Grundinstallation/01_02_Grundinstallation_Debian.md).

## Ordnerstruktur

```text
DarkNAS_Markdown_v1.2/
├── 00_00_Inhalt.md
├── 00_01_Systemparameter.md
├── 00_02_Quellen.md
├── 00_03_Aenderungen.md
├── 01_Grundinstallation/
├── 02_Erweiterte_Installation_Zeit/
├── 03_Erweiterte_Installation_Dienste/
└── 04_Administration/
```

Die Dateinamen und IDs bleiben unverändert. Beispielsweise liegt `01_02_Grundinstallation_Debian.md` jetzt in `01_Grundinstallation/`.

## Gemeinsam gepflegte Dateien

- [00_01 Systemparameter und Leseregeln](00_01_Systemparameter.md)
- [00_02 Quellen](00_02_Quellen.md)
- [00_03 Änderungsnachweis](00_03_Aenderungen.md)

## 01 Grundinstallation

[Kapitelübersicht 01_00](01_Grundinstallation/01_00_Grundinstallation.md)

- [01_01 USB-Stick erstellen](01_Grundinstallation/01_01_USB_Stick_erstellen.md)
- [01_02 Grundinstallation Debian](01_Grundinstallation/01_02_Grundinstallation_Debian.md)

## 02 Erweiterte Installation: Zeitsynchronisation (optional)

[Kapitelübersicht 02_00](02_Erweiterte_Installation_Zeit/02_00_Erweiterte_Installation_Zeit.md)

- [02_01 Chrony – Zeitsynchronisation](02_Erweiterte_Installation_Zeit/02_01_Chrony_Zeitsynchronisation.md)

## 03 Erweiterte Installation: Dienste (optional)

[Kapitelübersicht 03_00](03_Erweiterte_Installation_Dienste/03_00_Erweiterte_Installation_Dienste.md)

- [03_01 UFW – Installation](03_Erweiterte_Installation_Dienste/03_01_UFW_Installation.md)
- [03_02 Fail2ban – Installation](03_Erweiterte_Installation_Dienste/03_02_Fail2ban_Installation.md)
- [03_03 Samba – Installation](03_Erweiterte_Installation_Dienste/03_03_Samba_Installation.md)
- [03_04 Podman – Installation](03_Erweiterte_Installation_Dienste/03_04_Podman_Installation.md)
- [03_05 Podman – Container „Network Proxy Manager“](03_Erweiterte_Installation_Dienste/03_05_Podman_Nginx_Proxy_Manager.md)
- [03_06 Podman – Container „WordPress“](03_Erweiterte_Installation_Dienste/03_06_Podman_WordPress.md)
- [03_07 Portfreigabe – Ports unter 1024 für normale User freigeben](03_Erweiterte_Installation_Dienste/03_07_Portfreigabe_unter_1024.md)

## 04 Administration

[Kapitelübersicht 04_00](04_Administration/04_00_Administration.md)

- [04_01 System – Update](04_Administration/04_01_System_Update.md)
- [04_02 UFW – Administration](04_Administration/04_02_UFW_Administration.md)
- [04_03 Fail2ban – Administration](04_Administration/04_03_Fail2ban_Administration.md)
- [04_04 Datenträger – Temporär einbinden](04_Administration/04_04_Datentraeger_temporaer_einbinden.md)
- [04_05 Datenträger – Dauerhaft einbinden (LVM)](04_Administration/04_05_Datentraeger_dauerhaft_einbinden_LVM.md)
- [04_06 Samba – Userverwaltung](04_Administration/04_06_Samba_Userverwaltung.md)
- [04_07 Samba – Globale Freigaben](04_Administration/04_07_Samba_Globale_Freigaben.md)
- [04_08 Samba – Userfreigaben](04_Administration/04_08_Samba_Userfreigaben.md)
- [04_09 Samba – Podman Freigabe (alle Podman Volumes, read only)](04_Administration/04_09_Samba_Podman_Volumes_read_only.md)
- [04_10 Backup – Konfiguration](04_Administration/04_10_Backup_Konfiguration.md)
- [04_11 Backup – Freigaben](04_Administration/04_11_Backup_Freigaben.md)
- [04_12 Backup – Podman](04_Administration/04_12_Backup_Podman.md)

## Empfohlene Ausführungsreihenfolge

1. Grundinstallation `01_01` → `01_02`, einschließlich sudo-Prüfung.
2. [04_01 System – Update](04_Administration/04_01_System_Update.md); Datenplatten danach über `04_04` und `04_05` prüfen und einbinden.
3. Optionale Zeit- und Systemdienste nach den Voraussetzungen der jeweiligen Stichwörter installieren.
4. Samba-Freigaben erst nach dem Datenmount erstellen. Containerimages/Secrets/Volumes nach den Podman-Kapiteln anlegen.
5. Backups und Wiederherstellungstests vor Internetfreigabe durchführen; NPM-/WordPress-Schritte enthalten den abgestimmten Veröffentlichungsablauf.

## Hinweise zur Pflege

Das Dateiformat ist UTF-8 mit Unix-Zeilenenden (LF). Relative Links funktionieren, wenn die gesamte Ordnerstruktur gemeinsam entpackt und unverändert beibehalten wird. Die vier `00_`-Dateien liegen im Stamm; Kapiteldateien einschließlich ihrer Übersichten liegen in den entsprechenden Unterordnern. Quellen sind als portable Markdown-Links hinterlegt.

Die Fassung ist anhand der Dokumentation und mittels Syntaxprüfungen geprüft. Ein vollständiger End-to-End-Praxistest auf einem realen DarkNAS steht weiterhin aus.
