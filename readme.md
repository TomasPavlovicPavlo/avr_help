## avrdude help

 - -v verbose output
 - -p *partno*
   - **m2560** ATmega2560
   - **t85** ATtiny85
   - ...
 - -c *programmer-id*, Specify the programmer to be used
    - **usbtiny** USBtiny simple USB programmer, https://learn.adafruit.com/usbtinyisp
    - ...

 - -b *baudrate* Override the RS-232 connection baud rate specified in the respective program-
mer’s entry of the configuration file.    
 - -U *memtype*:*op*:*filename*[:*format*]
   - memtype
     - calibration &emsp; One or more bytes of RC oscillator calibration data.
     - eeprom &emsp; The EEPROM of the device.
     - efuse &emsp; The extended fuse byte.
     - flash &emsp; The flash ROM of the device.
     - fuse &emsp; The fuse byte in devices that have only a single fuse byte.
     - hfuse &emsp; The high fuse byte.
     - lfuse &emsp; The low fuse byte.
     - lock &emsp; The lock byte.
     - signature &emsp; The three device signature bytes (device ID).
     - fuseN &emsp; The fuse bytes of ATxmega devices, N is an integer number for each fuse supported by the device.
     - application &emsp; The application flash area of ATxmega devices.
     - apptable &emsp; The application table flash area of ATxmega devices.
     - boot &emsp; The boot flash area of ATxmega devices.
     - prodsig &emsp; The production signature (calibration) area of ATxmega devices.
     - usersig &emsp; The user signature area of ATxmega devices.
   - The *op* field specifies what operation to perform:
     - r &emsp; read the specified device memory and write to the specified file
     - w &emsp; read the specified file and write it to the specified device memory
     - v &emsp; read the specified device memory and the specified file and perform a verify operation
   - The *filename* field indicates the name of the file to read or write. 
   - The *format* field is optional and contains the format of the file to read or write. Possible values are:
     - i &emsp; Intel Hex
     - I &emsp; Intel Hex with comments on download and tolerance of checksum errors on upload
     - s &emsp; Motorola S-record
     - r &emsp; raw binary; little-endian byte order, in the case of the flash ROM data
     - e &emsp; ELF (Executable and Linkable Format), the final output file fromthe linker; currently only accepted as an input file
     - m &emsp; immediate mode; actual byte values specified on the command line, separated by commas or spaces in place of the filename field of the -U option. This is useful for programming fuse bytes without
having to create a single-byte file or enter terminal mode. If the number specified begins with 0x, it is  reated as a hex value. If the number otherwise begins with a leading zero (0) it is treated as octal. Otherwise, the value is treated as decimal.
     - a &emsp; auto detect; valid for input only, and only if the input is not provided at stdin.
     - d &emsp; decimal; this and the following formats are only valid on output. They generate one line of output for the respective memory section, forming a comma-separated list of the values. This can be particularly useful for subsequent processing, like for fuse bit settings.
     - h &emsp; hexadecimal; each value will get the string 0x prepended. Only valid on output.
     - o &emsp; octal; each value will get a 0 prepended unless it is less than 8 in which case it gets no prefix. Only valid on output.
     - b &emsp; binary; each value will get the string 0b prepended. Only valid on output.
The default is to use auto detection for input files, and raw binary format for
output files.

### examples

#### read flash binary to file "dump.bin"

```
avrdude -v -p m2560 -c usbtiny -Uflash:r:dump.bin:r
```

#### read flash in Intel format to file "dump.hex"

```
avrdude -v -p m2560 -c usbtiny -Uflash:r:dump.hex:i
```

#### read signature (device ID) to console

```
avrdude -v -p m2560 -c usbtiny -Usignature:r:-:h
```

#### read hFuse in hexadecimal to console

```
avrdude -v -p m2560 -c usbtiny -Uhfuse:r:-:h
```

#### read eFuse in binary to console

```
avrdude -v -p m2560 -c usbtiny -Uefuse:r:-:b
```

#### terminal mode

```
avrdude -v -p m2560 -c usbtiny -t
```

## disassemble avr dump file

avr-objdump -j .sec1 -d -m avr6 -l -Sz mega2560_dump.hex > mega2560_dis.asm