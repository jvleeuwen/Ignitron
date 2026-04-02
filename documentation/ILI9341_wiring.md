# ILI9341 Color TFT Wiring for Ignitron

Connect your 2.4" 320x240 SPI TFT (ILI9341) as follows (adjust if your screen uses different pins):

| TFT Pin | ESP32 Pin | Description         |
|---------|-----------|---------------------|
| VCC     | 3.3V/5V   | Power               |
| GND     | GND       | Ground              |
| CS      | GPIO5     | Chip Select         |
| RESET   | GPIO4     | Reset               |
| DC      | GPIO2     | Data/Command        |
| SDI(MOSI)| GPIO23   | SPI MOSI            |
| SCK     | GPIO18    | SPI Clock           |
| LED     | 3.3V      | Backlight (always on)|
| SDO(MISO)| (NC)     | Not used            |

- Update the pin numbers in SparkDisplayControl.cpp if your wiring differs.
- Make sure SPI MOSI/SCK match your ESP32 hardware SPI pins for best performance.
- Touch pins (if present) are not used by default.

