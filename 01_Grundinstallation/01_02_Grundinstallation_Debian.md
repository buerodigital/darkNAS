# 01_02 Grundinstallation Debian

<!-- id: 01_02; version: 1.2; updated: 2026-10-26; status: Arbeitsfassung -->

## 1. Ziel
Debian ist das Betriebssystem des DarkNAS und stellt Paketverwaltung, Netzwerk und Dienstverwaltung bereit. Am Ende läuft Debian 13 ohne Desktop, mit Hostname darknas und Administrationskonto darkroot; die Datenplatte wurde nicht verändert. [Q01](../00_02_Quellen.md#q01)

## 2. Voraussetzungen
- [ ] [01_01 USB-Stick erstellen](01_01_USB_Stick_erstellen.md) abgeschlossen; Internetzugang möglichst per Ethernet und DHCP, also automatische Netzwerkkonfiguration.
- [ ] Systemplatte eindeutig identifiziert. Empfehlung für dieses Beispiel: mindestens 32 GB; für Container besser 64 GB oder mehr.
- [ ] Hardware: amd64 mit UEFI. VM: VirtualBox installiert, mindestens 2 vCPU und 4 GB RAM als praktische Ausgangsbasis für NAS plus Webcontainer, nicht als Debian-Mindestanforderung.

## 3. Voraussetzungen
- [ ] ⚠️ Vorhandene Datenplatte für die Installation physisch abgesteckt oder aus der VM entfernt; vorher vorhandene Daten gesichert.
- [ ] Lokaler Bildschirm/VM-Konsole verfügbar. SSH wird erst nach lokal geprüftem sudo verwendet.
- [ ] Ein starkes, eigenes Passwort für darkroot vorbereitet; root-Passwort bleibt ausdrücklich leer.

## 4. Umsetzung

1. Physischer PC: UEFI-Bootmenü öffnen, Eintrag „UEFI: &lt;USB-Gerät&gt;“ wählen. UEFI ist die moderne Firmware; GRUB wird später darin als Bootloader registriert. Secure Boot mit offiziellen Debian-Komponenten zunächst aktiviert lassen. [Q01](../00_02_Quellen.md#q01)

2. VM: neue Linux-/Debian-64-Bit-VM erstellen, unbeaufsichtigte Installation überspringen, EFI aktivieren, nur eine neue System-VDI anschließen und ISO als DVD einlegen. Netzwerk „Netzwerkbrücke/Bridged Adapter“ zum kabelgebundenen Hostadapter, „Kabel verbunden“ aktiv. Die VM erhält damit eine eigene Heimnetz-IP. NAT nur für reine Installationsversuche verwenden; LAN-Dienste benötigen dann zusätzliche Weiterleitungen. Guest Additions und Extension Pack sind für dieses textbasierte NAS nicht erforderlich. [Q22](../00_02_Quellen.md#q22) [Q23](../00_02_Quellen.md#q23)

3. Installer „Install“ starten. Sprache Deutsch, Land Deutschland, Tastatur Deutsch wählen. DHCP übernehmen; Rechnername darknas; Domainname im Heimnetz leer lassen.

4. Beide root-Passwortfelder leer lassen und fortfahren. Benutzer-Vollname frei wählen, Benutzername darkroot. Zweimal ein starkes Benutzerpasswort eingeben. Der Installer installiert sudo und nimmt diesen ersten Nutzer in die sudo-Gruppe auf. Kein Benutzer darknas wird angelegt. [Q02](../00_02_Quellen.md#q02) [Q08](../00_02_Quellen.md#q08)

5. ⚠️ Partitionierung: „Geführt – vollständige Festplatte verwenden“, nur die Systemplatte anhand Größe/Modell auswählen, „Alle Dateien auf eine Partition“. Kein System-LVM und keine Datenplatte wählen. Vorschau kontrollieren: EFI-Systempartition mit FAT, Root-Dateisystem und ggf. Swap auf derselben Systemplatte; erst dann Änderungen schreiben. EFI speichert den Bootloader, Swap ergänzt bei Bedarf Arbeitsspeicher. [Q08](../00_02_Quellen.md#q08)

6. Debian-Spiegel auswählen; keinen Proxy eintragen, falls nicht vorhanden. Bei Softwareauswahl alle Desktopumgebungen und „Debian desktop environment“ abwählen. „Standard-Systemwerkzeuge“ behalten; „SSH server“ hier abwählen und im nächsten Schritt kontrolliert installieren. Installation und GRUB abschließen, Medium entfernen und neu starten. [Q08](../00_02_Quellen.md#q08)

7. Lokal als darkroot anmelden und die bereits vom Installer eingerichteten sudo-Rechte prüfen. sudo ist jetzt vorhanden; weder eine nachträgliche Paketinstallation noch eine manuelle Gruppenaufnahme ist nötig. whoami ist eine reine Benutzerprüfung. [Q02](../00_02_Quellen.md#q02) [Q08](../00_02_Quellen.md#q08)

```bash
whoami
id -nG
sudo -v
sudo -l
sudo visudo -c
sudo hostnamectl
sudo dpkg --print-architecture
sudo cat /etc/os-release
sudo test -d /sys/firmware/efi && echo 'UEFI: OK'
```

Erwartet: `whoami` meldet `darkroot`, `id -nG` enthält `sudo`, `sudo -v` akzeptiert das Benutzerpasswort und `sudo visudo -c` meldet `parsed OK`. `sudo -l` zeigt die erlaubten administrativen Befehle. Scheitert diese Prüfung, noch keine weiteren Administrationsschritte ausführen. [Q02](../00_02_Quellen.md#q02) [Q08](../00_02_Quellen.md#q08)

8. Grundwerkzeuge, korrekte Zeit und Fernzugriff einrichten. Hostname ist bereits darknas; daher /etc/hostname und /etc/hosts hier nicht unnötig bearbeiten. Ein individueller PATH ist nicht erforderlich: sudo verwendet seinen eigenen sicheren Suchpfad.

```bash
sudo apt update
sudo apt install --no-install-recommends openssh-server ca-certificates nano
sudo timedatectl set-timezone Europe/Berlin
sudo install -d -m 0700 /var/backups/darknas
sudo cp -a /etc/ssh /var/backups/darknas/ssh.$(date +%F-%H%M%S)
sudo tee /etc/ssh/sshd_config.d/00-darknas.conf >/dev/null <<'CONF'
PermitRootLogin no
AllowUsers darkroot
PermitEmptyPasswords no
CONF
sudo /usr/sbin/sshd -t
sudo systemctl enable --now ssh.service
sudo systemctl reload ssh.service
sudo /usr/sbin/sshd -T | grep -E 'permitrootlogin|allowusers|permitemptypasswords'
```

⚠️ SSH-Konfiguration nur bei fehlerfreier Syntax übernehmen. Lokale Konsole offen lassen. Das Präfix 00- macht die Einstellung früh wirksam; sshd übernimmt grundsätzlich den zuerst erhaltenen Wert. Andere Drop-ins bei abweichender Ausgabe untersuchen. [Q20](../00_02_Quellen.md#q20)

9. IP und Netz ermitteln. Im Router eine DHCP-Reservierung für die MAC-Adresse des NAS/der VM setzen; sie hält &lt;IP-ADRESSE&gt; konstant, ohne Debian-Netzwerkdateien zu ändern.

```bash
sudo ip -br address
sudo ip route
sudo ip link
```

10. Vom Windows-PC eine zweite Verbindung testen. Fingerabdruck zuerst lokal anzeigen und mit der SSH-Abfrage vergleichen. Niemals eine ungeprüfte geänderte Hostidentität akzeptieren.

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

```powershell
ssh darkroot@<IP-ADRESSE>
```

Erst nach dem erfolgreichen Zweitzugriff die lokale Sitzung schließen. Routerfreigaben für SSH bleiben aus; [03_01 UFW – Installation](../03_Erweiterte_Installation_Dienste/03_01_UFW_Installation.md) schützt anschließend den Zugang im Heimnetz.

## 5. Prüfung

```bash
sudo hostnamectl --static
sudo dpkg --print-architecture
sudo id darkroot
sudo visudo -c
sudo passwd -S root
sudo systemctl is-active ssh.service
sudo timedatectl show -p Timezone
```

Erwartet: darknas; amd64; Gruppen enthalten sudo; sudoers-Syntax meldet parsed OK; root-Status enthält L (gesperrt); active; Timezone=Europe/Berlin. /etc/os-release nennt VERSION_ID="13" und VERSION_CODENAME=trixie. [Q08](../00_02_Quellen.md#q08)

## 6. Fallstricke & Fehlersuche
- sudo fehlt → root-Passwort wurde doch gesetzt oder anderes Installationsprofil verwendet → nicht mit den Folgeschritten improvisieren; auf dieser frischen Installation mit bestätigter Installer-Variante neu beginnen.
- UEFI-Prüfung scheitert → Legacy-Boot gewählt → vor weiterer Einrichtung im UEFI-Modus neu installieren; VM-EFI nicht nachträglich umschalten.
- VM im LAN unsichtbar → NAT oder falscher Bridge-Adapter → VM ausschalten und Adapter prüfen; Ethernet-Brücke bevorzugen. [Q22](../00_02_Quellen.md#q22)
- SSH „Connection refused“ → Dienst/Netzwerk prüfen → lokal sudo systemctl status ssh.service und sudo journalctl -u ssh.service -n 50 verwenden.
- ⚠️ SSH nach Änderung gesperrt → lokale Konsole nutzen, gesicherte Konfiguration zurückkopieren, sshd -t und reload ausführen. Keine Routerfreigabe als Problemlösung setzen.

---

[Inhaltsübersicht](../00_00_Inhalt.md) · [Systemparameter](../00_01_Systemparameter.md) · [Quellen](../00_02_Quellen.md) · [Kapitel 01](01_00_Grundinstallation.md)
