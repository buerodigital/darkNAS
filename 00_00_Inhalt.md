# 00_00 DarkNAS – Inhaltsübersicht

Debian 13 „Trixie“ · amd64/UEFI · Oracle VirtualBox · Arbeitsfassung 1.3 · 26.10.2026

Die Anleitung besteht aus 22 fachlichen Ausarbeitungen in eigenen Unterpunktordnern. Jede Ausarbeitung heißt `README.md`; Skripte oder weitere Materialien können später unmittelbar daneben liegen. Interne Dokumentverknüpfungen sind entfernt. Die Quellen-URLs stehen direkt in der betreffenden Ausarbeitung.

## Gemeinsame Dateien im Stamm

- `README.md`: Einstieg für GitHub.
- `00_00_Inhalt.md`: vollständige Inhaltsübersicht.
- `00_01_Systemparameter.md`: feste Systemwerte, Platzhalter und Leseregeln.
- `00_02_Quellen.md`: Gesamtliste der Quellen.
- `00_03_Aenderungen.md`: Änderungsnachweis.

## 01 Grundinstallation

Kapitelordner: `01_Grundinstallation/`. Seine `README.md` enthält die Kapitelübersicht.

- 01_01 USB-Stick erstellen
  Ausarbeitung: `01_Grundinstallation/01_01_USB_Stick_erstellen/README.md`
- 01_02 Grundinstallation Debian
  Ausarbeitung: `01_Grundinstallation/01_02_Grundinstallation_Debian/README.md`

## 02 Erweiterte Installation (optional)

Kapitelordner: `02_Erweiterte_Installation/`. Seine `README.md` enthält die Kapitelübersicht.

- 02_01 Chrony – Zeitsynchronisation
  Ausarbeitung: `02_Erweiterte_Installation/02_01_Chrony_Zeitsynchronisation/README.md`
- 02_02 UFW – Installation
  Ausarbeitung: `02_Erweiterte_Installation/02_02_UFW_Installation/README.md`
- 02_03 Fail2ban – Installation
  Ausarbeitung: `02_Erweiterte_Installation/02_03_Fail2ban_Installation/README.md`
- 02_04 Samba – Installation
  Ausarbeitung: `02_Erweiterte_Installation/02_04_Samba_Installation/README.md`
- 02_05 Podman – Installation
  Ausarbeitung: `02_Erweiterte_Installation/02_05_Podman_Installation/README.md`
- 02_06 Podman – Container „Network Proxy Manager“
  Ausarbeitung: `02_Erweiterte_Installation/02_06_Podman_Nginx_Proxy_Manager/README.md`
- 02_07 Podman – Container „WordPress“
  Ausarbeitung: `02_Erweiterte_Installation/02_07_Podman_WordPress/README.md`
- 02_08 Portfreigabe – Ports unter 1024 für normale User freigeben
  Ausarbeitung: `02_Erweiterte_Installation/02_08_Portfreigabe_unter_1024/README.md`

## 03 Administration

Kapitelordner: `03_Administration/`. Seine `README.md` enthält die Kapitelübersicht.

- 03_01 System – Update
  Ausarbeitung: `03_Administration/03_01_System_Update/README.md`
- 03_02 UFW – Administration
  Ausarbeitung: `03_Administration/03_02_UFW_Administration/README.md`
- 03_03 Fail2ban – Administration
  Ausarbeitung: `03_Administration/03_03_Fail2ban_Administration/README.md`
- 03_04 Datenträger – Temporär einbinden
  Ausarbeitung: `03_Administration/03_04_Datentraeger_temporaer_einbinden/README.md`
- 03_05 Datenträger – Dauerhaft einbinden (LVM)
  Ausarbeitung: `03_Administration/03_05_Datentraeger_dauerhaft_einbinden_LVM/README.md`
- 03_06 Samba – Userverwaltung
  Ausarbeitung: `03_Administration/03_06_Samba_Userverwaltung/README.md`
- 03_07 Samba – Globale Freigaben
  Ausarbeitung: `03_Administration/03_07_Samba_Globale_Freigaben/README.md`
- 03_08 Samba – Userfreigaben
  Ausarbeitung: `03_Administration/03_08_Samba_Userfreigaben/README.md`
- 03_09 Samba – Podman Freigabe (alle Podman Volumes, read only)
  Ausarbeitung: `03_Administration/03_09_Samba_Podman_Volumes_read_only/README.md`
- 03_10 Backup – Konfiguration
  Ausarbeitung: `03_Administration/03_10_Backup_Konfiguration/README.md`
- 03_11 Backup – Freigaben
  Ausarbeitung: `03_Administration/03_11_Backup_Freigaben/README.md`
- 03_12 Backup – Podman
  Ausarbeitung: `03_Administration/03_12_Backup_Podman/README.md`

## Empfohlene Ausführungsreihenfolge

1. `01_01` → `01_02` abschließen, einschließlich der bereits vom Installer eingerichteten sudo-Rechte.
2. `03_01 System – Update` ausführen; Datenplatten über `03_04` und `03_05` prüfen und einbinden.
3. Optionale Dienste nach den Voraussetzungen ihrer Ausarbeitung installieren. Chrony ist `02_01`; UFW ist `02_02`.
4. Samba-Freigaben erst nach dem Datenmount erstellen; Podman vor den Containerdiensten einrichten.
5. Backups und Wiederherstellungstests vor der Internetfreigabe durchführen; NPM-/WordPress-Ausarbeitungen enthalten die abgestimmte Veröffentlichungsreihenfolge.

Dateiformat: UTF-8 mit Unix-Zeilenenden (LF). Diese Ausarbeitung bleibt eine Arbeitsfassung; ein vollständiger End-to-End-Praxistest auf NAS-Hardware steht aus.
