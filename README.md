# Brocade ICX 6430-C12 Configuration & Documentation

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

I followed the software recovery process found on page 40 of the **FastIron Ethernet Switch Software Upgrade Guide, 08.0.30d**, which I downloaded from the Ruckus ICX 6430 and 6450 Campus Switches product page on the [Ruckus Support Website](https://support.ruckuswireless.com/products/121-ruckus-icx-6430-and-6450-campus-switches?open=document#sort=relevancy&f:@source=[Documentation]&f:@commonproducts=[ICX-6430-6450]).

### TFTPD64 Log

```text
Connection received from 10.45.1.2 on port 2747 [11/10 14:35:41.934]
Read request for file <ICX64S08030u bin>. Mode octet [11/10 14:35:41.934]
OACK: <timeout=5,blksize=1468,> [11 10 14:35:41.949]
Using local port 51562 [11/10 14:35:41.949]
<ICX64S08030u.bin>: sent 5834 blks, 8563580 bytes in 2 s. 0 blk resent [11/10 14:35:43.119]
```

## Commands

```shell
alias                        Display configured aliases
boot                         Boot system from bootp/tftp server/flash image
clear                        Clear table/statistics/keys
clock                        Set clock
configure                    Enter configuration mode
copy                         Copy between flash, tftp, scp, config/code
debug                        Enable debugging functions (see also 'undebug')
disable                      Disable system monitoring
dot1x                        802.1X
downgrade_to                 downgrade to a version prior to 8.0
enable                       Enable system monitoring
erase                        Erase image/configuration from flash
execute                      Execute commands in batch
exit                         Exit Privileged mode
inline                       Inline power (PoE) configuration/operation
jitc                         JITC execution commands
kill                         Kill active CLI session
page-display                 Display data one page at a time
phy                          PHY related commands
ping                         Ping IP node
port                         Port security command
quit                         Exit to User level
reload                       Halt and perform a warm restart
remote-packet-capture        Info about remote packet capture utility
show                         Display system information
simulate-non-stacking-unit   Simulate the absence of the stacking PROM
skip-page-display            Enable continuous display
ssh                          SSH by name or IP address / hostkeys
stop-traceroute              Stop TraceRoute operation
supportsave                  support save related
telnet                       Telnet by name or IP address
temperature                  temperature sensor commands
terminal                     display syslog
trace-l2                     TraceRoute L2
traceroute                   TraceRoute to IP node
undebug                      Disable debugging functions (see also 'debug')
verify                       Verify object contents
whois                        WHOIS lookup
write                        Write running configuration to flash or terminal
```
