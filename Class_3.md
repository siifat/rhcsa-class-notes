**Date:** Saturday, May 19, 2025, 9:00 PM

## Topics Covered:
- Privilege Administration  
- Control Access for Linux (Advanced File Permission)
- ACL
- Archiving and Compression
- Network Configuration


Today we'll create 4 new users.
```
cse -> cse123
eee -> eee123
bba -> bba123
eco -> eco123
```

**Group:**
uiu : cse, eee, bba, eco
sose: cse, bba
sobe: bba, eco

# Shared folder
```bash
cd /home
mkdir UIU SOSE SOBE
```
Let's see the detailed permissions output by
```bash
ll
```
Output:
```bash
total 0
drwxr-xr-x. 2 root root 6 May 29 11:43 SOBE
drwxr-xr-x. 2 root root 6 May 29 11:43 SOSE
drwxr-xr-x. 2 root root 6 May 29 11:43 UIU
```
We want to give **no access** to the `group` and `other`
```bash
chmod 770 *
```
Output of `ll` now:
```bash
total 0
drwx------. 2 root root 6 May 29 11:43 SOBE
drwx------. 2 root root 6 May 29 11:43 SOSE
drwx------. 2 root root 6 May 29 11:43 UIU
```
In previous class, we changed the ownership of the file/folders directly to give a specific user permission of that file/folder. But today we'll take a different approach to this task. 

# Setting ACL

If we want to check if a file/folder has ACL:
```bash
getfacl SOSE
```
output:
```bash
# file: SOSE
# owner: root
# group: root
user::rwx
group::---
other::---
```
- user has read, write, execute permission
- we'll add a new specific user with the same permissions as `user`

Suppose, we want to give ACL to the `eee` user:
```bash
setfacl -m u:eee:rwx SOSE
```
Let's break down this command:

- `setfacl`: Command used to set file access control lists.
- `-m`: Modifies the ACL of a file or directory.
- `u:eee:rwx`: This specifies the entry to be added or modified.
    - `u`: Denotes a user entry.
    - `eee`: The username to whom you're granting permissions.
    - `rwx`: The permissions to grant (read, write, execute).
- `SOSE`: The target file or directory.

To set ACL for multiple users at once:
```bash
setfacl -m u:eee:rwx,u:cse:rwx SOSE
```
Output of `getfacl SOSE` now:
```bash
# file: SOSE
# owner: root
# group: root
user::rwx
user:cse:rwx    # This line has been added
user:eee:rwx    # This line has been added
group::---
mask::rwx       # This line has been added
other::---
```

**❗NOTE:**
Notice the output of `ll` now:
```bash
total 0
drwx------. 2 root root 6 May 29 11:43 SOBE
drwxrwx---+ 2 root root 6 May 29 11:43 SOSE   # Has a `+` at the end and group permission looks odd
drwx------. 2 root root 6 May 29 11:43 UIU
```

- The `+` means "ACLs are present, check `getfacl` for full details."
- The `rwx` in the group permissions field (`drwxrwx---+`) is _not_ necessarily the actual permissions for the _owning group_ (`root` in this case, which still has `---` according to `getfacl`). Instead, it is typically a representation of the **ACL mask**, indicating the maximum permissions that can be granted to _any_ user or group through ACLs.

Always rely on `getfacl` to understand the true and complete permission set when you see a `+` in.
## Remove ACL
```bash
setfacl -x u:cse SOSE
```

# ACL for groups
Same Command almost.
```bash
setfacl -m g:sobe:r-x SOBE
```
- `-m`: Modify
- `g` : Group
- `SOBE` : Target file or directory

## Archive📦and Compress 🗃️
#### Archive:
Combine multiple files and/or directories into a single file.
- `.tar`
#### Compress:
Reduce the size of a file or set of files by encoding the data in a more compact form.
- `.gz` (least compressed)
- `.bz2` (in between)
- `.xz` (most compressed)

#### Task: Copy the `/etc` directory to `/home` and archive it. See the size before and after
Copy `etc` directory to `/home`:
```bash
cp -r /etc /home
```
- To copy folders `-r` option is necessary.

See the size of copied `etc` folder:
```bash
du -sh /home/etc
```
- `-s` : Summarize
- `-h` : Human readable size

👉 **`ls -lh` ≠ shows content size**  
👉 **`du` = shows true disk usage**

Archive `etc` using `tar` format:
```bash
tar -cvf newname.tar etc/
```
- `-c`: Create
- `-f`: Give a name for the `.tar` file
- `-v`: Verbose

#### Archive and Compress:
```bash
tar -czvf file.tar.gz etc/
```
- `-z`: Compress using gzip
- `-j`: bz2
- `-J`: xz

#### Extract the archived/compressed file

Extract `.tar`:
```bash
tar -xf file.tar
```

Extract `.tar.gz`:
```bash
tar -xzf file.tar.gz
```

Extract `.tar.bz2`:
```bash
tar -xjf file.tar.bz2
```

Extract `.tar.xz`:
```bash
tar -xJf file.tar.xz
```

#### 📌 **How to extract to a different location**

Use the `-C` (uppercase C) option:
```bash
tar -xf file.tar -C /path/to/destination
```

# Network Configuration
### 📌 **What `ifconfig` does**

`ifconfig` = **interface configuration**  
👉 It shows and configures your network interfaces (like `eth0`, `wlan0`, `lo`).

Typical uses:
- Check IP address & MAC address
- Bring interfaces up or down
- Assign static IPs (older method

🔑 **Tip:** If `ifconfig` is missing, install it with:
```bash
sudo apt install net-tools  # Debian/Ubuntu
sudo yum install net-tools  # CentOS/RHEL
```

However, in *minimal mode* or *server mode* even this command will not work. We need to set it up manually.

So, we can use:
```bash
ip address show
```
Output:
```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:5a:8c:77 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 192.168.0.192/24 brd 192.168.0.255 scope global dynamic noprefixroute ens160
       valid_lft 4170sec preferred_lft 4170sec
    inet6 2401:f40:1355:1da:20c:29ff:fe5a:8c77/64 scope global dynamic noprefixroute 
       valid_lft 296sec preferred_lft 296sec
    inet6 fe80::20c:29ff:fe5a:8c77/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

We can see the ip address is: `192.168.0.192/24`
And our **NIC (Network Interface Card)** is: `ens160`

Let's see the host ip address by:
```PowerShell
ipconfig
```

Output:
```PowerShell
Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   IPv6 Address. . . . . . . . . . . : 2401:f40:1355:1da:6328:5852:7705:7a34
   Temporary IPv6 Address. . . . . . : 2401:f40:1355:1da:68fe:cd80:1cc5:c6a3
   Link-local IPv6 Address . . . . . : fe80::943b:f9bf:5a6d:5642%15
   IPv4 Address. . . . . . . . . . . : 192.168.0.199
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : fe80::62a4:b7ff:fea9:71bf%15
                                       192.168.0.1

Ethernet adapter VMware Network Adapter VMnet1:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::3fd9:c57a:54f3:a4f6%17
   IPv4 Address. . . . . . . . . . . : 192.168.126.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :

Ethernet adapter VMware Network Adapter VMnet8:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::5bd4:2880:3af2:e876%13
   IPv4 Address. . . . . . . . . . . : 192.168.47.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :

Ethernet adapter Bluetooth Network Connection:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
```

We can see the host ip address is: `192.168.0.199`.
We want to bridge with this host ip address.

🔑 **Note:** If we're using a VM, we need to make sure that the network adapter is set to **Bridged**. But in exam it will already be setup.


#### Task:
We'll set the following:
- IP: `192.168.0.199/24`
- Gateway: `192.168.0.1`
- DNS: 8.8.8.8
- Hostname: `server.uiu.com`
- NIC: `ens160`

See the host details inside the vm:
```bash
hostnamectl status
```

Output:
```bash
   Static hostname: (unset)                                 
Transient hostname: localhost
         Icon name: computer-vm
           Chassis: vm 🖴
        Machine ID: 1f657a5bf7b64209b11476cf1c99330a
           Boot ID: 6a0b0ac055774e629322298ef1ff5511
    Virtualization: vmware
  Operating System: Red Hat Enterprise Linux 9.6 (Plow)     
       CPE OS Name: cpe:/o:redhat:enterprise_linux:9::baseos
            Kernel: Linux 5.14.0-570.12.1.el9_6.x86_64
      Architecture: x86-64
   Hardware Vendor: VMware, Inc.
    Hardware Model: VMware20,1
  Firmware Version: VMW201.00V.24006586.B64.2406042154
```

Set hostname:
```bash
hostnamectl set-hostname server.uiu.com
```

**Note:** Our terminal prompt should look like this before setting the hostname:
```bash
[root@localhost home]#
```

But after setting the hostname, if we relaunch the terminal we can see the new hostname:
```bash
[root@server ~]# 
```

Now we'll set the IP. To do that we'll use `nmcli` or the **network manager command line interface**.
```bash
nmcli connection show
```
Output:
```bash
NAME    UUID                                  TYPE      DEVICE 
ens160  cd0d84eb-f285-3aa9-a065-f6db4b9e93b3  ethernet  ens160 
lo      b14f3adc-f609-4d62-b896-ea3e1e61f93e  loopback  lo     
```

Modify `ens160`:
```bash
nmcli connection modify ens160 ipv4.address 192.168.0.199/24 ipv4.gateway 192.168.0.1 ipv4.dns 8.8.8.8 ipv4.method manual connection.autoconnect yes
```
State up the connection:
```bash
nmcli connection up ens160
```
Output:
```bash
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/3)
```

**Note:** Also, in Red Hat systems, we can use `nmtui` to do the same task.