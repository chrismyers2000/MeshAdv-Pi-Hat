# MeshAdv Pi Hat

The MeshAdv Pi Hat is a 1 Watt Raspberry Pi hat designed to be used with the Linux-native version of Meshtastic known as meshtasticd. The board includes space for a GPS module and breakout for I2C bus. 
This makes for a good "base station" or "Router" node that can be mounted high on a pole and powered over POE (using separate POE adapter). No more need to retrieve the node everytime you want to update firmware, it can all be done remotely. It also makes it easy and reliable to connect to MQTT.

I have now created a version without the SMA connector. See: No ANT version, it uses a 2 layer board design so it will be cheaper to produce.

- V1.0 is tested and 100% works. Please note, the GPS header is supplied with 5V on this version. Output on the E22 has been measured at 29.3dbm in my tests when `lora.tx_power` is set to 22.
- V1.1 is tested and 100% works.

Some PCB's may be available here: https://frequencylabs.etsy.com New batch has arrived!

![](https://github.com/chrismyers2000/MeshAdv-Pi-Hat/blob/d43ea52d606c9e0167098d327dad065feb6ee043/V1.1/IPEX/Photos/3D_PCB%20V1.1_Top_IPEX.png)

# Info

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

Click here for the new configuration method: [https://meshtastic.org/docs/hardware/devices/linux-native-hardware/#configuration]

The old method is below and still works if you prefer it


In /etc/meshtasticd/config.yaml, add or uncomment the following lines as needed.
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

