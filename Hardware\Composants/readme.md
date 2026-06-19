## 🔌 Components connected to the Radxa module (excluding passives)

### Brain PCB (directly connected to the rCore SOM)

- ESP32-WROOM-32U-N4 (coprocessor, I2C1)
- LAN8720A (RMII Ethernet PHY)
- PCF2129T (RTC, I2C)
- CH340K ×2 (USB-UART)
- FE1.1S (USB hub)
- HX1188NL (Ethernet transformer)
- MP2161 (buck converter)
- MLT-9032 (buzzer)
- eInk Waveshare 2.9" 296×128px (SPI2)

### Top PCB (via FPC connector to I2C2)

- BMI160 (6-axis IMU)
- LIS2MDLTR (magnetometer)
- OPT3001DNPR (ambient light)
- HDC1080DMBR (temperature/humidity)
- PCA9554DW (8-bit GPIO expander)
- 4× SPH0645LM4H (I2S microphones, not I2C)
- MAX98357A (I2S amplifier, not I2C)

### Bottom PCB (via FPC to I2C2)

- INA260AIPW (power monitor)
- PCA9536D (4-bit GPIO expander)
- OLED 0.49" Wisevision (I2C)

### Front PCB (via FPC to I2C2)

- VL53L0CXV0DH/1 (ToF sensor)

### LEDs (non-I2C, controlled by the ESP32)

- SK6812 ×15 (Contour ring)
- WS2812B/C ×17 (Top ring + status)


Composants reliés au module Radxa (hors passifs)
Basé sur tout ce qu'on a établi ensemble :
Sur PCB Cerveau (directement liés au SOM rCore) :

ESP32-WROOM-32U-N4 (coprocesseur, I2C1)
LAN8720A (PHY Ethernet RMII)
PCF2129T (RTC, I2C)
CH340K ×2 (USB-UART)
FE1.1S (hub USB)
HX1188NL (transfo Ethernet)
MP2161 (buck converter)
MLT-9032 (buzzer)
eInk Waveshare 2.9" 296×128px (SPI2)

Sur PCB Dessus (via connecteur FPC vers I2C2) :

BMI160 (IMU 6 axes)
LIS2MDLTR (magnétomètre)
OPT3001DNPR (lumière ambiante)
HDC1080DMBR (température/humidité)
PCA9554DW (GPIO expander 8 bits)
4× SPH0645LM4H (micros I2S, pas I2C)
MAX98357A (ampli I2S, pas I2C)

Sur PCB Dessous (via FPC vers I2C2) :

INA260AIPW (moniteur puissance)
PCA9536D (GPIO expander 4 bits)
OLED 0.49" Wisevision (I2C)

Sur PCB Avant (via FPC vers I2C2) :

VL53L0CXV0DH/1 (capteur ToF)

LEDs (non-I2C, contrôlées par l'ESP32) :

SK6812 ×15 (Contour)
WS2812B/C ×17 (Dessus + status)
