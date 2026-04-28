# EEE308-Weather-Station[README.md](https://github.com/user-attachments/files/27159576/README.md)
#  Arduino Uno-Based Mini Weather Station

> **EEE308 Microprocessors — Term Project**
> OSTIM Technical University | Electrical & Electronics Engineering | April 2026

---

##  Team Members

| Name | Student ID |
|------|-----------|
| Sayme Ayça Öztekin | 230202402 |
| İbrahim Yasin Aşıcı | 230202042 |

---

##  Project Overview

This project is a microcontroller-based mini weather station built with an **Arduino Uno**. It measures real-time environmental data and displays it on an LCD screen. A rule-based algorithm estimates the probability of rainfall using three sensor inputs.

### Features
-  Real-time temperature & humidity measurement (DHT11)
-  Atmospheric pressure measurement (BMP180 / BMP280)
-  LCD display with two alternating pages
-  LED alert when temperature exceeds threshold
-  Rain probability prediction (0–100%)
-  Serial Monitor & Serial Plotter support

---

##  Hardware Components

| Component | Role |
|-----------|------|
| Arduino Uno (ATmega328P) | Main microcontroller |
| DHT11 | Temperature & humidity sensor |
| BMP180 / BMP280 | Atmospheric pressure sensor (I2C) |
| LM044L / LM016L LCD | Real-time data display |
| LED-RED + 220 Ω resistor | Temperature alarm |
| 4.7 kΩ × 3 resistors | I2C pull-up & DHT11 data pull-up |
| 100 nF × 3 capacitors | Power supply bypass |
| LM1117DT-3.3 | 3.3V regulator for BMP VDDIO |

---

##  System Architecture

```
┌─────────────────────────────────────────────┐
│              Arduino Uno (MCU)              │
│                                             │
│  DHT11 ──────► Temperature & Humidity       │
│  BMP180 ─────► Atmospheric Pressure         │
│                      │                      │
│              Prediction Module              │
│           Rain Probability (0-100%)         │
│                      │                      │
│         ┌────────────┴────────────┐         │
│       LCD Display            LED Alert      │
└─────────────────────────────────────────────┘
```

---

##  Rain Probability Formula

The prediction module uses a weighted rule-based model:

```
P_rain = 0.55 × RH + 0.30 × (1013 − P_atm) + 0.15 × (25 − T)
```

| Parameter | Weight | Physical Meaning |
|-----------|--------|-----------------|
| RH (Humidity %) | 0.55 | Dominant factor — air saturation |
| P_atm (hPa) | 0.30 | Low pressure = unstable weather |
| T (°C) | 0.15 | Lower temp supports condensation |

Result is clamped to **[0, 100]%**.

---

##  LCD Display Pages

The LCD cycles every 2 seconds between two pages:

```
┌────────────────┐     ┌────────────────┐
│ Sicaklik:27.0C │     │ Rain Prob:     │
│ Nem Orani:%80  │ ──► │ %80            │
│ Basinc:1013hPa │     │                │
│ Ya9mur :%80    │     │                │
└────────────────┘     └────────────────┘
```

---

##  Pin Connections

| Signal | Arduino Pin |
|--------|------------|
| DHT11 DATA | D7 |
| LED Anode | D8 |
| BMP SCL | A5 |
| BMP SDA | A4 |
| LCD RS | D7* |
| LCD E | D8* |
| LCD D4–D7 | D9–D12 |
| LCD VEE | GND |
| LCD RW | GND |
| BMP VDDIO | 3.3V (LM1117) |

---

##  Libraries Required

Install via Arduino IDE → Library Manager:

```
DHT sensor library       — Adafruit
Adafruit BMP085 Library  — Adafruit  (covers BMP180)
Adafruit Unified Sensor  — Adafruit
LiquidCrystal            — Built-in (no install needed)
Wire                     — Built-in (no install needed)
```

---

##  Simulation

The circuit was designed and verified in **Proteus ISIS** using the SIMULINO UNO model.

### Test Scenarios

| Scenario | Temp | Humidity | Pressure | Rain % | LED |
|----------|------|----------|----------|--------|-----|
| Normal | 27°C | 80% | 1013 hPa | 80% | ON |
| High Temp | 31°C | 80% | 1013 hPa | 80% | ON |
| Storm Risk | 31°C | 90% | 915 hPa | 100% | ON |

---

##  Repository Structure

```
mini-weather-station/
├── eee308termproject.ino          # Arduino source code
├── EEE308_WeatherStation_Report.docx  # Full project report
├── README.md                      # This file
└── simulation/
    ├── sim_normal.jpeg            # Scenario 1 screenshot
    ├── sim_high_temp.jpeg         # Scenario 2 screenshot
    └── sim_storm.jpeg             # Scenario 3 screenshot
```

---

##  License

This project was developed for academic purposes at OSTIM Technical University.
© 2026 Sayme Ayça Öztekin & İbrahim Yasin Aşıcı
