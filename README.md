# MCP2515 CAN Controller with TJA1050 CAN Transceiver Wiring

The current page describes different posibilities of connecting a MCP2515 with TJA1050 to a set of board (e.g., Arduino, Raspberry Pis).

## Resources

* [CAN-Bus with Raspberry Pi: HowTo/Quickstart MCP2515 Kernel 4.4.x+]( https://vimtut0r.com/2017/01/17/can-bus-with-raspberry-pi-howtoquickstart-mcp2515-kernel-4-4-x/)
* [raspberrypi.org: CAN bus on raspberry pi with MCP2515]( https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=141052&sid=37e6879817d1f410311246f97a0a20a3)
* [How to Connect Raspberry Pi to CAN Bus]( http://youness.net/raspberry-pi/raspberry-pi-can-bus)
* [Say it with a CAN Bus and a Raspberry Pi]( https://modis.io/blog/say-it-with-a-can-bus/)

## Arduino UNO

| Pin  | UNO  | MCP2515  |
| ---- | ---- | -------- |
| INT  | 2    | INT      |
| MISO | 12   | SO       |
| MOSI | 11   | SI       |
| SCK  | 13   | SCK      |
| SS   | 10   | CS       |

## Arguino MEGA2560

| Pin  | MEGA | MCP2515  |
| ---- | ---- | -------- |
| INT  | 2    | INT      |
| MISO | 50   | SO       |
| MOSI | 51   | SI       |
| SCK  | 52   | SCK      |
| SS   | 53   | CS       |

## Raspberry Pi 3/4

To use a MCP2515 CAN controller with TJA1050 transciever [this modification](https://forums.raspberrypi.com/viewtopic.php?f=44&t=141052&sid=37e6879817d1f410311246f97a0a20a3) must be done.

### Single MCP2515

| RPi  | MCP2515+TJA1050 |
| ---- | --------------- |
| 22   | INT             |
| 21   | SO              |
| 19   | SI              |
| 23   | SCK             |
| 24   | CS              |
| 6    | GND             |
| 2    | TJA1050 VCC 5v  |
| 1    | MCP2515 VCC 3v3 |

Boot config:

```
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=8000000,interrupt=12
dtoverlay=spi1-1cs
```

### Double MCP2515
TODO

### MCP2515 with Optiga TPM SLx 9670

| RPi  | MCP2515+TJA1050 |
| ---- | --------------- |
| 37   | INT             |
| 35   | SO              |
| 38   | SI              |
| 40   | SCK             |
| 36   | CS              |
| 39   | GND             |
| 4    | TJA1050 VCC 5v  |
| 1    | MCP2515 VCC 3v3 |

Boot config:

```
  # Enable SPI
  dtparam=spi=on
  # CAN Mcp2515 interface
  dtoverlay=spi1-1cs,cs0_pin=16,cs0_spidev=off
  dtoverlay=mcp2515-can2,oscillator=8000000,interrupt=26
  # TPM SLB9670
  dtoverlay=tpm
```

### Raising a CAN interface at boot

Append to */etc/network/interfaces* the *ip link* command that raises the interface.

Example 1:

```
auto can0
  iface can0 can static
  bitrate 250000
```

Example 2:
```
auto can1
iface can1 inet manual
    pre-up /sbin/ip link set can1 type can bitrate 500000 loopback off restart-ms 100
    up /sbin/ifconfig can1 up
    down /sbin/ifconfig can1 down
```


