![MeshtasticRouterNode](docs/LoraHarvesterBox.jpg)

## What is this thing?

This generic LoRa base board is built around the ST STM32WLE5 (RAK3172) and the TI BQ25570 energy harvester.

It is suitable for a wide range of applications, including:

- Meshtastic (repeater, limited router, sensor)
- LoRaWAN sensors
- Generic raw LoRa devices
- Or virtually any other custom project

For persistent data storage, an optional FRAM IC (Infineon FM24V10) can be mounted on the back and connected via I²C. A variety of sensors can be integrated through I²C, SPI (on the back), or USART interfaces.

The power supply design is highly flexible: alongside standard JST-PH connectors, the board also supports very small LiPo batteries (commonly used in in-ear headphones or Bluetooth headsets), which can be soldered directly. Such batteries are readily available on AliExpress (e.g. 501010). The TI BQ25570 also supports exotic storages like Supercaps, LTO, etc.


## Design Decisions

### Why did you use the TI BQ25570?

It's the only energy harvester that supports up to 110mA peak current.

I'm not overly happy with this device as it lacks a few helpful features:

 - No I²C access and ADC for reading voltages/currents

 - No Thermistor for switching off charging above and below specific temperatures

 
### Why not buying a "naked" ST STM32WLE5? 

Using a SoC module from RAK brings all the HF and impedancy matching. 

Every leaving the housing is mostly not critically signal wise.

Also it's not too expensive with 6€.

### Why 3V and not 3.3V?

Improved system runtime by only loosing 1-2dB of TX power.

## Hardware

### Resistors Values

#### VOUT


| Target VOUT | R_OUT1  | R_OUT2 | R_SUM | Calculated VOUT |
|-------------|--------------|--------------|------------|-----------------|
| 1.8 V       | 8.66 MΩ       | 4.22 MΩ       | 12.88 MΩ      | ≈ 1.80V        |
| 3.0 V       | 5.10 MΩ       | 7.50 MΩ       | 12.60 MΩ     | ≈ 2.99V        |
| 3.3 V       | 4.70 MΩ       | 8.20 MΩ       | 12.90 MΩ     | ≈ 3.32V        |

#### LiPo

| R_OV1 | R_OV2 | R_SUM | V_OV |
|-------------|-------------|------------|-----------|
| 3.90 MΩ     | 5.10 MΩ     | 9.00       | ≈ 4.18V   |
| 3.60 MΩ     | 4.70 MΩ     | 8.30       | ≈ 4.18V   |
| 3.30 MΩ     | 4.30 MΩ     | 7.60       | ≈ 4.18V   |
| 4.30 MΩ     | 5.60 MΩ     | 9.90       | ≈ 4.17V   |
| 5.10 MΩ     | 6.20 MΩ     | 11.90      | ≈ 4.02V   |

| R_OK1  | R_OK2 | R_OK3 | Sum  | VBAT_OK_PROG | VBAT_OK_HYST |
|-------------|-------------|-------------|----------|------------------|------------------|
| 4.32 MΩ     | 6.34 MΩ     | 2.15 MΩ     | 12.81 MΩ    | ≈ 2.99V           | ≈ 3.59V           |
| 4.32 MΩ     | 6.80 MΩ     | 1.96 MΩ     | 13.08 MΩ   | ≈ 3.12V           | ≈ 3.66V           |
| 4.32 MΩ     | 7.15 MΩ     | 1.43 MΩ     | 12.90 MΩ   | ≈ 3.21V           | ≈ 3.61V           |
| 4.32 MΩ     | 7.50 MΩ     | 1.10 MΩ     | 12.92 MΩ   | ≈ 3.31V           | ≈ 3.61V           |
| 4.30 MΩ     | 7.50 MΩ     | 0.68 MΩ     | 12.48 MΩ   | ≈ 3.36V           | ≈ 3.55V           |

#### LTO

| R_OV1 | R_OV2 | R_SUM | V_OV (V) |
|-------------|-------------|------------|----------|
| 5.60 MΩ     | 2.74 MΩ     | 8.34 MΩ      | ≈ 2.70V     |
| 6.20 MΩ     | 3.00 MΩ     | 9.20 MΩ      | ≈ 2.69V     |
| 5.10 MΩ     | 2.74 MΩ     | 7.84 MΩ      | ≈ 2.79V     |
| 6.20 MΩ     | 3.30 MΩ     | 9.50 MΩ      | ≈ 2.78V     |
| 5.10 MΩ     | 2.87 MΩ     | 7.97 MΩ      | ≈ 2.84V     |

| R_OK1 | R_OK2 | R_OK3 | R_SUM | VBAT_OK_PROG | VBAT_OK_HYST  |
|-------------|-------------|-------------|------------|------------------|------------------|
| 7.15 MΩ     | 4.64 MΩ     | 1.18 MΩ     | 12.97 MΩ     | ≈ 2.00V           | ≈ 2.19V           |
| 6.80 MΩ     | 4.99 MΩ     | 1.13 MΩ     | 12.92 MΩ     | ≈ 2.10V           | ≈ 2.30V           |
| 6.49 MΩ     | 5.36 MΩ     | 1.05 MΩ     | 12.90 MΩ     | ≈ 2.21V           | ≈ 2.41V           |
| 6.65 MΩ     | 5.11 MΩ     | 1.10 MΩ     | 12.86 MΩ     | ≈ 2.14V           | ≈ 2.34V           |
| 6.04 MΩ     | 5.49 MΩ     | 1.00 MΩ     | 12.53 MΩ     | ≈ 2.31V           | ≈ 2.51V           |
| 6.19 MΩ     | 5.36 MΩ     | 1.02 MΩ     | 12.57 MΩ     | ≈ 2.26V           | ≈ 2.46V           |

## License

CERN Open Hardware Licence Version 2 - Strongly Reciprocal 

