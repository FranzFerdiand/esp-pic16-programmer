[🇩🇪 Deutsch](README.md) | [🇬🇧 English](README.en.md)

# ESP32 / ESP8266 PIC16(L)F193X ICSP Programmer

An ultra-fast, affordable ICSP programmer for Microchip PIC16(L)F193X microcontrollers (`PIC16LF1938`, `PIC16F1936`, `PIC16F1937`, etc.) powered by a standard **ESP32** or **ESP8266** (NodeMCU / D1 Mini).

No expensive dedicated hardware (like a PICkit) or MPLAB X required!

---

## 🌟 Features & Highlights

* **Full Microchip DS41397B Protocol Implementation:**
  * 32-word row writes (`WROW32`) for high-speed programming (< 20 seconds for full 8K words).
  * Direct configuration programming (`WCONFIG`) for CONFIG1 & CONFIG2 words.
  * Native 3.3V LVP (Low-Voltage Programming) bit-banging.
* **9V Battery Trick:** Bypass factory code protection (`LVP = OFF`) on fresh chips without high-voltage programmer equipment.
* **Precompiled Binaries:** `.bin` files ready in `bin/` for both ESP32 and ESP8266 – flashable right in your browser via WebSerial!
* **High-Speed Python CLI Client (`flash_pic.py`):** Parses Intel-HEX, buffers row writes, and automatically verifies targets.

---

## 🔌 Pinout & Wiring

| PIC16(L)F193X Pin | Signal | ESP32 Pin | ESP8266 Pin (NodeMCU / D1 Mini) | Note |
| :--- | :--- | :--- | :--- | :--- |
| **MCLR / VPP** | Reset / Program Mode Entry | **GPIO 18** | **D5** (GPIO 14) | Enters programming mode |
| **VDD** | Power Supply | **3V3** | **3V3** (3.3V) | **CAUTION: Never connect to 5V!** |
| **VSS** | Ground | **GND** | **GND** | Common Ground |
| **ICSPDAT (PGD)** | ICSP Data | **GPIO 19** | **D6** (GPIO 12) | Bidirectional ICSP data |
| **ICSPCLK (PGC)** | ICSP Clock | **GPIO 21** | **D7** (GPIO 13) | ICSP Clock |

---

## ⚡ The 9V Battery Trick (Initial Flash on `LVP = OFF` Chips)

Fresh factory chips often ship with `LVP = OFF`. 3.3V LVP entry cannot override this initial state without High-Voltage Vpp.
1. Connect a 9V battery negative terminal (-) to **GND**.
2. Touch the positive terminal (+9V) via a 1 kΩ – 10 kΩ resistor to **MCLR** when starting the erase cycle.
3. Once new firmware with `LVP = ON` is flashed, the 9V battery is never needed again!

---

## 🚀 Quickstart

### 1. Program the ESP
* **Without Arduino IDE:** Open [Espressif Web Flasher](https://espressif.github.io/esptool-js/), connect ESP and flash:
  * ESP8266: [`bin/esp8266_programmer.bin`](bin/esp8266_programmer.bin) at offset `0x0000`
  * ESP32: [`bin/esp32_programmer.bin`](bin/esp32_programmer.bin) at offset `0x10000`
* **Using Arduino IDE:** Open `esp32_programmer/esp32_programmer.ino` or `esp8266_programmer/esp8266_programmer.ino` and upload.

### 2. Flash Your PIC
```bash
python flash_pic.py COM3 my_firmware.hex
```

### 3. Check Diagnostics (Optional)
```bash
python check_diag.py COM3
```

---

## 📄 License
MIT License
