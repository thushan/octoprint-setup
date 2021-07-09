# OctoPrint Configuration

Configuration for my running OctoPrint for printers on a single RaspberryPi 4 with 4Gb or 8Gb.

The main point of this example (outside of me documenting my own things) is to show you how to setup OctoPrint with multiple printers via the USB ports.

## Hardware

This setup is for a RaspberryPi 4Gb & 8GB running DietPi.

With these printers - with the mappings (for my machines):

* Wanhao Duplicator i3 Plus / Cocoon Create Touch
    - ` /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0` 
* FLSun Super Racer
    - `/dev/serial/by-id/usb-marlinfw.org_Marlin_USB_Device_03030F0D534D08544DFC0B33F5000006-if00`

## USB Device Mapping

Instead of mapping to `/dev/ttyUSB*` or `/dev/ttyACM*` which may change as you plug/unplug devices on your Pi, you can use the automapped unique identifier for your printer by looking at `/dev/serial/by-id/`.

```shell
$ ls -1 /dev/serial/by-id/
usb-1a86_USB2.0-Serial-if00-port0
usb-marlinfw.org_Marlin_USB_Device_03030F0D534D08544DFC0B33F5000006-if00
```

The above is for my two printers, yours will be unique to your printers plugged in. You can figure out which id is for which USB device with `lsusb`.

```shell
$ lsusb
Bus 002 Device 002: ID 0781:55a9 SanDisk Corp.
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 004: ID 1d50:6029 OpenMoko, Inc.
Bus 001 Device 003: ID 1a86:7523 QinHeng Electronics HL-340 USB-Serial adapter
Bus 001 Device 002: ID 2109:3431 VIA Labs, Inc. Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hu
```

On a side note, if you're curious, there's an article from [Sam Tardieu](https://rfc1149.net/) about [the difference between ttyUSB and ttyACM](https://rfc1149.net/blog/2013/03/05/what-is-the-difference-between-devttyusbx-and-devttyacmx/).

## Set UDEV Rule

Unfortunately by default, any serial devices can only be used by root users and if you're running  Docker as a non-root account, we need to create a new `udev` rule to allow the USB devices.

Create a new file at `/etc/udev/rules.d/99-serial.rules` with the following:

```
KERNEL=="ttyUSB[0-9]*",MODE="0666"
```

Gives users read/write access to your USB devices.

## Running the stack

You can bring up the stack with

```
$ docker-compose up -d .
```

As long as you have two printers plugged in to any of the USB ports, you'll have two instances of OctoPrint at these addresses - my printers as examples:

* **FLSun Super Racer** - port 1980 
* **Wanhao Duplicator i3** - port 1981
* **Code Server (VSCode)** - port 8443; you can mount `/printers` which contains the configuration for both Octoprint volumes.

You can wire up Octofarm to this container to bind to port 80 - coming soon.
