SpaceMonitor bootloader
=======================

This source is based on the excellent work of Peter Fleury. The original can be
found at http://homepage.hispeed.ch/peterfleury/avr-software.html

This version of the bootloader is in essence the same, it has been tested on an
ATMega88 at a speed of 19200 baud. Avrdude expects the stk500 to work at 115200
baud so you do have to override the speed on the commandline (-b 19200). The
major differences are;

* The bootloader is always started.
* Upon startup it will read the first 6 bytes of eeprom and send a message(1)
over UART indicating that it is active.
* After two seconds of inactivity it wil time-out, send another message and
jump to 0x0000.
* After it has sent the active message it will behave like a regular stk500
bootloader at a speed of 19200 baud.

(1) The message format used is based on the format already in use at Hack42.
It consist of an ascii start character, message size (16 bit), hexadecimal id,
forward slash, message, ascii end character, checksum, newline.

0x02<size><hexadecimal id>/<message>0x03<crc>

Bootloader start message;
0x02 0x00 0x11 "A1B2C3D4E5F6/OLA!" 0x03 0xfd

Boorloader end message;
0x02 0x00 0x11 "A1B2C3D4E5F6/BYE!" 0x03 0x81

