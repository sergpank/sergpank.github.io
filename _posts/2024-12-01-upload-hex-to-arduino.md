---
layout: post
title:  "How to uplad binary file to Arduboy"
date:   2024-12-01 20:00:01 +0200
categories: Arduboy
---
# How to uplad binary file to Arduboy

It is very very simple.

1. Open Arduino IDE 
2. Go to File -> Preferences
3. Enable -> Show verbose output during: [] upload
4. Upload some sample sketch into Arduboy
5. Find and copy upload command from Arduino terminal
6. Change path to binary file in this command and run it from terminal

Notes:
*. You need to close Arduino IDE to release the COM port
*. You may need to restart Arduboy before executing upload command
*. You may need to start upload command right after reset button is pressed

In my case there was a following command:
```bash
"/Users/spanko/Library/Arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/bin/avrdude" \
"-C/Users/spanko/Library/Arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/etc/avrdude.conf" \
-v \
-V \
-patmega32u4 \
-cavr109
"-P/dev/cu.usbmodem101" 
-b57600 \
-D
"-Uflash:w:/private/var/folders/lw/g6b_rzbd1616b86pn1547p9w0000gn/T/arduino/sketches/EB6F26627ADC17C2831534A5E05EC8DB/barcode-boy.ino.hex:i"
```

I've has to replace path to hex.file after `-Uflash` flag to `"-Uflash:w:/Users/spanko/Arduboy/Shadow-Runner/Shadow Runner.hex"`  
And hit `Enter`:  
```bash
$ "/Users/spanko/Library/Arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/bin/avrdude" \
"-C/Users/spanko/Library/Arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/etc/avrdude.conf" \
-v \
-V \
-patmega32u4 \
-cavr109 \
"-P/dev/cu.usbmodem101" \
-b57600 \
-D \
"-Uflash:w:/Users/spanko/Arduboy/Shadow-Runner/Shadow Runner.hex"

avrdude: Version 6.3-20190619
         Copyright (c) 2000-2005 Brian Dean, http://www.bdmicro.com/
         Copyright (c) 2007-2014 Joerg Wunsch

         System wide configuration file is "/Users/spanko/Library/Arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/etc/avrdude.conf"
         User configuration file is "/Users/spanko/.avrduderc"
         User configuration file does not exist or is not a regular file, skipping

         Using Port                    : /dev/cu.usbmodem101
         Using Programmer              : avr109
         Overriding Baud Rate          : 57600
         AVR Part                      : ATmega32U4
         Chip Erase delay              : 9000 us
         PAGEL                         : PD7
         BS2                           : PA0
         RESET disposition             : dedicated
         RETRY pulse                   : SCK
         serial program mode           : yes
         parallel program mode         : yes
         Timeout                       : 200
         StabDelay                     : 100
         CmdexeDelay                   : 25
         SyncLoops                     : 32
         ByteDelay                     : 0
         PollIndex                     : 3
         PollValue                     : 0x53
         Memory Detail                 :

                                  Block Poll               Page                       Polled
           Memory Type Mode Delay Size  Indx Paged  Size   Size #Pages MinW  MaxW   ReadBack
           ----------- ---- ----- ----- ---- ------ ------ ---- ------ ----- ----- ---------
           eeprom        65    20     4    0 no       1024    4      0  9000  9000 0x00 0x00
           flash         65     6   128    0 yes     32768  128    256  4500  4500 0x00 0x00
           lfuse          0     0     0    0 no          1    0      0  9000  9000 0x00 0x00
           hfuse          0     0     0    0 no          1    0      0  9000  9000 0x00 0x00
           efuse          0     0     0    0 no          1    0      0  9000  9000 0x00 0x00
           lock           0     0     0    0 no          1    0      0  9000  9000 0x00 0x00
           calibration    0     0     0    0 no          1    0      0     0     0 0x00 0x00
           signature      0     0     0    0 no          3    0      0     0     0 0x00 0x00

         Programmer Type : butterfly
         Description     : Atmel AppNote AVR109 Boot Loader

Connecting to programmer: .
Found programmer: Id = "CATERIN"; type = S
    Software Version = 1.0; No Hardware Version given.
Programmer supports auto addr increment.
Programmer supports buffered memory access with buffersize=128 bytes.

Programmer supports the following devices:
    Device code: 0x44

avrdude: devcode selected: 0x44
avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9587 (probably m32u4)
avrdude: safemode: lfuse reads as FF
avrdude: safemode: hfuse reads as D8
avrdude: safemode: efuse reads as CB
avrdude: reading input file "/Users/spanko/Arduboy/Shadow-Runner/Shadow Runner.hex"
avrdude: input file /Users/spanko/Arduboy/Shadow-Runner/Shadow Runner.hex auto detected as Intel Hex
avrdude: writing flash (22474 bytes):

Writing | ################################################## | 100% 1.69s

avrdude: 22474 bytes of flash written

avrdude: safemode: lfuse reads as FF
avrdude: safemode: hfuse reads as D8
avrdude: safemode: efuse reads as CB
avrdude: safemode: Fuses OK (E:CB, H:D8, L:FF)

avrdude done.  Thank you.
```

That's all folks!  
Thank a lot for advice to Arduino forum : <https://forum.arduino.cc/t/the-simplest-way-to-upload-a-compiled-hex-file-without-the-ide/401996/>
