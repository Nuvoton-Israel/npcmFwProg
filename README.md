# NpcmFwProg

## The tool only work when internal flash(mcp) data empty or verfiy failed.  

### [Windows Nuvoton Flash Program Tool Usage]  
  
Options:  
```text
Usage: NpcmFwProg.exe [-c console-number] [-wre flash-type] [-a address] [-l length] [-i program-file] [-o output-file] [-bn]

-c, --comport  [#]                    : host console port number.  
-w, --prog     [flash-type]           : program [flash-type] data.        (must assign --file)  
-r, --read     [flash-type]           : read [flash-type] data.  
-e, --erase    [flash-type]           : erase [flash-type] data.  
-a, --addr     [hex-value]            : prog/read/erase address.   (prog or erase must align 4K size)  
-l, --len      [hex-value]            : prog/read/erase length.    (prog or erase must align 4K size)  
-i, --in       [file-name]            : write file data to [flash-type].  (only available with --prog option)  
-o, --out      [file-name]            : read [flash-type] data to file.   (only available with --read option)  
-b, --reboot                          : reboot npcm400f system.  
-n, --no-verify                       : no need verify flash program data.  
-v, --version                         : show NpcmFwProg version.  
-h, --help                            : this usage.  

[flash-type]
mcp              = NCPM400F internal flash (use for NPCM400F firmware image)  
shd              = NPCM400F external flash (may use for bios or recovery data, depend on use case)  
pvt              = NPCM400F external flash (may use for private data, depend on use case)  
backup           = NPCM400F external flash (may use for backup data, depend on use case)  
```
  
Program: 
```sh 
NpcmFwProg --comport 9 --prog mcp --in zephyr-signed.bin  
NpcmFwProg --comport 9 --prog mcp --in zephyr-signed.bin --reboot  
NpcmFwProg --comport 9 --prog shd --in zephyr-signed.bin  
NpcmFwProg --comport 9 --prog mcp --in zephyr-signed.bin --addr 0x1000 --len 0x6000  
NpcmFwProg -c 9 -w shd -i bios.bin -n  
```
Read:  
```sh
NpcmFwProg --comport 9 --read mcp --addr 0x0 --len 0x1000  
NpcmFwProg --comport 9 --read shd --addr 0x500 --len 0x800 --out dump.bin  
NpcmFwProg -c 9 -r mcp -a 0xa00 -l 0x80 -o flash-dump.bin  
```
Erase:  
```sh
NpcmFwProg --comport 9 --erase mcp  
NpcmFwProg --comport 9 --erase shd --addr 0x1000 --len 0x8000  
NpcmFwProg -c 9 -e pvt -a 0x8000 -l 0x4000  
```

---------------------------------------------------------------------------------------------  

### [Linux Nuvoton Flash Program Tool Usage]  
 
#### Need libc.so.6 and root privileges  

Options:  
```text  
Usage: NpcmFwProg.linux [-c console-device] [-wre flash-type] [-a address] [-l length] [-i program-file] [-o output-file] [-bn]  
  
-c [console path]               select console. ex: /dev/ttyS0  

-w [flash-type]                 program [flash-type]. ex: mcp, shd, pvt or backup  
-r [flash-type]                 read [flash-type]. ex: mcp, shd, pvt or backup  
-e [flash-type]                 erase [flash-type]. ex: mcp, shd, pvt or backup  
-a [address]                    assign address that use for program, read and erase [flash-type].  
-l [length]                     assign length that use for program, read and erase [flash-type].  
-i [input file path]            input file that use to program [flash-type].  
-o [output file path]           output file that read from [flash-type] data.  
-b                              reboot npcm400f system.  
-n                              no need verify program data.  
-v                              show NpcmFwProg.linux version.  
-h                              this usage.  
  
[flash-type]  
mcp              = NCPM400F internal flash (use for NPCM400F firmware image)  
shd              = NPCM400F external flash (may use for bios or recovery data, depend on use case)  
pvt              = NPCM400F external flash (may use for private data, depend on use case)  
backup           = NPCM400F external flash (may use for backup data, depend on use case)  
```   
  
Program:
```sh  
sudo ./NpcmFwProg.linux -c /dev/ttyS2 -w mcp -i npcm-signed.bin  
sudo ./NpcmFwProg.linux -c /dev/ttyS10 -w pvt -i npcm-signed.bin -n  
sudo ./NpcmFwProg.linux -c /dev/ttyS8 -w shd -i bios-signed.bin -b  
```
Read:
```sh  
sudo ./NpcmFwProg.linux -c /dev/ttyS0 -r pvt -a 0x100 -l 0x500  
sudo ./NpcmFwProg.linux -c /dev/ttyUSB0 -r shd -a 0x100 -l 0x500 -o /usr/dump-flash  
```
Erase:  
```sh
sudo ./NpcmFwProg.linux -c /dev/ttyACM0 -e backup  
sudo ./NpcmFwProg.linux -c /dev/ttyUSB1 -e backup -a 0x2000 -l 0x5000  
```
