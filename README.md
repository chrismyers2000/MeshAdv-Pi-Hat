# MeshAdv Pi Hat

The MeshAdv Pi Hat is a 1 Watt Raspberry Pi hat designed to be used with the Linux-native version of Meshtastic known as meshtasticd. The board includes space for a GPS module and breakout for I2C bus. 
This makes for a good "base station" or "Router" node that can be mounted high on a pole and powered over POE (using separate POE adapter). No more need to retrieve the node everytime you want to update firmware, it can all be done remotely. It also makes it easy and reliable to connect to MQTT.

I have now created a version without the SMA connector. See: No ANT version, it uses a 2 layer board design so it will be cheaper to produce.

- V1.0 is tested and 100% works. Please note, the GPS header is supplied with 5V on this version. Output on the E22 has been measured at 29.3dbm in my tests when `lora.tx_power` is set to 22.
- V1.1 is tested and 100% works.

Fully assembled units will be available here: https://frequencylabs.etsy.com 

![](https://github.com/chrismyers2000/MeshAdv-Pi-Hat/blob/d43ea52d606c9e0167098d327dad065feb6ee043/V1.1/IPEX/Photos/3D_PCB%20V1.1_Top_IPEX.png)

# Info

|Pin# |GPIO|Pin Name   |Description            |   |   |Pin# |GPIO|Pin Name   |Description                      |
|-----|----|-----------|-----------------------|---|---|-----|----|-----------|---------------------------------|
|1    |    |3.3V       |                       |   |   |2    |    |5V         |                                 |
|3    |2   |SDA        |(I2C)                  |   |   |4    |    |5V         |                                 |
|5    |3   |SCL        |(I2C)                  |   |   |6    |    |GND        |                                 |
|7    |4   |Unused     |                       |   |   |8    |14  |UART TX    |(GPS)RX                          |
|9    |    |GND        |                       |   |   |10   |15  |UART RX    |(GPS)TX                          |
|11   |17  |Unused     |                       |   |   |12   |18  |RST        |(LoRa)                           |
|13   |27  |Unused     |                       |   |   |14   |    |GND        |                                 |
|15   |22  |Unused     |                       |   |   |16   |23  |PPS        |(GPS) Pulse Per Second           |
|17   |    |3.3V       |                       |   |   |18   |24  |Unused     |                                 |
|19   |10  |MOSI       |(LoRa) SPI             |   |   |20   |    |GND        |                                 |
|21   |9   |MISO       |(LoRa) SPI             |   |   |22   |25  |Unused     |                                 |
|23   |11  |CLK        |(LoRa) SPI             |   |   |24   |8   |Unused     |                                 |
|25   |    |GND        |                       |   |   |26   |7   |Unused     |                                 |
|27   |0   |Unused     |                       |   |   |28   |1   |Unused     |                                 |
|29   |5   |Unused     |                       |   |   |30   |    |GND        |                                 |
|31   |6   |Unused     |                       |   |   |32   |12  |RXEN       |(LoRa) Recieve Enable            |
|33   |13  |TXEN       |(LoRa) Transmit Enable |   |   |34   |    |GND        |                                 |
|35   |19  |Unused     |                       |   |   |36   |16  |IRQ        |(LoRa)                           |
|37   |26  |Unused     |                       |   |   |38   |20  |BUSY       |(LoRa)                           |
|39   |    |GND        |                       |   |   |40   |21  |NSS        |(LoRa)                           |

== NOTICE!! always have an antenna connected to the Hat when powered on, failure to do so can damage the E22 module. ==

To use the GPS you must select your voltage using J3. Use the jumpers or solder together the desired pads (Left+Center for 3.3V and Right+Center for 5V) On V1.0, GPS voltage is fixed at 5V. The recommended GPS module is the ATGM336H which requires 5v.

Starting 1/1/25 all V1.1 boards will have GPS PPS routed to GPIO 23 (pin 16). 

# Compatibility

| Raspberry Pi Model      | Working? |
|-------------------------|----------|
| Raspberry Pi 1 Model A  | Never*   |
| Raspberry Pi 1 Model A+ | ???      |
| Raspberry Pi 1 Model B  | Never*   |
| Raspberry Pi 1 Model B+ | ???      |
| Raspberry Pi 2 Model B  | Yes      |
| Raspberry Pi 3 Model B  | Yes      |
| Raspberry Pi 3 Model B+ | Yes      |
| Raspberry Pi 3 Model A+ | Yes      |
| Raspberry Pi 4 Model B  | Yes      |
| Raspberry Pi 400        | Yes      |
| Raspberry Pi 5          | Yes      |
| Raspberry Pi 500        | Yes      |
| Raspberry Pi Zero       | Yes      |
| Raspberry Pi Zero W     | Yes      |
| Raspberry Pi Zero 2 W   | Yes      |
| Raspberry Pi Pico       | Never*   |
| Raspberry Pi Pico W     | Never*   |

*Raspberry Pi `1 Model A`, `1 Model B`, and `Pico` do not implement the 40-pin layout used in the MeshAdv Pi Hat.

# Installing Meshtasticd

~~Watch this video first: [How to install Meshtastic on Raspberry Pi](https://www.youtube.com/watch?v=vLGoEPNT0Mk)~~ This video covers the old method, still a good video but out of date.


Official installation instructions: [https://meshtastic.org/docs/hardware/devices/linux-native-hardware/]



# Configuration

These instructions assume you are using a raspberry pi with Raspberry Pi OS. 

## New Method:

   - As methods keep changing, please [CLICK HERE](https://meshtastic.org/docs/hardware/devices/linux-native-hardware/#configuration) for the most up to date configuration process

---
## Old Method:

   - The old method is below and still works if you prefer it


```bash
sudo nano /etc/meshtasticd/config.yaml
```
add or uncomment the following lines as needed. 

```yaml
Lora:
  Module: sx1262  # Ebyte E22-900M30S and E22-900M33S choose only one module at a time
# Module: sx1268  # Ebyte E22 400M30S and E22-400M33S
  CS: 21
  IRQ: 16
  Busy: 20
  Reset: 18
  TXen: 13
  RXen: 12
  DIO3_TCXO_VOLTAGE: true
  # SX126X_MAX_POWER: 8  # 8=33dBm output. Ebyte E22-900M33S and E22-400M33S only

GPS:
  SerialPath: /dev/ttyS0
#  SerialPath: /dev/ttyAMA0 # For Raspberry Pi 5

I2C:
  I2CDevice: /dev/i2c-1

Logging:
  LogLevel: info # debug, info, warn, error

Webserver:
  Port: 443 # Port for Webserver & Webservices
  RootPath: /usr/share/meshtasticd/web # Root Dir of WebServer

General:
  MaxNodes: 200
```
## LoRa Setup:

- You must now set the LoRa Region to be able to start using Meshtastic. [CLICK HERE](https://meshtastic.org/docs/getting-started/initial-config/#set-regional-settings) for info on how to set region settings. Please note: Linux-Native is currently unable to connect over bluetooth or to the Apple app. All other methods are working. 
