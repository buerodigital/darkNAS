# 01_01 USB-Stick erstellen

<!-- id: 01_01; version: 1.3; updated: 2026-10-26; status: Arbeitsfassung -->

## 1. Ziel
balenaEtcher schreibt das Debian-Installationsabbild auf einen USB-Stick und kontrolliert den Schreibvorgang. Danach kann ein amd64-PC im UEFI-Modus davon starten; VirtualBox verwendet dieselbe ISO direkt als virtuelles DVD-Laufwerk. [Q13]

## 2. Voraussetzungen
- [ ] Windows-PC mit Internet, aktuellem balenaEtcher und Schreibrechten auf einen USB-Stick ab 2 GB.
- [ ] Debian-13-amd64-netinst-ISO sowie SHA512SUMS aus demselben offiziellen Downloadverzeichnis. Keine Live-/Desktop-ISO. [Q06]

## 3. Voraussetzungen
- [ ] ⚠️ Inhalt des USB-Sticks gesichert; alle anderen externen Datenträger möglichst abgezogen.
- [ ] ISO und Prüfsummendatei stammen vom Debian-Projekt. Ein Hash prüft die Unversehrtheit; die signierte Prüfsummenliste bestätigt bei geprüfter Signatur zusätzlich den Herausgeber. [Q19]

## 4. Umsetzung

1. Die unter „Quellen“ direkt angegebene Debian-Downloadseite öffnen und ausdrücklich Debian 13 „Trixie“, amd64, netinst auswählen. ISO und SHA512SUMS lokal speichern. Eine spätere Stable-Version nicht ungeprüft verwenden.

2. SHA-512 in Windows PowerShell vergleichen. Beide Pfade ersetzen; dieses Windows-Kommando benötigt kein sudo.

```powershell
$iso = Get-Item -LiteralPath '<ISO-PFAD>'
$sum = Get-FileHash -LiteralPath $iso.FullName -Algorithm SHA512
$line = Get-Content -LiteralPath '<SHA512SUMS-PFAD>' |
  Where-Object { ($_ -split '\s+')[-1].TrimStart('*') -eq $iso.Name }
if (@($line).Count -ne 1) { throw 'Passender Prüfsummeneintrag fehlt' }
$expected = ($line -split '\s+')[0]
if ($sum.Hash -ine $expected) { throw 'SHA512 stimmt NICHT überein' }
'OK: SHA512 stimmt überein'
```

3. ⚠️ Etcher öffnen: „Flash from file“ → geprüfte ISO; „Select target“ → USB-Stick anhand Größe und Modell; „Flash!“. Windows-Administratorabfrage bestätigen und Validierung abwarten. Etcher überschreibt den Stick. [Q13]

4. Stick sicher auswerfen. Fordert Windows zum Formatieren auf, ABBRECHEN: Linux-/ISO-Partitionen sind nicht zwangsläufig unter Windows lesbar. Für VirtualBox den Stick nicht benötigen; dort die geprüfte ISO einlegen.

## 5. Prüfung
PowerShell meldet:

```text
OK: SHA512 stimmt überein
```

Etcher meldet einen erfolgreich abgeschlossenen Flash-/Validierungsvorgang. Der anschließende UEFI-Boot zeigt das Debian-Installationsmenü; die eigentliche Installation folgt in 01_02 Grundinstallation Debian.

## 6. Fallstricke & Fehlersuche
- Hash abweichend → falsche Version oder beschädigter Download → ISO und passende Prüfsummen erneut laden; nicht flashen.
- Stick nicht auswählbar → Gerät gesperrt oder defekt → andere USB-Buchse bzw. anderen Stick verwenden; Zielmodell erneut prüfen.
- Windows will formatieren → Installationsabbild ist kein normales Windows-Laufwerk → abbrechen.
- Hash korrekt, Herkunft unklar → Hash allein bestätigt keinen Herausgeber → ausschließlich offizielle Quellen verwenden; für erhöhte Sicherheit Debian-Signaturprüfung anhand der direkt unter „Quellen“ angegebenen Anleitung durchführen. [Q19]

### Quellen

- [Q06] Debian Download: `https://www.debian.org/download.de.html`
- [Q13] balenaEtcher: `https://etcher.balena.io/`
- [Q19] Debian ISO-Verifikation: `https://www.debian.org/CD/verify`
