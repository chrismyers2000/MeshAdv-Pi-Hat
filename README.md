# MeshAdv Pi Hat

The MeshAdv Pi Hat is a 1 Watt Raspberry Pi hat designed to be used with the Linux-native version of Meshtastic known as meshtasticd. The board includes space for a GPS module and breakout for I2C bus. 
This makes for a good "base station" or "Router" node that can be mounted high on a pole and powered over POE (using separate POE adapter). No more need to retrieve the node everytime you want to update firmware, it can all be done remotely. It also makes it easy and reliable to connect to MQTT.

I have now created a version without the SMA connector. See: No ANT version, it uses a 2 layer board design so it will be cheaper to produce.

- V1.0 is tested and 100% works. Please note, the GPS header is supplied with 5V on this version. Output on the E22 has been measured at 29.3dbm in my tests.
- V1.1 is tested and 100% works.

Some PCB's may be available here: https://frequencylabs.etsy.com Out of Stock! Check back in 1-2 weeks. New batch has been ordered!

![](https://github.com/chrismyers2000/MeshAdv-Pi-Hat/blob/38716f06be6e8867a65c0815f295fbd1433ee2e0/V1.1/Photos/3D_PCB%20V1.1_Top.png)

# Info

== NOTICE!! always have an antenna connected to the Hat when powered on, failure to do so can damage the E22 module. ==

To use the GPS you must select your voltage using J3. Solder together the desired pads (Left+Center for 3.3V and Right+Center for 5V) On V1.0, GPS voltage is fixed at 5V. The recommended GPS module is the ATGM336H.

Watch this video first: [How to install Meshtastic on Raspberry Pi](https://www.youtube.com/watch?v=vLGoEPNT0Mk)

I followed the video exactly and had no problems. I was using a Raspberry Pi Zero 2 W running 64bit Raspberry Pi OS Debian Bookworm. Using the official "Raspberry Pi Imager" makes this very easy.

https://meshtastic.org/docs/hardware/devices/linux-native-hardware/

https://meshtastic.org/docs/software/linux-native/


# Configuration


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

GPS:
  SerialPath: /dev/ttyS0

I2C:
  I2CDevice: /dev/i2c-1

Logging:
  LogLevel: info # debug, info, warn, error

Webserver:
  Port: 443 # Port for Webserver & Webservices
  RootPath: /usr/share/doc/meshtasticd/web # Root Dir of WebServer

General:
  MaxNodes: 200
```

