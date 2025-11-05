# KrampusAugen

Animierte Krampus-Augen auf Basis eines ESP32 und runden 240×240 px GC9A01-TFT-Displays.  
Das Projekt kombiniert die Eye-Animationen aus dem „ESP32 LCD Round Eyes“-Projekt von The Last Outpost mit eigenen Anpassungen für Krampus-Kostüme, Masken oder Requisiten.

## Inhalt
- [Highlights](#highlights)
- [Hardwarevoraussetzungen](#hardwarevoraussetzungen)
- [Software & Tools](#software--tools)
- [Aufbau & Verdrahtung](#aufbau--verdrahtung)
- [Firmware flashen](#firmware-flashen)
- [Konfiguration anpassen](#konfiguration-anpassen)
- [Projektstruktur](#projektstruktur)
- [Fehlersuche](#fehlersuche)
- [Weiterführende Ressourcen](#weiterführende-ressourcen)
- [Lizenz & Danksagung](#lizenz--danksagung)

## Highlights
- Krampus-taugliche horizontale Pupille (`goatEye`) bereits vorbereitet.
- Unterstützt ein oder zwei Displays, autonomes Blinzeln und Pupillenbewegung.
- Erweiterbar um eigene Animationen, Sensoren und NeoPixel-Effekte.
- Ausführliches Handbuch (PDF) für den schnellen Einstieg.

## Hardwarevoraussetzungen
- ESP32-Devkit (z. B. ESP32 WROOM/WROVER).
- 1 × oder 2 × 1,28" 240×240 px GC9A01 TFT-Display.
- USB-Kabel zum Flashen.
- (Optional) Fotowiderstand oder Potentiometer für Pupillensteuerung.
- (Optional) Taster für manuelles Blinzeln/Winken.
- (Optional) NeoPixel-Streifen oder Ring (siehe `user_xmas.cpp`).

## Software & Tools
- Arduino IDE ≥ 2.1 **oder** PlatformIO.
- ESP32 Board-Paket (z. B. „esp32 by Espressif Systems“ ab 2.0).
- Bibliotheken (über Bibliotheksmanager installierbar):
  - `TFT_eSPI`
  - `Adafruit_GFX` (indirekte Abhängigkeit)
  - `Adafruit_NeoPixel` (nur für optionale LEDs)
- PDF-Viewer für das Handbuch `manuals/Animierte_Augen_mit_ESP32_und_rundem_1_28_TFT-Display.pdf`.

## Aufbau & Verdrahtung
1. `3V3` des ESP32 an `VCC` des Displays, `GND` zu `GND`.
2. SPI-Leitungen: `MOSI`, `SCK`, `DC`, `CS`, `RST` gemäß Konfiguration in `TFT_eSPI`.
3. Hintergrundbeleuchtung des Displays (falls vorhanden) auf `3V3` oder einen PWM-Pin.
4. Optional: Sensoren/Taster laut `config.h` verbinden (Pins anpassen).
5. Für zwei Displays verkabelst du beide parallel an MOSI/SCK/DC, erhältst aber eigene `CS`-Leitungen.

> Tipp: Trage die Pins im `User_Setup_Select.h` der Bibliothek `TFT_eSPI` ein oder nutze eine projektlokale Kopie des Setups.

## Firmware flashen
1. Projektordner `ESP32LCDRound240x240Eyes-main` in der IDE öffnen.
2. Ziel-Board `ESP32 Dev Module` wählen und die passende COM-Schnittstelle setzen.
3. In der Datei `ESP32LCDRound240x240Eyes.ino` den Sketch kompilieren und auf den ESP32 laden.
4. Nach dem ersten Flash ggf. seriellen Monitor (115200 bps) öffnen, um Debug-Ausgaben zu sehen.

## Konfiguration anpassen
Alle wichtigen Einstellungen findest du in `ESP32LCDRound240x240Eyes-main/config.h`.

- **Grafik auswählen:** Standardmäßig ist `defaultEye` aktiv. Für die Krampus-Variante `goatEye` einkommentieren:
  ```cpp
  // #include "data/defaultEye.h"
  #include "data/goatEye.h"

- Displayanzahl: TFT_COUNT und NUM_EYES für Einzel- oder Doppel-Setup setzen.
- Rotation & Position: TFT_1_ROT, TFT_2_ROT, sowie EYE_1_XPOSITION / EYE_2_XPOSITION für Gehäuse anpassen.
- Blinzel- und Pupillenverhalten: AUTOBLINK, TRACKING, IRIS_MIN/MAX nach Geschmack verändern.
- Benutzerlogik: Eigenen Code in genau einer der Dateien user*.cpp aktivieren (Schalter am Kopf der Datei von #if 0 auf #if 1 setzen). Hier können Sensoren, NeoPixel oder animierte Spezialeffekte eingebunden werden.

## Projektstruktur

```text
KrampusAugen/
|- README.md                     # Dieses Dokument
|- main/                         # Ablage für eigene Experimente/Build-Artefakte
|- manuals/
|  |- Animierte_Augen_mit_ESP32_und_rundem_1_28_TFT-Display.pdf
- ESP32LCDRound240x240Eyes-main/ # Original- und angepasste Firmware
   |- README.md                  # Hinweise zum ursprünglichen Projekt
   |- LICENSE
   |- config.h                   # Zentrale Konfiguration (Pins, Features, Augen)
   |- ESP32LCDRound240x240Eyes.ino
   |- eye_functions.ino          # Rendering-Logik
   |- user.cpp                   # Einstiegspunkt für eigene Erweiterungen
   |- user_bat.cpp               # Beispiel: Fledermaus-Logik
   |- user_xmas.cpp              # Beispiel: NeoPixel-Weihnachtsmodus
   |- data/                      # Augen-Assets (*.h mit Farbtabellen)
   |  |- catEye.h
   |  |- goatEye.h
   |  - ...
   - images/
      - IMG-3903.JPG            # Referenzfoto der Hardware
```

## Fehlersuche
- Schwarzer Bildschirm: Verkabelung, TFT_eSPI-Setup und CS-Pin prüfen; Display ggf. mit 5 V Backlight speisen.
- Vertauschte Augen/Ausrichtung: TFT_*_ROT und Offsets in config.h justieren.
- Ruckelige Bewegung: Serielle Ausgabe deaktivieren oder Sensorwerte glätten (IRIS_SMOOTH).
- NeoPixel reagieren nicht: Stromversorgung und Datapin kontrollieren, Adafruit_NeoPixel-Bibliothek aktivieren.

## Weiterführende Ressourcen
- Originalprojekt von The Last Outpost Workshop: https://github.com/thelastoutpostworkshop/ESP32LCDRound240x240Eyes
- Anleitungsvideo: https://youtu.be/pmCc7z_Mi8I
- Handbuch im Ordner manuals/ für deutschsprachige Schritt-für-Schritt-Anweisungen.

## Lizenz & Danksagung
Basierend auf der Arbeit von The Last Outpost Workshop (siehe ESP32LCDRound240x240Eyes-main/LICENSE).  
Eigene Anpassungen und Dokumentation © 2025 KrampusAugen-Team.  
Vielen Dank an die Community für Feedback, Erweiterungen und kreative Einsätze!