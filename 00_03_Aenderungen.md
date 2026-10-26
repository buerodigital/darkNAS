# 00_03 Änderungsnachweis

## Version 1.3 · 26.10.2026

- Chrony in das bisherige Installationskapitel 3 aufgenommen; dieses Kapitel ist jetzt Kapitel 2.
- Administration von Kapitel 4 zu Kapitel 3 umnummeriert. Grundinstallation bleibt Kapitel 1.
- Jeder der 22 fachlichen Unterpunkte besitzt jetzt einen eigenen nummerierten Ordner mit `README.md`. Kapitelübersichten heißen ebenfalls `README.md`.
- Vier bestehende `00_*.md` bleiben im Stamm; eine zusätzliche Stamm-`README.md` ist der Einstieg für GitHub.
- Alle internen Dokumentlinks, Navigationsleisten und Quellenanker entfernt. Fachliche Querverweise bleiben als Text mit aktualisierten IDs erhalten.
- Quellenkennungen sind reine Textverweise; vollständige Quellen-URLs stehen direkt in jeder Ausarbeitung und zusätzlich in der Gesamtliste.
- Codeblöcke, feste Systemwerte und technische Vorgehensweise unverändert übernommen. Es wurde kein Repository erstellt und nichts auf GitHub veröffentlicht.

### Neue Nummerierung

| Vorher (v1.2) | Jetzt (v1.3) |
| --- | --- |
| `01_01` / `01_02` Grundinstallation | unverändert |
| `02_01` Chrony | `02_01` im neuen Kapitel Erweiterte Installation |
| `03_01` bis `03_07` Installationsdienste | `02_02` bis `02_08` |
| `04_01` bis `04_12` Administration | `03_01` bis `03_12` |

Die ursprünglichen Dateinamen erscheinen nur noch in diesem historischen Änderungsnachweis. Für spätere Ergänzungen ab Version 1.3 die bestehenden IDs/Ordnernamen beibehalten.

### Prüfung der Version 1.3

22 fachliche Ausarbeitungen mit insgesamt 132 Pflichtabschnitten, drei Kapitelübersichten und die Stammdateien geprüft. Alle 96 Bash-Blöcke bleiben unverändert und wurden erneut syntaxgeprüft. Jede verwendete Quellenkennung besitzt eine direkte URL in der betreffenden Ausarbeitung. Markdown-Parsing und ZIP-Integrität wurden geprüft; ein vollständiger NAS-Praxistest bleibt ausstehend.

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

