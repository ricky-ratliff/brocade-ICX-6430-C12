# Ubuntu 22.04 SSH Setup Issue Troubleshooting

## Table of Contents

- [Ubuntu 22.04 SSH Setup Issue Troubleshooting](#ubuntu-2204-ssh-setup-issue-troubleshooting)
  - [Table of Contents](#table-of-contents)
  - [Ubuntu SSH Commands](#ubuntu-ssh-commands)
  - [Brocade SSH Commands](#brocade-ssh-commands)
    - [View/Follow (SSH) System Logs](#viewfollow-ssh-system-logs)
  - [Config Files](#config-files)
    - [Brocade SSH Configs](#brocade-ssh-configs)
    - [Ubuntu ~/.ssh/config File](#ubuntu-sshconfig-file)
    - [Ubuntu tftpd-hpa config file](#ubuntu-tftpd-hpa-config-file)
  - [Issue #1: TFTP Public Key](#issue-1-tftp-public-key)
    - [Ubuntu SysLog Errors](#ubuntu-syslog-errors)
    - [Switch Errors](#switch-errors)
    - [Resolution](#resolution)
    - [Cleaning Up](#cleaning-up)
  - [Issue #2: Private Key Invalid Format](#issue-2-private-key-invalid-format)
    - [Current Key](#current-key)
    - [New Key](#new-key)
  - [Issue #3: Error in libcrypto When Loading Key](#issue-3-error-in-libcrypto-when-loading-key)

## Ubuntu SSH Commands

- Show SSH version: `ssh -V`
- Show SSH key length: `ssh-keygen -l -f ~/.ssh/id_rsa`
  - The key length is the first returned value
- Restart sshd service (after updating config file): `sudo systemctl restart sshd`

## Brocade SSH Commands

- Display SSH configuration: `show ip ssh config`
- Display currently loaded public keys: `show ip client-pub-key`
- Clear public keys: `clear public-key`
- Load public key from a TFTP server: `ip ssh pub-key-file tftp 1.2.3.4 public_key_file_name`

### View/Follow (SSH) System Logs

- `tail -F /var/log/syslog`
- `journalctl -fu ssh`

## Config Files

### Brocade SSH Configs

```shell
rknet-bro(config)#show ip ssh config
SSH server                 : Enabled
SSH port                   : tcp\22
Host Key                   :  RSA 2048
Encryption                 : aes256-cbc, aes192-cbc, aes128-cbc, aes256-ctr, aes192-ctr, aes128-ctr, 3des-cbc
Permit empty password      : No
Authentication methods     : Password, Public-key, Interactive
Authentication retries     : 3
Login timeout (seconds)    : 120
Idle timeout (minutes)     : 0
SCP                        : Enabled
SSH IPv4 clients           : All
SSH IPv6 clients           : All
SSH IPv4 access-group      :
SSH IPv6 access-group      :
SSH Client Keys            :
```

### Ubuntu ~/.ssh/config File

```yaml
Host rknet-bro
   Hostname 10.45.1.2
   IdentitiesOnly yes
   IdentityFile ~/.ssh/id_rsa
   KexAlgorithms +diffie-hellman-group1-sha1
   PubkeyAcceptedKeyTypes=+ssh-rsa
   HostKeyAlgorithms=+ssh-rsa
```

### Ubuntu tftpd-hpa config file

```yaml
# /etc/default/tftpd-hpa

TFTP_USERNAME="tftpd-hpa"
TFTP_DIRECTORY="/home/ricky/.ssh/"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="--secure -vvvv"
```

## Issue #1: TFTP Public Key

Public key file transfer via tftpd-hpa from Ubuntu 22.04 to brocade ICX 6430-C12 fails. 

### Ubuntu SysLog Errors

```shell
ricky@rknixnova:~$ tail -F /var/log/syslog
Oct 16 19:00:48 rknix-nova kernel: [40652.357182] [UFW BLOCK] IN=wlp0s20f3 OUT= MAC=bc:09:1b:05:fd:a7:60:9c:9f:38:4b:b6:08:00 SRC=10.45.1.2 DST=10.45.1.224 LEN=67 TOS=0x00 PREC=0x00 TTL=64 ID=61 DF PROTO=UDP SPT=1027 DPT=69 LEN=47 
```

### Switch Errors

```shell
rknet-bro(config)#TFTP: received error request -- code 0 message Permission denied
SSH tftp client public key failed!
```

```shell
rknet-bro(config)#ip ssh pub-key-file tftp 10.45.1.224 id_rsa.pub
downloading public key file, please wait...
rknet-bro(config)#ERROR: key# 1 must end with ---- END SSH2 PUBLIC KEY ----
Error in SSH Public Key file!
```

### Resolution

Changed TFTP_USERNAME to "nobody" and restarted the tftpd-hpa server.

```yaml
# /etc/default/tftpd-hpa

TFTP_USERNAME="nobody"
TFTP_DIRECTORY="/home/ricky/.ssh/"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="--secure -vvvv"
```

Changed ~/.ssh directory permissions and invoked tail in follow mode on /var/log/syslog before attempting to download the public key over tftp on the switch again to monitor the attempt.

```shell
~$ chmod 777 ./.ssh

Oct 16 21:31:36 rknix-nova in.tftpd[67453]: RRQ from 10.45.1.2 filename rknix@icx-6430-c12.pub

rknet-bro(config)#ip ssh pub-key-file tftp 10.45.1.224 rknix@icx-6430-c12.pub
downloading public key file, please wait...
rknet-bro(config)#Public key written

Finished downloading public key file!
```

I finalized the change on the switch by writing the config to memory with `write memory`.

### Cleaning Up

With the public key file now loaded on the switch, I needed to revert some changes to keep the systems secure.

- Reset permissions on ~/.ssh directory `chmod 700 ~/.ssh`
- Reset permissions on public key file `chmod 644 ~/.ssh/key-file.pub`
- Re-enabled UFW `sudo ufw enable`
- Disabled tftpd-hpa `sudo systemctl disable tftpd-hpa`

## Issue #2: Private Key Invalid Format

Attempting to ssh into Brocade ICX 6430-C12 from Ubuntu 22.04 produces the following error:

```shell
ssh_dispatch_run_fatal: Connection to 10.45.1.2 port 22: invalid format
```

### Current Key

- Bit length is 3072 bits, should be 2048 per switch specifications.
  - ssh-keygen -l -f ./id_rsa
3072 SHA256:zTvX5mhtOjOJJA4WMXkfrgNuN+Ca50kuv5W1JzyijiY ricky@rknix-nova (RSA)

### New Key

- ssh-keygen
  - [x] Use RSA: `-t rsa`
  - [x] Set to 2048 bits: `-b 2048`
  - [x] Must have a passphrase/password. Brocade ssh config parameter `Permit empty password` is set to `no`.
    - [x] Set when prompted during key generation

For the switch to accept your public key file it should start with:

---- BEGIN SSH2 PUBLIC KEY ----

and end with:

---- END SSH2 PUBLIC KEY ----

- Updated public and private keys with begin/end lines, confirmed they both ended with a newline, and successfully loaded the public key on the switch. 
- Successfully connected to the switch with ssh and root password. 

``` shell
ricky@rknix-nova:~/.ssh$ ssh root@rknet-bro 
Load key "/home/ricky/.ssh/id_rsa": error in libcrypto
(root@10.45.1.2) Password:
(root@10.45.1.2) Password:
SSH@rknet-bro>
```

## Issue #3: Error in libcrypto When Loading Key

```shell
ricky@rknix-nova:~$ ssh root@rknet-bro -v
OpenSSH_8.9p1 Ubuntu-3ubuntu0.10, OpenSSL 3.0.2 15 Mar 2022
debug1: Reading configuration data /home/ricky/.ssh/config
debug1: /home/ricky/.ssh/config line 1: Applying options for rknet-bro
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
debug1: Connecting to 10.45.1.2 [10.45.1.2] port 22.
debug1: Connection established.
debug1: identity file /home/ricky/.ssh/id_rsa type 0
debug1: identity file /home/ricky/.ssh/id_rsa-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
debug1: Remote protocol version 2.0, remote software version RomSShell_5.40
debug1: compat_banner: no match: RomSShell_5.40
debug1: Authenticating to 10.45.1.2:22 as 'root'
debug1: load_hostkeys: fopen /home/ricky/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: diffie-hellman-group1-sha1
debug1: kex: host key algorithm: ssh-rsa
debug1: kex: server->client cipher: aes128-ctr MAC: hmac-sha1 compression: none
debug1: kex: client->server cipher: aes128-ctr MAC: hmac-sha1 compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: SSH2_MSG_KEX_ECDH_REPLY received
debug1: Server host key: ssh-rsa SHA256:y9NwhoULZbDbGorN6A5Bpa21J2KdwnjRuwNqHdfDKyM
debug1: load_hostkeys: fopen /home/ricky/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: Host '10.45.1.2' is known and matches the RSA host key.
debug1: Found key in /home/ricky/.ssh/known_hosts:8
debug1: rekey out after 4294967296 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 4294967296 blocks
debug1: get_agent_identities: bound agent to hostkey
debug1: get_agent_identities: agent returned 1 keys
debug1: Will attempt key: /home/ricky/.ssh/id_rsa RSA SHA256:QGXk9X+eV0X0s70k9mGUiQ3q17gPyVbQZuG0QXx77S0 explicit
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey,password,keyboard-interactive
debug1: Next authentication method: publickey
debug1: Offering public key: /home/ricky/.ssh/id_rsa RSA SHA256:QGXk9X+eV0X0s70k9mGUiQ3q17gPyVbQZuG0QXx77S0 explicit
debug1: Server accepts key: /home/ricky/.ssh/id_rsa RSA SHA256:QGXk9X+eV0X0s70k9mGUiQ3q17gPyVbQZuG0QXx77S0 explicit
Load key "/home/ricky/.ssh/id_rsa": error in libcrypto
debug1: Next authentication method: keyboard-interactive
(root@10.45.1.2) Password:

```
