# ft232r_prog

The `ft232r_prog` (v1.25) program provides a Linux command-line interface for reconfiguring the FT232R chip, eliminating the need for FTDI's MProg/FTProg (MS-Windows) packages. 

This is copy of the code by Marc Lord, http://rtr.ca/ft232r/

The FTDI FT232R is a USB/serial converter chip, compatible with both 3.3V and 5.0V TTL logic, but can also be used as General Purpose I/O (GPIO) device. The code hosted here is primarily in support of the latter: a simple way to control real-world stuff from a Linux box, using the CBUS GPIO capabilities of the chip.

All of this stuff requires the [libftdi](http://www.intra2net.com/en/developer/libftdi/) library, which provides basic interfaces for programming FTDI chips.
It also requires the [libusb](http://www.libusb.org/) library, for driver-free installation and use of USB devices on Linux.

Once those two prerequisites are in place, the rest of the code here becomes usable.

## Build
```bash
sudo apt-get install build-essential gcc make libftdi-dev libusb-dev
cd ft232r_prog
make
```

## Usage
```bash
❯ ./ft232r_prog --help

ft232r_prog: version 1.25, by Mark Lord.

Usage:  ft232r_prog [<arg> <val>]..

where <arg> must be any of:
    --help     # (show this help text)
    --dump     # (dump eeprom settings to stdout))
    --verbose  # (show debug info and raw eeprom contents)
    --save     # (save original eeprom contents to file)
    --restore  # (restore initial eeprom contents from file)
    --cbus0  [TxDEN|PwrEn|RxLED|TxLED|TxRxLED|Sleep|Clk48|Clk24|Clk12|Clk6|IO|WR|RD|RxF]
    --cbus1  [TxDEN|PwrEn|RxLED|TxLED|TxRxLED|Sleep|Clk48|Clk24|Clk12|Clk6|IO|WR|RD|RxF]
    --cbus2  [TxDEN|PwrEn|RxLED|TxLED|TxRxLED|Sleep|Clk48|Clk24|Clk12|Clk6|IO|WR|RD|RxF]
    --cbus3  [TxDEN|PwrEn|RxLED|TxLED|TxRxLED|Sleep|Clk48|Clk24|Clk12|Clk6|IO|WR|RD|RxF]
    --cbus4  [TxDEN|PwrEn|RxLED|TxLED|TxRxLED|Sleep|Clk48|Clk24|Clk12|Clk6|IO|WR|RD|RxF]
    --manufacturer       <string>  # (new USB manufacturer string)
    --product            <string>  # (new USB product name string)
    --old-serial-number  <string>  # (current serial number of device to be reprogrammed)
    --new-serial-number  <string>  # (new USB serial number string)
    --self-powered       [on|off]  # (self powered)
    --max-bus-power      <number>  # (max bus current in milli-amperes)
    --high-current-io    [on|off]  # (enable high [6mA @ 5V] drive current on CBUS pins)
    --suspend-pull-down  [on|off]  # (force I/O pins into logic low state on suspend)
    --old-vid    <number>  # (current vendor id of device to be reprogrammed, eg. 0x0403)
    --old-pid    <number>  # (current product id of device to be reprogrammed, eg. 0x6001)
    --new-vid    <number>  # (new/custom vendor id to be programmed)
    --new-pid    <number>  # (new/custom product id be programmed)
    --invert_txd   Inverts the current value of TXD
    --invert_rxd   Inverts the current value of RXD
    --invert_rts   Inverts the current value of RTS
    --invert_cts   Inverts the current value of CTS
    --invert_dtr   Inverts the current value of DTR
    --invert_dsr   Inverts the current value of DSR
    --invert_dcd   Inverts the current value of DCD
    --invert_ri    Inverts the current value of RI
```

## Examples
Reprogram a FT232 Serial -> USB cable, e.g. P1 Smart Meter Cable, to invert the RXD values:
```bash
sudo ./ft232r_prog --save backup.eeprom
sudo ./ft232r_prog --invert_rxd
```

## See also
* https://github.com/richardeoin/ftx-prog
