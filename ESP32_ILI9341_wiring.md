# ESP32-WROOM-32 (38-pin) to ILI9341 TFT Wiring Diagram

| TFT Pin    | ESP32 GPIO | ESP32 38-pin Physical Pin |
|------------|------------|---------------------------|
| VCC        | 3.3V/5V    | 1 (3V3) or 2 (5V)         |
| GND        | GND        | 14, 38, or 1 (GND)        |
| CS         | GPIO5      | 29                        |
| RESET      | GPIO4      | 24                        |
| DC         | GPIO2      | 22                        |
| SDI(MOSI)  | GPIO23     | 36                        |
| SCK        | GPIO18     | 30                        |
| LED        | 3.3V       | 1 (3V3)                   |
| SDO(MISO)  | (NC)       | (not connected)           |

- Always connect to the correct GPIO number, not just the physical pin number.
- If your ESP32 board has pin numbers printed, match them to the GPIO numbers above.
- For a visual diagram, use the Mermaid code below in a compatible viewer:

```
flowchart TD
    subgraph ESP32_WROOM_32_38pin
        VCC[3V3/5V Pin 1/2]
        GND1[GND Pin 14/38/1]
        CS[GPIO5 Pin 29]
        RESET[GPIO4 Pin 24]
        DC[GPIO2 Pin 22]
        MOSI[GPIO23 Pin 36]
        SCK[GPIO18 Pin 30]
        LED[3V3 Pin 1]
    end
    subgraph TFT_ILI9341
        TFT_VCC[VCC]
        TFT_GND[GND]
        TFT_CS[CS]
        TFT_RESET[RESET]
        TFT_DC[DC]
        TFT_MOSI[SDI_MOSI]
        TFT_SCK[SCK]
        TFT_LED[LED]
    end
    TFT_VCC --- VCC
    TFT_GND --- GND1
    TFT_CS --- CS
    TFT_RESET --- RESET
    TFT_DC --- DC
    TFT_MOSI --- MOSI
    TFT_SCK --- SCK
    TFT_LED --- LED
```

If you need an image-based diagram, let me know!
