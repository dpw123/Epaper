# ESPHome ePaper Dashboard

A low-power smart information display powered by **ESPHome** and **Home Assistant**, running on an ESP32 and a Waveshare 4.2" e-Paper display (400×300).

---

## Features

- **Date & Wedding Countdown**:
  - Formatted as `ddd d mmm` (e.g. `Wed 7 Oct`).
  - Countdown to 31 August 2027 (`Wedding in X days!`).
- **Battery & EV Status**:
  - Home battery State of Charge (SOC %) with dynamic battery level icons.
  - Electric vehicle (Kona EV) remaining range in miles.
- **Budget Tracking**:
  - Monzo account balances (Spends, Groceries, Work) displayed as proportional progress bars against monthly limits.
  - Monthly progress indicator marker to compare spending pace against days elapsed.
- **Solar PV Production**:
  - 7-hour historical solar generation bar chart with dynamic Y-axis scaling.
- **Weather & Forecast**:
  - Current conditions: weather icon, daily min/max temperature, precipitation, and wind speed.
  - 4-slot forecast: day-of-week, weather icon, and temperature.

---

## Hardware & Wiring

- **Microcontroller**: ESP32 Dev Board (`esp32dev`)
- **Display**: Waveshare 4.20" e-Paper Module (v2, 400×300, SPI)
- **Framework**: `esp-idf`

### Pin Configuration

| Waveshare Pin | ESP32 GPIO | Description |
| :--- | :--- | :--- |
| **BUSY** | `GPIO25` | Busy signal |
| **RST** | `GPIO26` | Reset |
| **DC** | `GPIO27` | Data / Command control |
| **CS** | `GPIO15` | SPI Chip Select |
| **CLK** | `GPIO13` | SPI Clock |
| **DIN (MOSI)** | `GPIO14` | SPI Master-Out-Slave-In |
| **VCC** | `3.3V` | Power supply |
| **GND** | `GND` | Ground |

---

## Prerequisites & Setup

### 1. Requirements
- Python 3.11+
- [ESPHome](https://esphome.io/) (tested with 2024.12.2)
- PlatformIO (`platformio==6.1.16` recommended for ESPHome 2024.12)

### 2. Configure Secrets
Create a `secrets.yaml` file in the project root (this file is ignored by git for security):

```yaml
wifi_ssid: "Your_WiFi_SSID"
wifi_password: "Your_WiFi_Password"
ha_api: "Your_Home_Assistant_API_Encryption_Key"
ota_pw: "Your_OTA_Password"
```

### 3. Download Required Font
While Google Fonts (`Montserrat` and `Material Symbols Outlined`) are downloaded automatically by ESPHome at build time, the **Material Design Icons** font (`materialdesignicons-webfont.ttf`) is ignored by git and must be placed into the `fonts/` folder:

**PowerShell (Windows):**
```powershell
New-Item -ItemType Directory -Force -Path fonts
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/Templarian/MaterialDesign-Webfont/master/fonts/materialdesignicons-webfont.ttf" -OutFile "fonts/materialdesignicons-webfont.ttf"
```

**curl (macOS / Linux / Bash):**
```sh
mkdir -p fonts
curl -L -o fonts/materialdesignicons-webfont.ttf "https://raw.githubusercontent.com/Templarian/MaterialDesign-Webfont/master/fonts/materialdesignicons-webfont.ttf"
```

---

## Installation & Flashing

### Validate & Compile
To verify configuration and build the firmware without uploading:

```sh
esphome compile epaper.yaml
```

### Upload to Device
Connect the ESP32 to your computer via USB (or ensure it is reachable on the network for OTA updates):

```sh
esphome upload epaper.yaml
```

When prompted, select the serial port corresponding to your connected ESP32 (e.g., `COM3` on Windows or `/dev/ttyUSB0` on Linux/macOS), or choose the wireless OTA option if the device is already on your network.

### Compile, Upload & Monitor
To build, upload, and immediately open the serial console log stream in one step:

```sh
esphome run epaper.yaml
```

---

## Fonts & Assets
The project uses:
- **Montserrat** (`gfonts://Montserrat@600`): Headers and numeric data (fetched automatically by ESPHome).
- **Material Symbols Outlined** (`gfonts://Material+Symbols+Outlined`): Battery and vehicle icons (fetched automatically by ESPHome).
- **Material Design Icons** (`fonts/materialdesignicons-webfont.ttf`): Weather icons and Monzo category glyphs (downloaded to `fonts/`, see [Step 3](#3-download-required-font)).
