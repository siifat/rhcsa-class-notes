**Date:** Tuesday, May 20, 2025, 9:00 PM

## Topics Covered:
- Linux Cron-job's
- Linux root password reset
- Managing Firewall
- Grant a user Sudo Privileges

---
## 1. Linux Cron Job's
- **Service**: `crond`
- **Package**: `crontabs`

### 1.1 Check if cron is installed
In RPM based systems:
```bash
rpm -qa | grep crontabs
```
- `-q`: Query
- `-a`: All

### 1.2 Start and Enable cron service
```bash
systemctl start crond
systemctl enable crond
```

### 1.3 Edit a cron job
```bash
crontab -e 
```
- `-e`: Edit

We usually create a cron job in below format:
```bash
* * * * * <Command>
```

There are 5 stars. They indicate:
1. Minutes (0 - 59)
2. Hour (0 - 23)
3. Date (1 - 31)
4. Month (1 - 12)
5. Day of Week (0 - 6) \[0 = Sunday]

### ❓Ques: Create a cron job that will reboot your server everyday at 11:59 PM

**Solution:**

**Step 1: First, let's check where is the `reboot` command:**
```bash
which reboot
```

**Example Output:**
```bash
/usr/sbin/reboot
```

 **Step 2: Open the cron tab:**
```bash
crontab -e
```

**Step 3: Set the cron job**
```
59 23 * * * /usr/sbin/reboot
```
- `*` means default value
- Save the file using `:wq`

**Step 4: Restart cron service**
```bash
systemctl restart crond
```

### 1.4 List all cron jobs
```bash
crontab -l
```

---
## 2. Root Password Reset (rpm based systems only)

### ❓Ques: Set your root password `uiu12345`

**Solution:**

**Step 1: Reboot your system and wait for GRUB screen**

You may see a screen like this:
![[{893D2C8A-72FF-42A5-BF80-D77CEFB67123}.png]]<br>

Now Press `e` to edit the boot commands.

**Step 2: Edit the command and boot into command list**

After you pressed `e`, you'll be greeted with a screen like this:
![[{FA0B5DA6-563E-45E5-98CE-CF37AEF5A246}.png]]<br>

Go to the last line of `linux` and add `rd.break`.<br>
Then Press `Ctrl + X`

**Step 3: Remount `/sysroot`**

We're in **Emergency Mode** now. You'll see a screen like this:
![[{16E32528-202E-4EE9-ADDF-1AB07FD14DA9}.png]]

`sysroot` is already mounted. However, we want to remount it in read/write mode.
```bash
mount -o remount,rw /sysroot
```

**Step 4: Change to `sysroot` and change the password**
```bash
chroot /sysroot
```

```bash
passwd
```
Set the password to `uiu12345`

**Step 5: Create a supporting password file and exit chroot**
```bash
touch /.autorelabel
```

```bash
exit
```

---
## 3. Managing Firewall
### 3.1 Firewall status
```bash
systemctl status firewalld
```

### 3.2 See Firewall services
```bash
firewall-cmd --list-service
```

### 3.3 See Firewall Ports
```bash
firewall-cmd --list-port
```

### 3.4 Add a Service
```bash
firewall-cmd --add-service=http --permanent
```

We can add different types of services here:
- http
- ftp
- smtp (Simple Main Transfer Protocol)
- imap
- pop (Post Office Protocol)

**Explanation:**
- `--permanent`: Add this service permanently to the list

Then we need to reload the Firewall:
```bash
firewall-cmd --reload
```

### 3.5 Add a Port
```bash
firewall-cmd --add-port=53/tcp --permanent # TCP
firewall-cmd --add-port=53/udp --permanent # UDP

# Reload
firewall-cmd --reload
```

### 3.6 Remove a Service
```bash
firewall-cmd --remove-service=http

firewall-cmd --reload
```

### 3.7 Remove a Port
```bash
firewall-cmd --remove-port=53/tcp
firewall-cmd --remove-port=53/udp

firewall-cmd --reload
```

---

## 4. Grant user SUDO privileges

**Step 1: Open the `sudoers` file**
```bash
nano /etc/sudoers
```

**Step 2: Add the user**

Find the line which looks something like below:
```bash
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
```

Add the user you'd like to grant SUDO privileges:
```bash
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
server  ALL=(ALL)       ALL
```
