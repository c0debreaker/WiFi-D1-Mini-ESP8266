# WiFi-D1-Mini-ESP8266 PRO USB-TTL CP2104
WiFi D1 mini PRO is an integrated ESP8266-based WiFi-enabled microprocessor unit equipped with 32Mb (megabits) of flash memory. It is pin-compatible with the WeMos D1 mini / mini PRO boards.

WiFi D1 mini PRO comes with an embedded CERAMIC WiFi antenna on-board, and has a connector for an optional external antenna.

The input power can vary from 4 to 9V DC (supplied to the «5V» pin). We recommend to supply exactly 5V in case of using external shields requiring 5V operation. The unit can also be powered directly from the USB port. Alternatively, you can power the WiFi D1 mini PRO from a stabilized 3.3V source by connecting it to the «3.3V» pin on the PCB.

PC communication is done via CP2104 (USB-TTL). Corresponding driver may need to be installed on your system. In order to use WiFi D1 mini PRO with an Arduino IDE, please use the ESP8266 library.

More info: https://robotdyn.com/wifi-d1-mini-esp8266-dev-board-usb-cp2104.html

This document contain instructions for getting WiFi D1 Mini ESP8266 PRO to work on a Mac OSX using CP210x USB to UART Bridge VCP Drivers.

## Before installing the drivers
Before installing the driver, verify if the WiFi D1 Mini is being detected by the system by connecting it to your laptop via one of your usb ports and running `system_preferences SPUSBDataType`. The line to expect should be similar to `CP2104 USB to UART Bridge Controller`

Here's the complete output of the `system_profiler` command.

```sh
$ system_profiler SPUSBDataType
USB:

    USB 3.0 Bus:

      Host Controller Driver: AppleUSBXHCISPT
      PCI Device ID: 0xa12f
      PCI Revision ID: 0x0031
      PCI Vendor ID: 0x8086

        CP2104 USB to UART Bridge Controller:

          Product ID: 0xea60
          Vendor ID: 0x10c4  (Silicon Laboratories, Inc.)
          Version: 1.00
          Serial Number: 01767C12
          Speed: Up to 12 Mb/sec
          Manufacturer: Silicon Labs
          Location ID: 0x14600000 / 18
          Current Available (mA): 500
          Current Required (mA): 100
          Extra Operating Current (mA): 0

        iBridge:

          Product ID: 0x8600
          Vendor ID: 0x05ac (Apple Inc.)
          Version: 1.01
          Manufacturer: Apple Inc.
          Location ID: 0x14200000

    USB 3.1 Bus:

      Host Controller Driver: AppleUSBXHCIAR
      PCI Device ID: 0x15d4
      PCI Revision ID: 0x0002
      PCI Vendor ID: 0x8086
      Bus Number: 0x01

    USB 3.1 Bus:

      Host Controller Driver: AppleUSBXHCIAR
      PCI Device ID: 0x15d4
      PCI Revision ID: 0x0002
      PCI Vendor ID: 0x8086
      Bus Number: 0x00
```

## Installing the Mac OSX driver
Now that you've confirmed that your WiFi D1 Mini device is detected, disconnect it before continuing.

You can download drivers specific to your operating system from this link.
https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

For Mac OSX, the link is https://www.silabs.com/documents/public/software/Mac_OSX_VCP_Driver.zip

### Warning
There are times installation of driver fails. This happens when the OSX kernel blocks it. To verify, go to System Preferences -> Security & Privacy -> General. If you see an application listed as being blocked, allow it by clicking the `Allow` button.

### Tip(s) May or may not be needed
I've been troubleshooting this for the past few days and I ran out of ideas until someone told me to install CP210x driver. It still failed for me. However, the same driver worked when I unloaded `/Library/Extensions/usbserial.kext` and `/System/Library/Extensions/usb.kext`. It may not work on some systems since they may not be installed.

```sh
$ sudo kextunload /Library/Extensions/usbserial.kext
$ sudo kextunload /System/Library/Extensions/usb.kext
```

## USB to UART port validation
Connect back the WiFi D1 mini device via usb. You should see a new serial device added. The new device is `/dev/cu.SLAB_USBtoUART`

```sh
crw-rw-rw-  1 root  wheel   21,   3 Jul  3 19:41 /dev/cu.MALS
crw-rw-rw-  1 root  wheel   21,  11 Jul  3 19:41 /dev/cu.M-C02PXF4QG8WP-Bluetoot
crw-rw-rw-  1 root  wheel   21,   5 Jul  3 19:41 /dev/cu.JamReplay-SerialPort-3
crw-rw-rw-  1 root  wheel   21,   7 Jul  3 19:41 /dev/cu.JamReplay-SerialPort-1
crw-rw-rw-  1 root  wheel   21,   9 Jul  3 19:41 /dev/cu.Bluetooth-Incoming-Port
crw-rw-rw-  1 root  wheel   21,   1 Jul  5 19:17 /dev/cu.SOC
crw-rw-rw-  1 root  wheel   21,  23 Jul  6 18:41 /dev/cu.SLAB_USBtoUART
```

## Testing using Arduino
Before you can use your WiFi D1 Mini device, you'll need to add a list of unsupported boards in Arduino IDE's Preferences. You need to add `http://arduino.esp8266.com/stable/package_esp8266com_index.json` in Preferences. After making changes in Preferences, you need to search ESP8266 under Board Manager in the Arduino IDE and install it.

You're almost ready in uploading a sample sketch. However, before you can upload a new code, you need to select "Generic ESP8266 Module" in Tools -> Board. Set the Upload Speed to 115200. I was shocked that it failed when I chose 9600. I've always used 9600 befored. Lastly, select /dev/cu.SLAB_USBtoUART in Ports.

Now that's everything is setup, let's upload an example sketch called Blink. Go to File -> Examples -> ESP8266 -> Blink.

You should see an output of the upload task similar below. You can change the output by turning off or unchecking `compilation` in Preferences.

```sh
    fa27109954bc04ac 72accec85c8cf359 | .'..T...r...\..Y
    094a5ee6ee830425 a81e91fe1450b8ea | .J^....%.....P..
    5590804a756ff881 efeedbdce0d0c8e8 | U..Juo..........
    c3b1713f04c3f7ee 3f18bc3bf060d283 | ..q?....?..;.`..
    9f687c28caf05d14 36e1fd4f1edcc565 | .h|(..].6..O...e
    af381cabcaf83b9a 68eed2b282956b1f | .8....;.h.....k.
    7f5d57b03650c789 f9abaec6bff00de4 | .]W.6P..........
    6a91ab416e2b722e e49cc86d466e1372 | j..An+r....mFn.r
    3f476e030e231500 ff2468e252207712 | ?Gn..#...$h.R w.
    4d5ed1f8dea66eff c222e45e44ae18b9 | M^....n..".^D...
    201115a9b76de0ee 661689f9bfa4e257 |  ....m..f......W
    56161403d005dbdc c3519466ffbb348b | V........Q.f..4.
    ef79b20cf1cffd0a 0d6bfb06f5159986 | .y.......k......
    c577409d936bb87b 845268fe793952c3 | .w@..k.{.Rh.y9R.
    c53bf701cd1241d1 a2d9813a700a7c1a | .;....A....:p.|.
    9aed400edf4971b5 7b725d6bfe3ba2fd | ..@..Iq.{r]k.;..
    bd77c0                            | .w.
TRACE +1.021 Read 1 bytes: c0
TRACE +0.000 Read 8 bytes: 0111020000000000
TRACE +0.001 Read 1 bytes: 00
TRACE +0.000 Read 2 bytes: 00c0
TRACE +0.000 Received full packet: 01110200000000000000

Wrote 261840 bytes (191244 compressed) at 0x00000000 in 18.8 seconds (effective 111.2 kbit/s)...
TRACE +0.000 command op=0x13 data len=16 wait_response=1 timeout=3.000 data=00000000d0fe03000000000000000000
TRACE +0.000 Write 26 bytes: 
    c000131000000000 0000000000d0fe03 | ................
    0000000000000000 00c0             | ..........
TRACE +0.107 Read 1 bytes: c0
TRACE +0.000 Read 8 bytes: 0113120000000000
TRACE +0.309 Read 1 bytes: 8a
TRACE +0.000 Read 18 bytes: 
    5248dd3e216b8b27 18919bd1e9b0e600 | RH.>!k.'........
    00c0                              | ..
TRACE +0.000 Received full packet: 
    0113120000000000 8a5248dd3e216b8b | .........RH.>!k.
    2718919bd1e9b0e6 0000             | '.........
Hash of data verified.

Leaving...
TRACE +0.000 command op=0x02 data len=16 wait_response=1 timeout=3.000 data=00000000000000000040000000000000
TRACE +0.000 Write 26 bytes: 
    c000021000000000 0000000000000000 | ................
    0000400000000000 00c0             | ..@.......
TRACE +0.004 Read 1 bytes: c0
TRACE +0.000 Read 11 bytes: 01020200000000000000c0
TRACE +0.000 Received full packet: 01020200000000000000
TRACE +0.000 command op=0x12 data len=4 wait_response=1 timeout=3.000 data=01000000
TRACE +0.000 Write 14 bytes: c0001204000000000001000000c0
TRACE +0.003 Read 1 bytes: c0
TRACE +0.000 Read 11 bytes: 01120200000000000000c0
TRACE +0.000 Received full packet: 01120200000000000000
Hard resetting via RTS pin...
```

If the upload went well, you should see the led on the device blink on and off.

Congratulations!