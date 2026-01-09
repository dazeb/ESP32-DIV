# 🔌 ESP32-DIV Prototype Wiring Guide

This guide maps your components to the pins defined in the codebase (`shared.h`).

## 🧠 Main Board (Core)

### 1. PCF8574 I/O Expander
*Address: 0x20*
| PCF Pin | Component | Note |
| :--- | :--- | :--- |
| **SDA** | **ESP32 GPIO 8** | Or board labeled SDA |
| **SCL** | **ESP32 GPIO 9** | Or board labeled SCL |
| **VCC** | **3.3V** | |
| **GND** | **GND** | |
| **P3** | Button **LEFT** | Active Low |
| **P4** | Button **RIGHT** | Active Low |
| **P5** | Button **DOWN** | Active Low |
| **P6** | Button **SELECT** | Active Low |
| **P7** | Button **UP** | Active Low |

### 2. Display (ILI9341 TFT)
*Check `User_Setup.h` in your `TFT_eSPI` library to confirm these matches your wiring.*
| TFT Pin | ESP32 Pin | Note |
| :--- | :--- | :--- |
| **VCC** | **3.3V** | |
| **GND** | **GND** | |
| **CS** | **GPIO 10** | Shared with SD_CS in some configs, check setup! |
| **RESET** | **RST** | Or dedicated GPIO |
| **DC** | **GPIO 2** | (Example - Check Library) |
| **MOSI** | **GPIO 11** | |
| **SCK** | **GPIO 12** | |
| **LED** | **3.3V** | Or PWM pin if dimming supported |
| **MISO** | **GPIO 13** | |

### 3. Touch Screen (XPT2046)
*Defined in `shared.h`*
| Touch Pin | ESP32 Pin |
| :--- | :--- |
| **CS** | **GPIO 18** |
| **MOSI** | **GPIO 35** |
| **MISO** | **GPIO 37** |
| **CLK** | **GPIO 36** |

---

## 🛡️ Shield Modules

### 4. NRF24L01+ Modules (2.4GHz)
*You have 2 modules. Populate Radio 1 and Radio 2.*

**Radio 1:**
| Pin | ESP32 Pin |
| :--- | :--- |
| **CE** | **GPIO 15** |
| **CSN** | **GPIO 4** |
| **MOSI** | **GPIO 11** |
| **MISO** | **GPIO 13** |
| **SCK** | **GPIO 12** |

**Radio 2:**
| Pin | ESP32 Pin |
| :--- | :--- |
| **CE** | **GPIO 47** |
| **CSN** | **GPIO 48** |
| **MOSI** | **GPIO 11** |
| **MISO** | **GPIO 13** |
| **SCK** | **GPIO 12** |

*(Radio 3 is on GPIO 14/21 but conflicts with IR - Leave unpopulated)*

### 5. CC1101 Module (Sub-GHz)
| Pin | ESP32 Pin |
| :--- | :--- |
| **CSN** | **GPIO 5** |
| **MOSI** | **GPIO 11** |
| **MISO** | **GPIO 13** |
| **SCK** | **GPIO 12** |
| **GDO0** | **GPIO 6** | (TX) |
| **GDO2** | **GPIO 3** | (RX) |

### 6. Infrared (IR)
| Component | ESP32 Pin |
| :--- | :--- |
| **Receiver (Signal)** | **GPIO 21** |
| **Transmitter (Signal)** | **GPIO 14** |

---

## ⚠️ Critical Notes
1.  **Voltage:** All modules (ESP32, NRF24, CC1101, TFT) must run on **3.3V**. Do **NOT** connect to 5V.
2.  **SPI Bus:** The Display, SD Card, NRF24s, and CC1101 all share the same **SPI Bus** (MOSI=11, MISO=13, SCK=12) but have different **CS (Chip Select)** pins. This is normal.
3.  **Conflict:** NRF Radio 3 uses the same pins as IR. Since you are building the prototype with IR, **do not connect a 3rd NRF24 module**.
