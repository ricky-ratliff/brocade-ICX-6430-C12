# Brocade ICX 6430-C12 Configuration & Documentation

This repo contains coniguration notes and documentation for the Brocade ICX 6430-C12 I have deployed in my homelab.

## Management Interfaces

### Console Managment Interface

#### OIKWAN USB to RJ45 Console Cable

- Uses an FTDI FT232R chip
- Virtual COM port (VCP) driver used to provide a COM port on the PC for serial terminal emulation access

#### Out-of-Band Management Interface

| Internet Address | Physical Address  | Type    |
| ---------------- | ----------------- | ------- |
| 10.45.1.2        | 60:9c:9f:38:4b:c8 | dynamic |

#### Web Management Interface

| Internet Address | Physical Address  | Type    |
| ---------------- | ----------------- | ------- |
| 10.45.1.175      | 60:9c:9f:38:4b:b6 | dynamic |

## Initial Configuration Troubleshooting

To get console access, I connected the out of band management interface to my LAN gateway and used an RJ45 to USB cable with an embedded FTDI VCP (Virtual COM Port) chip to connect my PC to the console port. I then verified the COM port number in Device Manager (COM3). I used PuTTY to make the serial connection to COM3.

My first thought was to run a show command to see what the current configuration was, but I was met with an "unknown command" error and advised to try "help" instead. I took the advice and was then given a list of available commands to try out.

```shell
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
update_secondary   - secondary update; update secondary  partition
update_uboot - get the uboot image over tftp.
version - print monitor version
```

I was getting a "Bad Magic Number" error when attemting to boot by entering the `boot`, `bootcmd`, `boot_primary`, or `boot_secondary` commands:

```shell
ICX64XX-boot>> boot
Booting image from Primary
Bad Magic Number
could not boot from primary, no valid image; trying to boot from secondary
Booting image from Secondary
Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
could not boot from secondary, no valid image; trying to boot from primary
Booting image from Primary
Bad Magic Number
## Booting image at 01ffffc0 ...
Bad Magic Number
```

After some research, I learned that the magic number is used to validate the Master Boot Record (MBR), further confirming that the boot image is not present, corrupted, incompatible, or otherwise not functional. This sent me down the path of many failed attempts to update after changing environment variables and eventually obtaining and installing a new boot image by following the software recovery process.

## Software Recovery Process

I followed the software recovery process found on page 40 of the [FastIron Ethernet Switch Software Upgrade Guide, 08.0.30d](./brocade%20documentation/fastiron-08030d-upgradeguide.pdf), which I downloaded from the **Ruckus ICX 6430 and 6450 Campus Switches** product page on the [Ruckus Support Website](https://support.ruckuswireless.com/products/121-ruckus-icx-6430-and-6450-campus-switches?open=document#sort=relevancy&f:@source=[Documentation]&f:@commonproducts=[ICX-6430-6450]).

### Software recovery on ICX 6430, ICX 6450, ICX 6650, ICX 7450, ICX 7750, and FSX devices

*NOTE In practice, the TFTP server is also used as the terminal server to see the CLI output.*

1. Connect a console cable from the console port to the terminal server.
2. Connect an Ethernet cable from the management port (the port located under the console port on the device) to the TFTP
server.
3. On the TFTP server, assign an IP address to the connected NIC; for example, IP address 10.10.10.21 mask 255.255.255.0.
4. Reboot the device, and go to the boot monitor mode by pressing "b".
5. When in boot mode, enter the printenv command to display details of the images available on the device memory; for example:

```shell
ICX64XX-boot> printenv
baudrate=9600
uboot=/foundry/FGS/bootcode/kxz07400.bin
ver=07.4.00T310 (Mar 1 2012 - 11:28:23)
```

6. Provide the IP address of the TFTP server that hosts a valid software image using the setenv serverip command; for example:
ICX64XX-boot> setenv serverip 10.10.10.21
7. Set the IP address, gateway IP address, and netmask for the device management port, and save the configuration using the
setenv ipaddr, setenv gatewayip, setenv netmask, and saveenv commands; for example:

```shell
ICX64XX-boot> setenv ipaddr 10.10.10.22
ICX64XX-boot> setenv gatewayip 10.10.10.1
ICX64XX-boot> setenv netmask 255.255.255.0
ICX64XX-boot> saveenv
```

*NOTE The IP address and the gateway IP address set for the device management port should be for the same subnet as the TFTP server NIC.*

8. Enter the printenv command to verify the IP addresses that you configured for the device and the TFTP server; for example:

```shell
ICX64XX-boot> printenv
baudrate=9600
ipaddr=10.10.10.22
gatewayip=10.10.10.1
netmask=255.255.255.0
serverip=10.10.10.1
uboot=/foundry/FGS/bootcode/kxz07400.bin
ver=07.4.00T310 (Mar 1 2012 - 11:28:23)
```

9. Test the connectivity to the TFTP server from the device using the ping command to ensure a working connection; for example:

```shell
ICX64XX-boot> ping 10.10.10.21
ethPortNo = 0
Using egiga0 device
host 10.10.10.21 is alive
```

10.  Provide the file name of the image that you want to copy from the TFTP server using the setenv image_name command; for
example:
`ICX64XX-boot> setenv image_name images/ICX/ICX64R08000.bin`
11.  Update the primary flash using the update_primary command; for example:

```shell
ICX64XX-boot> update_primary
ethPortNo = 0
Using egiga0 device
TFTP from server 10.10.10.21; our IP address is 10.10.10.22
Download Filename 'ICX64S07400.bin'.
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
#################################################################
#################################################################
########################################################
done
Bytes transferred = 10360844 (9e180c hex)
prot off f8100000 f907ffff
................................................................................
................................................................................
................................................................................
........
Un-Protected 248 sectors
erase f8100000 f907ffff
.................................................
.................................................................
.................................................................
.................................................................
....
Erased 248 sectors
copying image to flash, it will take sometime...
sflash write 3000000 100000 f80000
TFTP to Flash Done.
```

12.  Load the image from the primary flash using the boot_primary command; for example:

```shell
ICX64XX-boot> boot_primary
Booting image from Primary
## Booting image at 00007fc0 ...
Created: 2012-03-02 20:38:52 UTC
Data Size: 10360268 Bytes = 9.9 MB
Load Address: 00008000
Entry Point: 00008000
Verifying Checksum ... OK
OK
Starting kernel in BE mode ...
Uncompressing Image.............................................................
................................................................................
................................................................................
................................................................................
................................................................................
........................................... done, booting the kernel.
Config partition mounted.
```

13.  Enter show flash and see the output to check whether the image copy process was successful.

```shell
ICX6430-C12-Switch# show flash
Stack unit 1:
  Compressed Pri Code size = 8563580, Version:08.0.30uT311 (ICX64S08030u.bin) [Primary]
  Compressed Sec Code size = 8563580, Version:08.0.30uT311 (ICX64S08030u.bin) [Secondary]
  Compressed Boot-Monitor Image size = 786944, Version:10.1.05T310
  Code Flash Free Space = 6991872
```

14.  Copy the image from the primary to the secondary flash partition using the copy flash flash secondary command

#### TFTPD64 Log

```text
Connection received from 10.45.1.2 on port 2747 [11/10 14:35:41.934]
Read request for file <ICX64S08030u bin>. Mode octet [11/10 14:35:41.934]
OACK: <timeout=5,blksize=1468,> [11 10 14:35:41.949]
Using local port 51562 [11/10 14:35:41.949]
<ICX64S08030u.bin>: sent 5834 blks, 8563580 bytes in 2 s. 0 blk resent [11/10 14:35:43.119]
```

## Current Switch Configurations

### Running Config
```shell
SSH@rknet-bro(config)#show running-config
Current configuration:
!
ver 08.0.30uT311
!
stack unit 1
  module 1 icx6430c-12-port-management-module
  module 2 icx6430c-copper-2port-2g-module
  module 3 icx6430c-fiber-2port-2g-module
  no legacy-inline-power
!
aaa authentication web-server default local
aaa authentication login default local
enable aaa console
hostname rknet-bro
ip address 10.45.1.2 255.255.255.0
ip dns server-address 10.45.1.1
no ip dhcp-client enable
ip default-gateway 10.45.1.1
!
no telnet server
username root password .....
!
clock summer-time
clock timezone gmt GMT-06
!
ntp
 disable serve
 server 216.239.35.0
 server 216.239.35.4
!
web-management https
!
ip ssh  password-authentication no
ip ssh  interactive-authentication no
!
end

```
### Flash
```shell
ICX6430-C12-Switch# show flash
Stack unit 1:
  Compressed Pri Code size = 8563580, Version:08.0.30uT311 (ICX64S08030u.bin)
  Compressed Sec Code size = 8563580, Version:08.0.30uT311 (ICX64S08030u.bin)
  Compressed Boot-Monitor Image size = 786944, Version:10.1.05T310
  Code Flash Free Space = 6991872
```
