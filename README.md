
# Optimize your Samba Server 
### update 2024

By following these tips, you can optimize your Samba Server and provide a faster and more reliable file sharing service for your users.
> **⚠️ Warning:**  
> These settings are for advanced users only. Misconfigurations **will** damage your system. Always make a backup of your `smb.conf` file before making changes.
Note: These configurations are specifically designed for a Samba Server environment, not for consumer-grade NAS devices or systems where SMB is simply passing data packets throug!


## Table of Contents
- [Basics](#basics)
  - [Use the latest version of Samba](#use-the-latest-version-of-samba)
  - [Use the appropriate SMB protocol version](#use-the-appropriate-smb-protocol-version)
  - [Configure the network settings](#configure-the-network-settings)
  - [Optimize Samba settings](#optimize-samba-settings)
  - [Use an appropriate file system](#use-an-appropriate-file-system)
  - [Use a caching solution](#use-a-caching-solution)
  - [Enable Asynchronous I/O](#enable-asynchronous-io)
  - [Use Modern Compression](#use-modern-compression)
  - [Enable Multi-Channel Support](#enable-multi-channel-support)
  - [Optimize for SSD/NVMe Storage](#optimize-for-ssdnvme-storage)
  - [Regularly Monitor and Tune Performance](#regularly-monitor-and-tune-performance)
  - [Use the Latest Kernel](#use-the-latest-kernel)
- [Advanced Settings](#advanced-settings)
  - [Socket options](#socket-options)
  - [Read raw and write raw](#read-raw-and-write-raw)
  - [SMB encrypt](#smb-encrypt)
  - [Max protocol](#max-protocol)
- [Only for Advanced Users (can damage your system)](#only-for-advanced-users-can-damage-your-system)
  - [Example smb.conf file](#example-smbconf-file)

## Basics

### Use the latest version of Samba
Using the latest version of Samba can help ensure that you are using the most up-to-date and optimized code. You can check for the latest version of Samba on the official Samba website.

### Use the appropriate SMB protocol version
SMB supports multiple protocol versions, including SMBv1, SMBv2, and SMBv3. Using the latest protocol version that is supported by your clients and server can improve performance and security. You can configure the SMB protocol version in the Samba configuration file (/etc/samba/smb.conf).

### Configure the network settings
Configuring the network settings on your clients and server can help improve performance. For example, using a wired Ethernet connection instead of a wireless connection can provide a faster and more stable network connection. You can also optimize the network settings in the Samba configuration file.

### Optimize Samba settings
There are several Samba settings that you can optimize to improve performance, such as increasing the socket options, setting the read raw and write raw options to yes, and disabling SMB packet signing. You can configure these settings in the Samba configuration file.

### Use an appropriate file system
Using an appropriate file system on your server can also help improve performance. For example, using an SSD or NVMe drive can provide faster read and write speeds than a traditional hard drive. You can also consider using a file system that is optimized for Samba, such as the Samba File System (SFS).

### Use a caching solution
Using a caching solution, such as Samba's built-in VFS cache or an external caching solution like memcached, can help improve performance by reducing the number of disk accesses needed to serve requests.

### Enable Asynchronous I/O
Enabling asynchronous I/O (AIO) can improve performance by allowing the server to handle multiple I/O operations simultaneously. You can enable AIO in the Samba configuration file.

```shell
aio read size = 1
aio write size = 1
```

### Use Modern Compression
Modern compression algorithms can significantly reduce the amount of data transmitted over the network. You can enable SMB compression in Samba.


```shell
smb compression = yes
```
 - Enables SMB compression, which can significantly reduce the amount of data transmitted over the network.
 - Supported on Windows 10 and Windows Server 2019 or later.
Please read this! -> https://learn.microsoft.com/en-us/windows-server/storage/file-server/smb-compression

### Enable Multi-Channel Support
SMB3 introduced multi-channel support, which allows the use of multiple network connections for a single SMB session, increasing throughput and redundancy.

```shell
server multi channel support = yes
```
 - Enables SMB3 multi-channel support, which allows the use of multiple network connections for a single SMB session, increasing throughput and redundancy.
 - Supported on Windows Server 2012 R2 and later.

### Optimize for SSD/NVMe Storage
If you are using SSD or NVMe storage, you can further optimize Samba by adjusting settings specific to high-speed storage solutions.

```shell
write cache size = 10485760
```

### Regularly Monitor and Tune Performance
Regular monitoring of your Samba server's performance can help identify bottlenecks and areas for improvement. Use tools like \`smbstatus\`, \`iotop\`, and \`htop\` to monitor Samba activity and system performance.

### Use the Latest Kernel
The Linux kernel receives numerous updates and performance improvements. Using the latest stable kernel can provide better performance and hardware support for your Samba server.

## Advanced Settings

### Socket options
The socket options setting controls various socket options for the connection between the server and clients. You can increase the SO_RCVBUF and SO_SNDBUF values to increase the size of the receive and send buffers, respectively. This can help improve performance by allowing more data to be sent and received at once. Here's an example:

```shell
socket options = SO_RCVBUF=8192 SO_SNDBUF=8192
```

### Read raw and write raw
The read raw and write raw settings control whether Samba uses raw mode for reading and writing data. When set to yes, Samba bypasses the standard I/O buffering and caching mechanisms, which can help improve performance. Here's an example:

```shell
read raw = yes
write raw = yes
```

### SMB encrypt
The smb encrypt setting controls whether SMB packet encryption is used. When set to desired or required, all SMB packets are encrypted, which can help improve security but may decrease performance. You can set this to off if performance is more important than security. Here's an example:

```shell
smb encrypt = off
```

### Max protocol
The max protocol setting controls the maximum SMB protocol version that Samba will use. Setting this to the latest version that is supported by your clients and server can help improve performance and security. Here's an example:

```shell
max protocol = SMB3
```

You can also adjust other settings in the smb.conf file to further optimize Samba performance. Be sure to test any changes in a non-production environment before making them on your production server.

> **⚠️ Warning:**  
> These settings are for advanced users only. Misconfigurations can damage your system. Always make a backup of your `smb.conf` file before making changes.

### Example smb.conf file
An example smb.conf file that is optimized for performance:

```shell
[global]
; Set socket options to optimize network performance
socket options = TCP_NODELAY IPTOS_LOWDELAY SO_RCVBUF=65536 SO_SNDBUF=65536

; Enable raw read operations
read raw = yes

; Enable raw write operations
write raw = yes

; Set the maximum SMB protocol version to SMB3
max protocol = SMB3

; Require server signing for all communications
server signing = mandatory

; Require encryption for all SMB communications
smb encrypt = required

; Enable strict allocation of file space
strict allocate = yes

; Enable opportunistic locking
oplocks = yes

; Enable kernel-level opportunistic locking
kernel oplocks = yes

; Disable POSIX locking
posix locking = no

; Ensure data is always synchronized with disk
sync always = yes

; Use the sendfile system call for sending files
use sendfile = yes

; Set the minimum size for receivefile operation
min receivefile size = 16384

; Set the size of the write cache
write cache size = 2097152

; Set the time in minutes before an idle connection is terminated
deadtime = 15

; Set the maximum transmit packet size
max xmit = 65535

; Enable large read and write operations
large readwrite = yes

; Set the maximum size of the log file in kilobytes
max log size = 2048

; Set the logging level (1 = minimal logging)
log level = 1

[share1]
; Path to the shared directory
path = /path/to/share1

; Allow read and write access to valid users
read only = no

; Specify valid users who can access the share
valid users = user1

; Specify users who have write access to the share
write list = user1

```

This example smb.conf file includes some of the settings mentioned earlier, such as socket options, read raw, write raw, and max protocol. It also includes other settings to further optimize Samba performance, such as sync always, use sendfile, and large readwrite.

Note that this is just an example configuration, and you may need to adjust the settings based on your specific environment and requirements. Also, be sure to test any changes in a non-production environment before making them on your production server.

## Credits
**Volkan Sah**
