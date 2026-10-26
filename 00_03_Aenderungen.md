# 00_03 Änderungsnachweis

## Version 1.2 · 26.10.2026

- Die vier Dateien mit Präfix `00_` verbleiben im Stammverzeichnis.
- Kapiteldateien einschließlich ihrer Übersichten in `01_Grundinstallation`, `02_Erweiterte_Installation_Zeit`, `03_Erweiterte_Installation_Dienste` und `04_Administration` eingeordnet.
- Sämtliche relativen Kapitel-, Index-, Parameter- und Quellenverknüpfungen an die neue Ordnerstruktur angepasst; Quellenanker und externe Links beibehalten.
- Hinweise zur Ordnerstruktur und künftigen Pflege aktualisiert. Dateinamen, Stichwort-IDs und technische Befehle bleiben unverändert.
- Neues ZIP-Paket `DarkNAS_Markdown_v1.2.zip` erstellt.

### Prüfung der Version 1.2

30 Markdown-Dateien und 22 Stichwortkapitel mit insgesamt 132 Pflichtabschnitten geprüft. Die 301 relativen Links und ihre Quellenanker sind gültig; vorhandene Codeblöcke bleiben unverändert. 96 Bash-Blöcke erneut mit `bash -n` nach neutraler Platzhalterersetzung geprüft. Die ZIP-Integrität und die Übereinstimmung mit den Einzelfassungen wurden kontrolliert. Ein vollständiger Praxistest auf NAS-Hardware steht weiterhin aus.

## Version 1.1 · 26.10.2026

- Word-Arbeitsfassung in ein modular pflegbares Markdown-Paket umgestellt.
- 22 Stichwortdateien mit stabiler Benennung `KK_UU_Stichwort.md`, vier Kapitelübersichten und zentralem Index erstellt.
- Eigenständigen Unterpunkt „Sudo – Installation und darkroot hinzufügen“ entfernt.
- `darkroot` bleibt der im Debian-Installer angelegte erste normale Benutzer. Leeres root-Passwort veranlasst die automatische sudo-Einrichtung; Prüfungen mit `id -nG`, `sudo -v`, `sudo -l` und `sudo visudo -c` sind in `01_02` integriert.
- Alle Verweise auf das entfernte sudo-Kapitel und die bisherige Ausführungsreihenfolge korrigiert.
- Gemeinsame Systemparameter, Platzhalter und Quellen in `00_01`/`00_02` ausgelagert; interne Verweise durch relative Markdown-Links ergänzt.
- Keine Änderung am gewählten rootless-Podman-Modell, Internetzugriff über NPM, Schutz vorhandener Daten oder Backupkonzept.

## Prüfung der Version 1.1

- 22 Stichwortdateien mit jeweils sechs Pflichtabschnitten geprüft: 132 Abschnittsüberschriften.
- 96 Bash-Codeblöcke nach neutraler Platzhalterersetzung mit `bash -n` syntaxgeprüft; keine Syntaxfehler.
- 301 relative Links sowie die Quellenanker geprüft; keine fehlenden Ziele.
- Alle 30 Dateien mit einem Markdown-Parser eingelesen; Codeinhalte bei der Aufteilung unverändert erhalten.
- Ein vollständiger Laufzeit-/Wiederherstellungstest auf realer NAS-Hardware bleibt ausstehend.

Die bisherige Wordfassung wird durch dieses Markdown-Paket als pflegbare Arbeitsfassung abgelöst. Versions-/Datumsangaben neuer Änderungen hier ergänzen; vorhandene Dateinummern nicht verschieben.

[Zur Inhaltsübersicht](00_00_Inhalt.md)
