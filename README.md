# 🎙️ Assistant Vocal Embarqué — RK3308B Custom PCB

Un assistant vocal autonome conçu sur PCB multi-couches custom, basé sur le module **Radxa rCore (RK3308B)**. Il intègre un réseau de microphones MEMS, un array de capteurs environnementaux, une connectivité réseau filaire et sans fil, et une interface de contrôle via ESP32.

---

## 🧠 Concept

Ce projet est un assistant vocal embarqué autonome. Il capture la voix via 4 microphones MEMS I2S, traite le signal audio sous Linux, et restitue la réponse via un ampli DAC I2S. Des capteurs environnementaux (température, humidité, lumière, proximité, orientation) enrichissent le contexte. La connectivité réseau (Ethernet + WiFi/BT) permet l'accès à des services cloud ou à un backend local.

Le produit est réparti sur **5 PCB interconnectés** via connecteurs FPC.

---

## 🗂️ Architecture matérielle

```
┌─────────────────────────────────────────────┐
│              PCB Cerveau (principal)         │
│  rCore RK3308B · ESP32 · RTC · USB · ETH    │
└────────┬──────────────┬───────────────┬──────┘
         │ FPC 17P      │ FPC 12P       │ FPC 5P
    ┌────┴────┐    ┌────┴────┐    ┌─────┴────┐
    │ Dessus  │    │ Dessous │    │ Contour  │
    │ Haut    │    │  Bas    │    │ LEDs RGB │
    └────┬────┘    └─────────┘    └──────────┘
         │ FPC 6P
    ┌────┴────┐
    │  Avant  │
    │  ToF    │
    └─────────┘
```

---

## 🖥️ PCB Cerveau — Plateforme principale

### Compute
| Composant | Réf. | Description |
|---|---|---|
| **SOM** | Radxa rCore (RK3308B) | Module quad-core ARM Cortex-A35 @ 1.3GHz, 512MB DDR3, 8GB eMMC |
| **Coprocesseur** | ESP32-WROOM-32U-N4 | Microcontrôleur WiFi 802.11b/g/n + BT 4.2, connecté via I2C1 au RK3308B |

### Connectivité réseau
| Composant | Réf. | Description |
|---|---|---|
| **PHY Ethernet** | Microchip LAN8720A | PHY RMII 10/100Mbps, adresse MDIO 0x01, REFCLK 50MHz généré par le SoC, reset GPIO0_B7 |
| **Transformateur ETH** | PULSE HX1188NL | Filtre magnétique Ethernet intégré, couplé au RJ45 |

### USB & Interfaces
| Composant | Réf. | Description |
|---|---|---|
| **USB-UART #1** | WCH CH340K | Pont USB-UART pour programmation et debug de l'ESP32 |
| **USB-UART #2** | WCH CH340K | Pont USB-UART pour console debug Linux du RK3308B (UART0) |
| **USB Hub** | Terminus FE1.1S | Hub USB 1.1 BSOP28, gestion multi-port USB |

### Temps réel & Alimentation
| Composant | Réf. | Description |
|---|---|---|
| **RTC** | NXP PCF2129T | Horloge temps réel I2C avec backup batterie, SO-16 |
| **Buck converter** | MP2161 | Convertisseur abaisseur DC/DC TSOT23-8 |
| **Buzzer** | MLT-9032 | Buzzer piézo SMD 9x3mm |

### Afficheurs
| Composant | Réf. | Interface | Description |
|---|---|---|---|
| **eInk 2.9"** | Waveshare 2.9" G | SPI2 | Écran e-paper 296×128 px, connecteur FPC 24P, faible consommation |
| **OLED 0.49"** | Wisevision X049-6432TSWPG02-H14 | I2C | Écran OLED 64×32 px (sur PCB Dessous) |

### Discrets actifs (Cerveau)
| Composant | Réf. | Rôle |
|---|---|---|
| MOSFET N | BSS138 x2 | Level shifting logique |
| Buffer 3-state | 74AHCT1G125GV x2 | Level shifter 5V↔3.3V |
| MOSFET P | SI1308EDL | Commutation alimentation |
| BJT NPN | MMBT3904 x2 | Pilotage signaux |
| Diode Schottky | MBR0530 x3 | Protection/redressement |

### Oscillateurs (Cerveau)
| Composant | Fréquence | Usage |
|---|---|---|
| Quartz SMD | 25 MHz | Référence Ethernet PHY |
| Quartz SMD | 12 MHz | Référence CH340K |

---

## 🔊 PCB Dessus (Haut) — Audio & Capteurs

### Audio
| Composant | Réf. | Description |
|---|---|---|
| **Micro MEMS x4** | Knowles SPH0645LM4H-B-8 | Microphones MEMS numériques I2S 24 bits, 4 canaux indépendants (SDI0/SDI1/SDI2/SDI3 sur I2S0) |
| **Ampli DAC** | Maxim MAX98357A | Ampli classe D mono I2S 3.2W, enable via GPIO0_B2 (sur Cerveau) |

### Capteurs environnementaux
| Composant | Réf. | Interface | Description |
|---|---|---|---|
| **IMU 6 axes** | Bosch BMI160 | I2C/SPI | Accéléromètre + gyroscope, adresse 0x68 |
| **Magnétomètre** | ST LIS2MDLTR | I2C | Magnétomètre 3 axes, adresse 0x1E |
| **Lumière ambiante** | TI OPT3001DNPR | I2C | Capteur lux calibré spectre visible |
| **Temp/Humidité** | TI HDC1080DMBR | I2C | Température + humidité relative, adresse 0x40 |

### GPIO & LEDs
| Composant | Réf. | Description |
|---|---|---|
| **I2C GPIO expander** | TI PCA9554DW | Expander I2C 8 bits SOIC-16 |
| **Level shifter** | 74AHCT1G125GV x2 | Buffer 3-state 5V↔3.3V |
| **LED RGB** | Worldsemi WS2812B-2020 x1 | LED RGB adressable 2×2mm |
| **LED RGB** | Worldsemi WS2812C-4020 x15 | LED RGB adressable 4×2mm, array |

---

## 📟 PCB Dessous (Bas) — Alimentation & Interface

| Composant | Réf. | Interface | Description |
|---|---|---|---|
| **Moniteur puissance** | TI INA260AIPW | I2C | Mesure tension/courant/puissance TSSOP-16 |
| **I2C GPIO expander** | NXP PCA9536D | I2C | Expander I2C 4 bits SOIC-8 |
| **OLED 0.49"** | Wisevision X049-6432TSWPG02-H14 | I2C | Écran OLED 64×32px, connecté sur I2C (I2C1 ou I2C2) |
| **LDO OLED** | TOREX XC6206P402MR | — | Régulateur 4.0V dédié alimentation OLED |
| **MOSFET P** | FDN338P | — | Commutation alimentation |
| **MOSFET N** | FDN335N | — | Commutation signal |

---

## 📡 PCB Avant — Détection de proximité

| Composant | Réf. | Interface | Description |
|---|---|---|---|
| **Capteur ToF** | ST VL53L0CXV0DH/1 | I2C | Télémètre laser Time-of-Flight, portée 2m, 12P LGA |

---

## 💡 PCB Contour — Éclairage

| Composant | Réf. | Description |
|---|---|---|
| **LED RGB adressable x15** | 欧思科 SK6812 | LEDs RGB 5×5mm adressables, anneau décoratif/status |

---

## 🔌 Interfaces logicielles utilisées

| Interface | Bus | Périphériques |
|---|---|---|
| I2S0 Master | GPIO2 | MAX98357A (SDO0) + 4× SPH0645 (SDI0–3) |
| SPDIF TX | GPIO0_C1 | Sortie numérique audio |
| GMAC RMII | GPIO1_B4→C5 | LAN8720A |
| SPI2 | GPIO1_C6/C7/D0/D1 | eInk 2.9" 296×128px (Waveshare) |
| I2C1 | GPIO0_B3/B4 | ESP32 |
| I2C2 | GPIO2_A2/A3 | Capteurs (BMI160, LIS2MDL, OPT3001, HDC1080, INA260, PCA9554, PCA9536, VL53L0) + OLED 0.49" |
| UART0 | GPIO2_A0/A1 | Console debug |
| GPIO | GPIO0 divers | LEDs, boutons, enable MAX98357A, reset PHY |

---

## 🗂️ Fichiers du dépôt

```
├── hardware/
│   └── rk3308-custom-pcb.dts    # Device tree overlay Linux
├── bom/
│   ├── BOM_Cerveau.xlsx
│   ├── BOM_Dessus.xlsx
│   ├── BOM_Dessous.xlsx
│   ├── BOM_Avant.xlsx
│   └── BOM_Contour.xlsx
└── README.md
```

---

## 🔧 Environnement logiciel

- **OS** : Debian (image Radxa Pi S0)
- **Kernel** : Linux 5.x BSP Rockchip (modifié)
- **Audio** : ALSA / PipeWire
- **Device tree** : overlay custom `rk3308-custom-pcb.dts`

---

## ⚠️ Notes de développement

> Ce PCB est en cours de validation matérielle.
>
> Points à valider au premier boot :
> - Décalage 1 bit SPH0645 (format Philips I2S non standard)
> - Direction REFCLK LAN8720A (`clock_in_out = "output"`)
> - Adresse MDIO PHY (0x01 via PHYAD0/RXER pull-up 10kΩ)
> - Adresses I2C des capteurs (BMI160 : 0x68, LIS2MDL : 0x1E, OPT3001 : configurable, HDC1080 : 0x40, INA260 : configurable)
