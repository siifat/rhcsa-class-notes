**Date:** Saturday, May 17, 2025, 9:00 PM

## Topics Covered:
- Text Processing Tools  
- Users and Groups Administration  
- Privilege Administration  

---

## 1. File and Directory Management

### 1.1 Create a File:
```bash
touch name_of_file
```

### 1.2 Create a Folder:
```bash
mkdir name_of_folder
```

### 1.3 Save History to a File
```bash
history > name_of_file
```

- `history`: Lists previously run terminal commands.
- `>`: Redirects output and **overwrites** the target file.

---

## 2. Text Processing Tools

### 2.1 `cat` Command

#### 2.1.1 Purpose:
View, create, and combine text files.

#### 2.1.2 Basic Usage:

1. **Display contents of a file**
   ```bash
   cat filename.txt
   ```

2. **Create a new file**
   ```bash
   cat > newfile.txt
   ```
   - Type your content.
   - Press `Ctrl + D` to exit.

3. **Append to an existing file**
   ```bash
   cat >> existingfile.txt
   ```
   - Adds lines to the end.
   - Press `Ctrl + D` to finish.

---

### 2.2 `echo` Command

#### 2.2.1 Purpose:
Display **text or variables** in the terminal.

#### 2.2.2 Basic Usage:

1. **Print a simple message**
   ```bash
   echo Hello, UIU!
   ```
   Output:
   ```
   Hello, UIU!
   ```

2. **Print a blank line**
   ```bash
   echo
   ```

3. **Add text to a file (overwrite)**
   ```bash
   echo "This is a note" > note.txt
   ```
   - Overwrites the file `note.txt` with the new content.

4. **Append text to a file**
   ```bash
   echo "This is another line" >> note.txt
   ```
   - Appends the new content to `note.txt`.

5. **Print the value of a variable**
   ```bash
   my_var="Hello, World!"
   echo $my_var
   ```
   Output:
   ```
   Hello, World!
   ```

6. **Print the value of a variable with text**
   ```bash
   echo "The value is: $my_var"
   ```
   Output:
   ```
   The value is: Hello, World!
   ```

7. **Print with special characters**
   ```bash
   echo "The value is: $my_var & more"
   ```
   Output:
   ```
   The value is: Hello, World! & more
   ```

8. **Escape special characters**
   ```bash
   echo "The value is: $my_var \& more"
   ```
   Output:
   ```
   The value is: Hello, World! & more
   ```

### 💡 Tip:
Combine `echo` with `>>` to quickly create notes from the terminal:
```bash
echo "Date: $(date)" >> rhcsa_log.txt
```

### 🔍 Breakdown:
- `echo`: Displays text or variable content.
- `$(date)`: Inserts current date and time.
- `>>`: Appends output to a file.
- `rhcsa_log.txt`: The target file.

---

### 📝 Text Editors for File Editing:
- `nano`: A simple, beginner-friendly text editor.
- `vim`: A powerful and advanced text editor.

---

# 👤 Users and Groups Administration

### 1. **See all users**
```bash
cat /etc/passwd
```
- Displays user account information.

Output:
```bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:998:998:User for polkitd:/:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/:/sbin/nologin
pipewire:x:997:996:PipeWire System Daemon:/run/pipewire:/usr/sbin/nologin
sssd:x:996:995:User for sssd:/:/sbin/nologin
geoclue:x:995:994:User for geoclue:/var/lib/geoclue:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
colord:x:994:993:User for colord:/var/lib/colord:/sbin/nologin
clevis:x:993:992:Clevis Decryption Framework unprivileged user:/var/cache/clevis:/usr/sbin/nologin
flatpak:x:992:991:Flatpak system helper:/:/usr/sbin/nologin
setroubleshoot:x:991:990:SELinux troubleshoot server:/var/lib/setroubleshoot:/usr/sbin/nologin
libstoragemgmt:x:989:989:daemon account for libstoragemgmt:/:/usr/sbin/nologin
gdm:x:42:42:GNOME Display Manager:/var/lib/gdm:/usr/sbin/nologin
gnome-initial-setup:x:988:987::/run/gnome-initial-setup/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:987:986:chrony system user:/var/lib/chrony:/sbin/nologin
dnsmasq:x:986:985:Dnsmasq DHCP and DNS server:/var/lib/dnsmasq:/usr/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
server:x:1000:1000:server:/home/server:/bin/bash
```

### 🔍 Breakdown:

Let's take 2 users that we've created. `root` and `server`:
```bash
root    :x    :0      :0      :root     :/root          :/bin/bash
server  :x    :1000   :1000   :server   :/home/server   :/bin/bash
```

Let's look at them one by one:
```bash
root    :x    :0      :0      :root     :/root          :/bin/bash
```

- `root`: Username
- `:x` : Has a password
- `:0` : User ID **\[Root user id always 0\]**
- `:0`: Group ID **\[Root group id is always 0\]**
- `:root` : Comment **(Usually the username)**
- `:/root`: Home directory of `root`
- `:/bin/bash`: Interpreter. Commands run from here.

```bash
server  :x    :1000   :1000   :server   :/home/server   :/bin/bash
```

- `server`: Username
- `:x` : Has a password
- `:1000` : User ID **\[Regular user id always 1000 and 1000+\]**
- `:0`: Group ID **\[Regular group id always 1000 and 1000+\]**
- `:server` : Comment **(Usually the username)**
- `:/home/server`: Home directory of `server` user.
- `:/bin/bash`: Interpreter. Commands run from here.

**👉🏻 System user ID ranges from : 1 to 999**


### **2. See all groups**
```bash
cat /etc/group
```

- Displays group account information

Output:
```bash
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:
cdrom:x:11:
mail:x:12:
man:x:15:
dialout:x:18:
floppy:x:19:
games:x:20:
tape:x:33:
video:x:39:
ftp:x:50:
lock:x:54:
audio:x:63:
users:x:100:
nobody:x:65534:
utmp:x:22:
utempter:x:35:
input:x:104:
kvm:x:36:
render:x:105:
sgx:x:106:
systemd-journal:x:190:
systemd-coredump:x:999:
dbus:x:81:
polkitd:x:998:
avahi:x:70:
printadmin:x:997:
ssh_keys:x:101:
rtkit:x:172:
pipewire:x:996:
sssd:x:995:
geoclue:x:994:
tss:x:59:clevis
colord:x:993:
clevis:x:992:
flatpak:x:991:
setroubleshoot:x:990:
libstoragemgmt:x:989:
brlapi:x:988:
gdm:x:42:
gnome-initial-setup:x:987:
sshd:x:74:
chrony:x:986:
slocate:x:21:
dnsmasq:x:985:
tcpdump:x:72:
server:x:1000:
```

#### 🔍 Breakdown:
```bash
root:x:0:
```

- `root`: Group name
- `x`: Has password
- `0` : Group ID

👉🏻 **As we didn't create any new group, the users created their own groups by their respective usernames. For example: `root` and `server`**


### **3. Creating new users and groups**

❓**Why create new groups?**
➡️ Privacy  

Suppose we'll create the following new users:

| Username | Password |
| -------- | -------- |
| cse      | cse123   |
| eee      | eee123   |
| ce       | ce123    |
| bba      | bba123   |
| eco      | eco123   |

And we'll also create the following groups:
- uiu
- sose
- sobe

The groups will contains the users we just created as follows:
- **uiu** : cse, eee, ce, bba, eco
- **sose**: cse, eee, ce
- **sobe**: bba, eco

**Create a new user**:
```bash
useradd cse
useradd eee
useradd ce
useradd bba
useradd eco
```

**Create password for each user:**
```bash
passwd cse
passwd eee
passwd ce
passwd bba
passwd eco
```

**Add new Groups:**
```bash
groupadd uiu
groupadd sose
groupadd sobe
```

#### Adding a user to a group
```bash
usermod -a -G uiu cse
```

- Adds the user `cse` to group `uiu`
- `-a` : append
- `-G` : add to a new Group
#### Remove a user from a group
```bash
nano /etc/group
```

Then find the group line:

```bash
groupname:x:1002:user1,user2,unwanteduser
```

🔻 Simply **remove `unwanteduser`** from the list.

#### Delete a user
```bash
userdel [username]
```
- Keeps the `home` directory of the user.
```bash
userdel -r [username]
```
- Deletes the user `home` directory as well.

---

# Privilege Administration

Let's create some shared directories in `/home`;
```bash
cd /home
mkdir UIU SOSE SOBE
ls
```

Output:
```bash
bba  ce  cse  eco  eee  server  SOBE  SOSE  UIU
```

- We want to give `UIU` directory access to all users in `uiu` group.
- We want to give `SOSE` directory access to all users in `sose` group.
- We want to give `SOBE` directory access to all users in `sobe` group.

### User permissions

```bash
cd /home
ls -l
```

Output:
```bash
total 0
drwx------. 3 bba    bba    78 May 22 13:15 bba
drwx------. 3 ce     ce     78 May 22 13:15 ce
drwx------. 3 cse    cse    78 May 22 00:36 cse
drwx------. 3 eco    eco    78 May 22 13:15 eco
drwx------. 3 eee    eee    78 May 22 13:14 eee
drwx------. 3 server server 78 May 21 18:52 server
drwxr-xr-x. 2 root   root    6 May 22 13:42 SOBE
drwxr-xr-x. 2 root   root    6 May 22 13:42 SOSE
drwxr-xr-x. 2 root   root    6 May 22 13:42 UIU
```

**Understanding permissions**
```bash
drwxr-xr-x. 2 root   root    6 May 22 13:42 UIU
```

It has 5 parts:
1. File Type: `d`
2. Permissions: `rwxr-xr-x`
3. Owner of the file/directory: `root`
4. Which group can access this file/directory: `root`
5. Name of file/directory: `UIU`

```
"Permissions"   File attributes that define who can access or change a file
                and whether a file can be executed as a program.

                 "-rwxr-xr-x" Shows file type and permissions.

                +-------------+------+-------+
                | d---------  |  File Type   |
                +-------------+------+-------+
                |    - = regular file        |
                |    d = directory           |
                |    l = link                |
                +-------------+------+-------+
                | Permission  | Octal| Field |
                +-------------+------+-------+
                | rwx------   | 700  | User  |
                | ---rwx---   | 070  | Group |
                | ------rwx   | 007  | Other |
                +-------------+------+-------+

                Octal values:

                Read    => r-- => 100 => 4
                Write   => -w- => 010 => 2
                Execute => --x => 001 => 1

                8 bits = 1 byte. Uses digits 0 1 2 3 4 5 6 7.
```

🔔 When **applying permissions to directories** on Linux, the permission bits have different meanings than on regular files. 

| Permission    | Meaning for a Directory 📂                                             |
| ------------- | ---------------------------------------------------------------------- |
| `r` (read)    | You can **list** the contents of the directory.                        |
| `w` (write)   | You can **create, delete, or rename** files inside.                    |
| `x` (execute) | You can **enter** (cd into) the directory and **access** its contents. |

You can set permissions in the file manager or with the command `chmod`. 
Examples:

#### **Set a file to be executable and readable by everyone but only writable by you** 

**Method 1**
```bash
chmod 770 filename
```

**Method 2**
```bash
chmod g+r filename
chmod g+w filename
chmod g+x filename

chmod o-r filename
chmod o-w filename
chmod o-x filename
```
- `g`: Group
- `o`: Other
- Gives `group` users full permission and removes `other` users `rwx` permission.

#### **Set execute bit without changing other permissions**

```bash
chmod +x filename
```

#### **Change the ownership**

```bash
chown [newowner] [target_file/directory]
```

#### **Change group ownership**

```bash
chgrp [newgroup] [target_file/directory]
```

