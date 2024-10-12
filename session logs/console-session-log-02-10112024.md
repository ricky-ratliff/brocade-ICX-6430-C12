# Console Session Log 02 10/11/2024

## Configuration Changes

- setenv
  - uboot
  - image_name
- update_primary
  - TFTP from server 10.45.1.140; our IP address is 10.45.1.2
Download Filename 'ICX64S08030u.bin'.

```shell
ICX64XX-boot>> setenv uboot /foundry/FGS/bootcode/kxz10105.bin
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2
serverip=10.45.1.140
netmask=255.255.255.0
gatewayip=10.45.1.1
uboot=/foundry/FGS/bootcode/kxz10105.bin
image_name=ICX64R08030u.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> ping 10.45.1.140
Link Status Changed, Re-Negotiation Start
Using egiga0 device
host 10.45.1.140 is alive
ICX64XX-boot>> setenv image_name ICX64S08030u.bin
ICX64XX-boot>> printenv
baudrate=9600
ipaddr=10.45.1.2
serverip=10.45.1.140
netmask=255.255.255.0
gatewayip=10.45.1.1
uboot=/foundry/FGS/bootcode/kxz10105.bin
image_name=ICX64S08030u.bin
ver=10.1.05T310 (Mar 19 2015 - 16:39:59)
ICX64XX-boot>> saveenv
ICX64XX-boot>> update_primary
Using egiga0 device
TFTP from server 10.45.1.140; our IP address is 10.45.1.2
Download Filename 'ICX64S08030u.bin'.
Load address: 0x3000000
Download to address: 0x3000000
Loading: %#################################################################
         #################################################################
         #################################################################
         #################################################################
         #################################################################
         #################################################################
         #################################################################
         #################################################################
         ################################################################
done
Bytes transferred = 8563580 (82ab7c hex)
prot off f8100000 f8cfffff
..............................................................................                                                     ..............................................................................                                                     ....................................
Un-Protected 192 sectors
erase f8100000 f8cfffff

.................................................
.................................................................
.................................................................
.............
Erased 192 sectors
copying image to flash, it will take sometime...
sflash write 3000000 100000 c00000
TFTP to Flash Done.
ICX64XX-boot>> boot_primary
Booting image from Primary
## Booting image at 00007fc0 ...
   Created:      2020-04-23  17:58:34 UTC
   Data Size:    8563004 Bytes =  8.2 MB
   Load Address: 00008000
   Entry Point:  00008000
   Verifying Checksum ... OK
OK

Starting kernel in BE mode ...
Uncompressing Image...........................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ................................................................. done, bootin                                                     g the kernel.
Config partition mounted.
Creating TUN device
Starting the FastIron.
FIPS Disabled:PORT NOT DISABLED
platform type 48
OS>Unable to set the kernel wall time
                                      Starting Main Task .INFO: startup config                                                      data is not available, try to read from backup
INFO: startup config data in the backup area is not available
CPSS DxCh Version: cpss3.4p1 release
Pre Parsing Config Data ...
INFO: empty config data in the primary area, try to read from backup
INFO: empty config data in the backup area also

Parsing Config Data ...
INFO: empty config data in the primary area, try to read from backup
INFO: empty config data in the backup area also

System initialization completed...console going online.
  Copyright (c) 1996-2016 Brocade Communications Systems, Inc. All rights rese                                                     rved.
    UNIT 1: compiled on Apr 23 2020 at 10:57:26 labeled as ICX64S08030u
                (8563580 bytes) from Primary ICX64S08030u.bin
        SW: Version 08.0.30uT311
  Boot-Monitor Image size = 786944, Version:10.1.05T310 (kxz10105)
  HW: Stackable ICX6430-C12
==========================================================================
UNIT 1: SL 1: ICX6430C 12-port Management Module
         Serial  #: CPW3204M08L
         License: BASE_SOFT_PACKAGE   (LID: eryIHFJoFNn)
         P-ENGINE  0: type E7EE, rev 01
==========================================================================
UNIT 1: SL 2: ICX6430C-Copper 2port 2G Module
==========================================================================
UNIT 1: SL 3: ICX6430C-Fiber 2port 2G Module
==========================================================================
  500 MHz ARM processor ARMv5TE, 400 MHz bus
32768 KB flash memory
  256 MB DRAM
STACKID 1  system uptime is 6 second(s)
The system started at 00:00:20 GMT+00 Thu Jan 01 1970

 The system : started=cold start

ICX6430-C12 Switch>Copy code image from primary to secondary
Load to buffer (8192 bytes per dot) ..........................................                                                     ..............................................................................                                                     ...................................................................
Stack unit 1 PS 1, Internal Power supply detected and up.

Stack unit 1 PS 1, Internal Power supply detected and up.
PoE: Stack unit 1 PS 1, Internal Power supply  with 68000 mwatts capacity is u                                                     p
PoE Info: Adding new 54V capacity of 68000 mW, total capacity is 68000, total                                                      free capacity is 68000
..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     .
SYNCING IMAGE TO FLASH. DO NOT SWITCH OVER OR POWER DOWN THE UNIT(8192 bytes p                                                     er dot)...
................................................................PoE Info: PoE                                                      module 1 of Unit 1 on ports 1/1/1 to 1/1/4 detected. Initializing....
PoE Event Trace Log Buffer for 2000 log entries allocated
PoE Event Trace Logging enabled...
........................................................PoE Info: PoE module 1                                                      of Unit 1 initialization is done.
..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ..............................................................................                                                     ...................................................................Copy code i                                                     mage done

ICX6430-C12-Switch>show flash
Stack unit 1:
  Compressed Pri Code size = 8563580, Version:08.0.30uT311 (ICX64S08030u.bin)
  Compressed Sec Code size = 8563580, Version:08.0.30uT311 (ICX64S08030u.bin)
  Compressed Boot-Monitor Image size = 786944, Version:10.1.05T310
  Code Flash Free Space = 6991872
ICX6430-C12-Switch>copy flash flash secondary
Invalid input -> copy flash flash secondary
Type ? for a list
ICX6430-C12-Switch>
  enable            Enter Privileged mode
  ping              Ping IP node
  show              Display system information
  stop-traceroute   Stop current TraceRoute
  traceroute        TraceRoute to IP Node
ICX6430-C12-Switch>ping 10.45.1.1
Sending 1, 16-byte ICMP Echo to 10.45.1.1, timeout 5000 msec, TTL 64
Type Control-c to abort
Reply from 10.45.1.1       : bytes=16 time=1ms TTL=64
Success rate is 100 percent (1/1), round-trip min/avg/max=1/1/1 ms.
ICX6430-C12-Switch>ping 10.45.1.140
Sending 1, 16-byte ICMP Echo to 10.45.1.140, timeout 5000 msec, TTL 64
Type Control-c to abort
Reply from 10.45.1.140     : bytes=16 time=2ms TTL=128
Success rate is 100 percent (1/1), round-trip min/avg/max=2/2/2 ms.
ICX6430-C12-Switch>traceroute 10.45.1.140

Type Control-c to abort
Tracing the route to IP node 10.45.1.140(10.45.1.140) from 1 to 30 hops

  1    <1 ms   <1 ms   <1 ms 10.45.1.140
ICX6430-C12-Switch>enable
No password has been assigned yet...
```