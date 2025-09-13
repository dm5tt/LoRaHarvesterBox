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

 
### Why not not buying a "naked" ST STM32WLE5? 

Using a SoC module from RAK brings all the HF and impedancy matching. 

Every leaving the housing is mostly not critically signal wise.

Also it's not too expensive with 6€.

### Why 3V and not 3.3V?

Improved system runtime by only loosing 1-2dB of TX power.

## Hardware

### Resistors Values

#### VOOUT


| Target VOUT | R_OUT1 (E24) | R_OUT2 (E24) | R_SUM (MΩ) | Calculated VOUT |
|-------------|--------------|--------------|------------|-----------------|
| 1.8 V       | 8.66 MΩ       | 4.22 MΩ       | 12.88      | ≈ 1.80V        |
| 3.0 V       | 5.10 MΩ       | 7.50 MΩ       | 12.60      | ≈ 2.99V        |
| 3.3 V       | 4.70 MΩ       | 8.20 MΩ       | 12.90      | ≈ 3.32V        |

#### LiPo

| R_OV1 | R_OV2 | R_SUM (MΩ) | V_OV |
|-------------|-------------|------------|-----------|
| 3.90 MΩ     | 5.10 MΩ     | 9.00       | ≈ 4.18V   |
| 3.60 MΩ     | 4.70 MΩ     | 8.30       | ≈ 4.18V   |
| 3.30 MΩ     | 4.30 MΩ     | 7.60       | ≈ 4.18V   |
| 4.30 MΩ     | 5.60 MΩ     | 9.90       | ≈ 4.17V   |
| 5.10 MΩ     | 6.20 MΩ     | 11.90      | ≈ 4.02V   |


| R_OK1  | R_OK2 | R_OK3 | Sum (MΩ) | VBAT_OK_PROG | VBAT_OK_HYST |
|-------------|-------------|-------------|----------|------------------|------------------|
| 4.32 MΩ     | 6.34 MΩ     | 2.15 MΩ     | 12.81    | ≈ 2.99V           | ≈ 3.59V           |
| 4.32 MΩ     | 6.80 MΩ     | 1.96 MΩ     | 13.08    | ≈ 3.12V           | ≈ 3.66V           |
| 4.32 MΩ     | 7.15 MΩ     | 1.43 MΩ     | 12.90    | ≈ 3.21V           | ≈ 3.61V           |
| 4.32 MΩ     | 7.50 MΩ     | 1.10 MΩ     | 12.92    | ≈ 3.31V           | ≈ 3.61V           |
| 4.30 MΩ     | 7.50 MΩ     | 0.68 MΩ     | 12.48    | ≈ 3.36V           | ≈ 3.55V           |

## License

CERN Open Hardware Licence Version 2 - Strongly Reciprocal 

