Unlock the code protection of the ST Link clone that will become your new Black Magic Probe

  openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg -c "init" -c "halt" -c "stm32f1x unlock 0" -c "shutdown"
Expected result

  Open On-Chip Debugger 0.10.0-dev-gdfc6658 (2016-02-16-19:23)
  Licensed under GNU GPL v2
  For bug reports, read
       http://openocd.org/doc/doxygen/bugs.html
  Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
  Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
  adapter speed: 1000 kHz
  adapter_nsrst_delay: 100
  none separate
  Info : Unable to match requested speed 1000 kHz, using 950 kHz
  Info : Unable to match requested speed 1000 kHz, using 950 kHz
  Info : clock speed 950 kHz
  Info : STLINK v2 JTAG v17 API v2 SWIM v4 VID 0x0483 PID 0x3748
  Info : using stlink api v2
  Info : Target voltage: 3.214708
  Info : stm32f1x.cpu: hardware has 6 breakpoints, 4 watchpoints
  stm32f1x.cpu: target state: halted
  target halted due to debug-request, current mode: Thread
  xPSR: 0x81000000 pc: 0x080047a2 msp: 0x20004f08
  Info : device id = 0x20036410
Now, really power cycle the ST Link clone by unplug it from the USB port and replug it.

Erase the ST Link Clone

  st-flash erase
Expected result

  2016-08-11T20:02:04 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Loading device parameters....
  2016-08-11T20:02:04 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Device connected is: F1 Medium-density device, id 0x20036410
  2016-08-11T20:02:04 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: SRAM size: 0x5000 bytes (20 KiB), Flash: 0x20000 bytes (128 KiB) in pages of 1024 bytes
  Mass erasing
Write the DFU Bootloader into the ST Link Clone at the beginning of flash

  st-flash write blackmagic_dfu.bin 0x8000000
Or do the same using OpenOCD:

  openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg -c "init" -c "halt" -c "flash write_image erase blackmagic_dfu.bin 0x8000000" -c "shutdown"
Expected result (with st-flash):

  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Loading device parameters....
  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Device connected is: F1 Medium-density device, id 0x20036410
  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: SRAM size: 0x5000 bytes (20 KiB), Flash: 0x20000 bytes (128 KiB) in pages of 1024 bytes
  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Attempting to write 6740 (0x1a54) bytes to stm32 address: 134217728 (0x8000000)
  Flash page at addr: 0x08001800 erased
  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Finished erasing 7 pages of 1024 (0x400) bytes
  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Starting Flash write for VL/F0/F3 core id
  2016-08-11T20:03:43 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Successfully loaded flash loader in sram
    6/6 pages written
  2016-08-11T20:03:44 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Starting verification of write complete
  2016-08-11T20:03:44 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Flash written and verified! jolly good!
Now flash the Black Magic Probe firmware

  st-flash --reset write blackmagic.bin 0x8002000
Expected result:

  2016-08-11T20:05:26 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Loading device parameters....
  2016-08-11T20:05:26 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Device connected is: F1 Medium-density device, id 0x20036410
  2016-08-11T20:05:26 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: SRAM size: 0x5000 bytes (20 KiB), Flash: 0x20000 bytes (128 KiB) in pages of 1024 bytes
  2016-08-11T20:05:26 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Attempting to write 54788 (0xd604) bytes to stm32 address: 134225920 (0x8002000)
  Flash page at addr: 0x0800f400 erased
  2016-08-11T20:05:28 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Finished erasing 54 pages of 1024 (0x400) bytes
  2016-08-11T20:05:28 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Starting Flash write for VL/F0/F3 core id
  2016-08-11T20:05:28 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Successfully loaded flash loader in sram
   53/53 pages written
  2016-08-11T20:05:31 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Starting verification of write complete
  2016-08-11T20:05:32 INFO /tmp/stlink-20160608-18806-p6zk82/stlink-1.2.0/src/stlink-common.c: Flash written and verified! jolly good!
Now disconnect the programming wires and the first ST Link Clone from USB. Only plug your newly flashed Black Magic Probe Clone into any USB port. The first ST Link Clone is untouched and remains usable.

Use the lsusb to verify that the device enumerated at the USB.

  system_profiler SPUSBDataType
Expected result:

                   Black Magic Probe (STLINK), (Firmware v1.6-rc0-213-gdf7ad91):
                     Product ID: 0x6018
                     Vendor ID: 0x1d50
                     Version: 1.00
                     Serial Number: D6CE8AB1
                     Speed: Up to 12 Mb/sec
                     Manufacturer: Black Sphere Technologies
                     Location ID: 0x1a121100 / 5
                     Current Available (mA): 1000
                     Current Required (mA): 100
                     Extra Operating Current (mA): 0
The Black Magic Probe Clone has a DFU bootloader which may be used to updated the Black Magic Probe Firmware. I did not test it but verified it is there:

  dfu-util -l
Expected result is:

  dfu-util 0.9
  Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
  Copyright 2010-2016 Tormod Volden and Stefan Schmidt
  This program is Free Software and has ABSOLUTELY NO WARRANTY
  Please report bugs to http://sourceforge.net/p/dfu-util/tickets/
  Deducing device DFU version from functional descriptor length
  Found Runtime: [05ac:828c] ver=0151, devnum=3, cfg=1, intf=3, path="29-3", alt=0, name="UNKNOWN", serial="UNKNOWN"
  Found Runtime: [1d50:6018] ver=0100, devnum=5, cfg=1, intf=4, path="26-1", alt=0, name="Black Magic Firmware Upgrade (STLINK)", serial="D6CE8AB1"

