```
ICX64XX-boot>>
ICX64XX-boot>> show config
Unknown command 'show' - try 'help'
ICX64XX-boot>> help
?       - alias for 'help'
boot    - boot default, i.e., run 'bootcmd'
boot_primary   - primary boot; boot from primary partition
boot_secondary   - secondary boot; boot from secondary partition
cp      - memory copy
eeprom - EEPROM dump or program command
help    - print online help
i2cprobe - Get special i2c device id
md      - memory display
memtest - To perform DDR memory test
pci     - list and access PCI Configuration Space
ping    - send ICMP ECHO_REQUEST to network host
printenv- print environment variables
reset   - Perform RESET of the CPU
saveenv - save environment variables to persistent storage
setenv  - set environment variables
sflash  - read, write or erase the external SPI Flash.
tftpboot- boot image via network using TFTP protocol
update_primary   - primary update; update primary partition
update_secondary   - secondary update; update secondary partition
update_uboot - get the uboot image over tftp.
version - print monitor version
ICX64XX-boot>> version

10.1.05T310 (Mar 19 2015 - 16:39:59)
 (Mar 19 2015 - 16:40:09)
CPLD version 0.0.1
PoE Software Version: N/A
ICX64XX-boot>> memtest
Unsupport this test rule
ICX64XX-boot>> md
Usage:
md      - memory display

ICX64XX-boot>> boot
Booting image from Primary
   Bad Magic Number
could not boot from primary, no valid image; trying to boot from
Booting image from Secondary
   Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
could not boot from secondary, no valid image; trying to boot fro
Booting image from Primary
   Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
ICX64XX-boot>> show flash
Unknown command 'show' - try 'help'
ICX64XX-boot>>[3~bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
r[3[3[3[3[3[3<INTERRUPT>bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbICX64XX-boot>> bbbbbbbbbb
ICX64XX-boot>> boot_primary
Booting image from Primary
   Bad Magic Number
could not boot from primary, no valid image; trying to boot from
Booting image from Secondary
   Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
could not boot from secondary, no valid image; trying to boot fro
Booting image from Primary
   Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
ICX64XX-boot>> show flash
Unknown command 'show' - try 'help'
ICX64XX-boot>> flash
Unknown command 'flash' - try 'help'
ICX64XX-boot>> stop
Unknown command 'stop' - try 'help'
ICX64XX-boot>> shutdown
Unknown command 'shutdown' - try 'help'
ICX64XX-boot>> halt
Unknown command 'halt' - try 'help'
ICX64XX-boot>> power off
Unknown command 'power' - try 'help'
ICX64XX-boot>> exit
Unknown command 'exit' - try 'help'
ICX64XX-boot>> help
?       - alias for 'help'
boot    - boot default, i.e., run 'bootcmd'
boot_primary   - primary boot; boot from primary partition
boot_secondary   - secondary boot; boot from secondary partition
cp      - memory copy
eeprom - EEPROM dump or program command
help    - print online help
i2cprobe - Get special i2c device id
md      - memory display
memtest - To perform DDR memory test
pci     - list and access PCI Configuration Space
ping    - send ICMP ECHO_REQUEST to network host
printenv- print environment variables
reset   - Perform RESET of the CPU
saveenv - save environment variables to persistent storage
setenv  - set environment variables
sflash  - read, write or erase the external SPI Flash.
tftpboot- boot image via network using TFTP protocol
update_primary   - primary update; update primary partition
update_secondary   - secondary update; update secondary partition
update_uboot - get the uboot image over tftp.
version - print monitor version
ICX64XX-boot>> login
Unknown command 'login' - try 'help'
ICX64XX-boot>> admin
Unknown command 'admin' - try 'help'
ICX64XX-boot>> mode
Unknown command 'mode' - try 'help'
ICX64XX-boot>> md
Usage:
md      - memory display

ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> ping 10.45.1.1
egiga0 no link
*** ERROR: `ipaddr' not set
ping failed; host 10.45.1.1 is not alive
ICX64XX-boot>> show ver
Unknown command 'show' - try 'help'
ICX64XX-boot>> set
ICX64XX-boot>> printennnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnnRUPT>
ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> setenv ipaddr
ICX64XX-boot>> printenv
baudrate=9600
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> set ipaddr 10.45.1.2 255.255.255.0
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> p[p
Unknown command 'p[p' - try 'help'
ICX64XX-boot>>
ICX64XX-boot>> ping 10.45.1.1
Link Status Changed, Re-Negotiation Start
Using egiga0 device
host 10.45.1.1 is alive
ICX64XX-boot>> p
Unknown command 'p' - try 'help'
ICX64XX-boot>>
ICX64XX-boot>> print env
## Error: "env" not defined
ICX64XX-boot>> print ipaddr
ipaddr=10.45.1.2 255.255.255.0
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> memtest
Unsupport this test rule
ICX64XX-boot>> ?
?       - alias for 'help'
boot    - boot default, i.e., run 'bootcmd'
boot_primary   - primary boot; boot from primary partition
boot_secondary   - secondary boot; boot from secondary partition
cp      - memory copy
eeprom - EEPROM dump or program command
help    - print online help
i2cprobe - Get special i2c device id
md      - memory display
memtest - To perform DDR memory test
pci     - list and access PCI Configuration Space
ping    - send ICMP ECHO_REQUEST to network host
printenv- print environment variables
reset   - Perform RESET of the CPU
saveenv - save environment variables to persistent storage
setenv  - set environment variables
sflash  - read, write or erase the external SPI Flash.
tftpboot- boot image via network using TFTP protocol
update_primary   - primary update; update primary partition
update_secondary   - secondary update; update secondary partition
update_uboot - get the uboot image over tftp.
version - print monitor version
ICX64XX-boot>> saveenv
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> ping 10.45.1.244
Using egiga0 device
host 10.45.1.244 is alive
ICX64XX-boot>> read sflash
Unknown command 'read' - try 'help'
ICX64XX-boot>> sflash read
Usage:
sflash  - read, write or erase the external SPI Flash.

ICX64XX-boot>> sflash
Usage:
sflash  - read, write or erase the external SPI Flash.

ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> ls
Unknown command 'ls' - try 'help'
ICX64XX-boot>> whoami
Unknown command 'whoami' - try 'help'
ICX64XX-boot>> pwd
Unknown command 'pwd' - try 'help'
ICX64XX-boot>> dir
Unknown command 'dir' - try 'help'
ICX64XX-boot>> ?
?       - alias for 'help'
boot    - boot default, i.e., run 'bootcmd'
boot_primary   - primary boot; boot from primary partition
boot_secondary   - secondary boot; boot from secondary partition
cp      - memory copy
eeprom - EEPROM dump or program command
help    - print online help
i2cprobe - Get special i2c device id
md      - memory display
memtest - To perform DDR memory test
pci     - list and access PCI Configuration Space
ping    - send ICMP ECHO_REQUEST to network host
printenv- print environment variables
reset   - Perform RESET of the CPU
saveenv - save environment variables to persistent storage
setenv  - set environment variables
sflash  - read, write or erase the external SPI Flash.
tftpboot- boot image via network using TFTP protocol
update_primary   - primary update; update primary partition
update_secondary   - secondary update; update secondary partition
update_uboot - get the uboot image over tftp.
version - print monitor version
ICX64XX-boot>> help
?       - alias for 'help'
boot    - boot default, i.e., run 'bootcmd'
boot_primary   - primary boot; boot from primary partition
boot_secondary   - secondary boot; boot from secondary partition
cp      - memory copy
eeprom - EEPROM dump or program command
help    - print online help
i2cprobe - Get special i2c device id
md      - memory display
memtest - To perform DDR memory test
pci     - list and access PCI Configuration Space
ping    - send ICMP ECHO_REQUEST to network host
printenv- print environment variables
reset   - Perform RESET of the CPU
saveenv - save environment variables to persistent storage
setenv  - set environment variables
sflash  - read, write or erase the external SPI Flash.
tftpboot- boot image via network using TFTP protocol
update_primary   - primary update; update primary partition
update_secondary   - secondary update; update secondary partition
update_uboot - get the uboot image over tftp.
version - print monitor version
ICX64XX-boot>> print help
## Error: "help" not defined
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> i2cprobe
I2C has probe the PD69000.(Reg0=0x03)
I2C has probe the EEPROM.(Reg0=0x0000)
Read device SFP Port 2.0 fail
Read device SFP Port 1.0 fail
I2C has probe the CPLD Interrupt Mask Control.(Reg3=0xff)
I2C has probe the PCA9535 SFP.(Reg0=0x00)
I2C has probe the PCA9535 LED.(Reg0=0xff)
I2C has probe the Hardware Monitor.(Regfd=0x50)
loop: 1
i2cprobe FAIL
ICX64XX-boot>> reset
Reset CPU by asserting RstOutn

Bootloader Version: 10.1.05T310 (Mar 19 2015 - 16:39:59)


Model ID: 0.0.0.0.0.1

Enter 'b' to stop at boot monitor:  0
bootdelay: ===
Booting image from Primary
   Bad Magic Number
could not boot from primary, no valid image; trying to boot from
Booting image from Secondary
   Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
could not boot from secondary, no valid image; trying to boot fro
Booting image from Primary
   Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> printenv
ICX64XX-boot>> print env
## Error: "env" not defined
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2 255.255.255.0
uboot=kxz10105
image_name=ICX64S08010m.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>>
```
