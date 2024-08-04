# MeshAdv Pi Hat

The MeshAdv Pi Hat is a 1 Watt Raspberry Pi hat designed to be used with the Linux-native version of Meshtastic known as meshtasticd. The board includes space for a GPS module and breakout for I2C bus. This hat is experimental at this point.

![https://github.comMeshAdv Pi Hat/V1.0/Photos/3D Top.png)](https://github.com/chrismyers2000/MeshAdv-Pi-Hat/blob/d284307009050c0d9ed418b5709d0a8eee448441/V1.0/Photos/3D%20Top.png)

# Info

== NOTICE!! always have an antenna connected to the Hat when powered on, failure to do so can damage the E22 module. ==

Watch this video first: [How to install Meshtastic on Raspberry Pi](https://www.youtube.com/watch?v=vLGoEPNT0Mk)

I followed the video exactly and had no problems. I was using a Raspberry Pi Zero 2 W running 64bit Raspberry Pi OS Debian Bookworm. Using the official "Raspberry Pi Imager" makes this very easy.

https://meshtastic.org/docs/hardware/devices/linux-native-hardware/

https://meshtastic.org/docs/software/linux-native/


# Configuration

In /etc/meshtasticd/config.yaml, add or uncomment the following lines as needed.
```yaml
Lora:
  Module: sx1262  # Ebyte E22-900M30S choose only one module at a time
# Module: sx1268  # Ebyte E22 400M33S
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

