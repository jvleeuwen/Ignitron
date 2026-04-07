# ILI9341 Color TFT Wiring for Ignitron

Connect your 2.4" 320x240 SPI TFT (ILI9341) as follows (adjust if your screen uses different pins):

| TFT Pin | ESP32 Pin | Description         |
|---------|-----------|---------------------|
| VCC     | 3.3V/5V   | Power               |
| GND     | GND       | Ground              |
| CS      | GPIO5     | Chip Select         |
| RESET   | GPIO4     | Reset               |
| DC      | GPIO2     | Data/Command        |
| SDI(MOSI)| GPIO21   | SPI MOSI            |
| SCK     | GPIO22    | SPI Clock           |
| LED     | 3.3V      | Backlight (always on)|
| SDO(MISO)| (NC)     | Not used            |

- Update the pin numbers in SparkDisplayControl.cpp if your wiring differs.
- In this project, SPI is intentionally mapped to GPIO21/GPIO22 because GPIO23/GPIO18 are used by LED/button logic.
- Touch pins (if present) are not used by default.

