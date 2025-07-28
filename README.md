![](https://github.com/chrismyers2000/MeshAdv-Pi-Hat/blob/4b7559ed795698574ae7a1e3c009874af9be14ab/Misc/MeshAdv%20Pi%20Hat%20logo.png)

The MeshAdv Pi Hat is a 1 Watt LoRa hat designed to be used with the Linux-native version of Meshtastic known as meshtasticd. The board includes space for a GPS module and breakout for I2C bus. 
This makes for a good "base station" or "Router" node that can be mounted high on a pole and powered over POE (using separate POE adapter). No more need to retrieve the node everytime you want to update firmware, it can all be done remotely. It also makes it easy and reliable to connect to MQTT.

---

Fully assembled units will be available here: https://frequencylabs.etsy.com 

![](https://github.com/chrismyers2000/MeshAdv-Pi-Hat/blob/d43ea52d606c9e0167098d327dad065feb6ee043/V1.1/IPEX/Photos/3D_PCB%20V1.1_Top_IPEX.png)

== NOTICE!! always have an antenna connected to the Hat when powered on, failure to do so can damage the E22 module. ==

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

You can try my new [Configuration Tool](https://github.com/chrismyers2000/Meshtasticd-Configuration-Tool), It can install and configure everything you need to get up and running, otherwise continue below with the official instructions. 
Please note: The MeshAdv Pi Hat does not have the HAT+ eeprom, so you will see no hat detected....this is normal.
![](https://github.com/chrismyers2000/Meshtasticd-Configuration-Tool/blob/b0ad9208a440797aa12c39bf289b6498ebf8cc92/Gui/ConfigTool1.png)

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

# GPS

   - Jumper J3 is used to select either 3.3v or 5v to the GPS 
   - The recommended GPS is the ATGM336H module. With it you can utilize the PPS output for very precise time keeping, useful for running an NTP server alongside Meshtastic.
   - You can use any GPS that outputs NMEA sentences using UART. 
   - Start by following the official instructions to get the GPS working with meshtasticd [CLICK HERE](https://meshtastic.org/docs/hardware/devices/linux-native-hardware/#uart-raspberry-pi)
   - PPS output: Starting 1/1/25 all V1.1 boards have GPS PPS routed to GPIO 23 (pin 16).
