# OctoPrint Configuration

Configuration for my running OctoPrint for printers on a single RaspberryPi 4 with 4Gb or 8Gb.

## Hardware

This setup is for a RaspberryPi 4Gb & 8GB running DietPi.

With these printers:

* Wanhao Duplicator i3 Plus / Cocoon Create Touch
    - `/dev/ttyUSB0` 
* FLSun Super Racer
    - `/dev/ttyACM0`

On a side note, if you're curious, there's an article from [Sam Tardieu](https://rfc1149.net/) about [the difference between ttyUSB and ttyACM](https://rfc1149.net/blog/2013/03/05/what-is-the-difference-between-devttyusbx-and-devttyacmx/).

The main point of this example (outside of me documenting my own things) is to show you how to setup OctoPrint with multiple printers via the USB ports.

## Running the stack

You can bring up the stack with

```
$ docker-compose up -d .
```

As long as you have two printers plugged in to any of the USB ports, you'll have two instances of OctoPrint at these addresses - my printers as examples:

* **FLSun Super Racer** - port 1980 
* **Wanhao Duplicator i3** - port 1981

You can wire up Octofarm to this container to bind to port 80 - coming soon.
