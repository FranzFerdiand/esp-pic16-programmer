[🇩🇪 Deutsch](README.md) | [🇬🇧 English](README.en.md)

# ESP32 / ESP8266 PIC16(L)F193X ICSP Programmer

Ein ultra-schneller, kostengünstiger ICSP-Programmer für Microchip PIC16(L)F193X Mikrocontroller (z. B. `PIC16LF1938`, `PIC16F1936`, `PIC16F1937`), basierend auf einem gewöhnlichen **ESP32** oder **ESP8266** (NodeMCU / D1 Mini).

Kein teures Spezialgerät (wie PICkit) oder MPLAB X erforderlich!

---

## 🌟 Features & Highlights

* **Vollständige Microchip DS41397B Protokoll-Implementierung:**
  * 32-Wort Zeilenprogrammierung (`WROW32`) für ultraschnelles Flashen (< 20 Sekunden für 8K Worte).
  * Gezielte Konfigurationsprogrammierung (`WCONFIG`) für CONFIG1 & CONFIG2 Fuses.
  * LVP (Low-Voltage Programming) direkt über 3,3V GPIOs.
* **9V-Batterie-Trick:** Entsperrt beim ersten Flashen werkseitig gesperrte Chips (`LVP = OFF`).
* **Vorkompilierte Binaries:** Vorkompilierte `.bin`-Dateien für ESP32 und ESP8266 in `bin/` bereitgestellt – flashbar direkt im Browser via Web-Flasher!
* **High-Speed Python CLI Client (`flash_pic.py`):** Parst Intel-HEX, sendet Zeilen im Block und verifiziert nach dem Schreiben.

---

## 🔌 Verkabelung & Pinout

| PIC16(L)F193X Pin | Signal | ESP32 Pin | ESP8266 Pin (NodeMCU / D1 Mini) | Hinweis |
| :--- | :--- | :--- | :--- | :--- |
| **MCLR / VPP** | Programmierfreigabe / Reset | **GPIO 18** | **D5** (GPIO 14) | Zum Starten des Programmiermodus |
| **VDD** | Versorgungsspannung | **3V3** | **3V3** (3.3V) | **ACHTUNG: Niemals 5V!** |
| **VSS** | Masse | **GND** | **GND** | Gemeinsame Masse |
| **ICSPDAT (PGD)** | Datenleitung | **GPIO 19** | **D6** (GPIO 12) | Bidirektionale ICSP-Daten |
| **ICSPCLK (PGC)** | Taktleitung | **GPIO 21** | **D7** (GPIO 13) | ICSP-Takt |

---

## ⚡ Der 9V-Batterie-Trick (Für gesperrte Chips mit `LVP = OFF`)

Werksneue Chips haben häufig die Fuse `LVP = OFF` gesetzt. Ein 3,3V-LVP-Signal reicht dann beim allerersten Mal nicht aus.
1. Minuspol einer 9V-Blockbatterie an **GND** anschließen.
2. Pluspol (+9V) über einen 1 kΩ – 10 kΩ Widerstand während des Erase-Starts an **MCLR** halten (oder Diode an GPIO 18/D5).
3. Sobald die neue Firmware mit `LVP = ON` geflasht ist, wird die 9V-Batterie nie wieder benötigt!

---

## 🚀 Schnellanleitung

### 1. ESP vorbereiten
* **Ohne Arduino IDE:** Öffne den [Espressif Web Flasher](https://espressif.github.io/esptool-js/), verbinde den ESP und flashe:
  * ESP8266: [`bin/esp8266_programmer.bin`](bin/esp8266_programmer.bin) an Offset `0x0000`
  * ESP32: [`bin/esp32_programmer.bin`](bin/esp32_programmer.bin) an Offset `0x10000`
* **Über Arduino IDE:** Öffne `esp32_programmer/esp32_programmer.ino` bzw. `esp8266_programmer/esp8266_programmer.ino` und lade den Sketch hoch.

### 2. PIC flashen
```bash
python flash_pic.py COM3 firmware.hex
```

### 3. Diagnose ausführen (optional)
```bash
python check_diag.py COM3
```

---

## 📄 Lizenz
MIT License
