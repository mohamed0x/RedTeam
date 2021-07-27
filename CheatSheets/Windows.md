# Windows

## Core Commands

```c
# List folders and files
dir
dir C:\Users
dir /a
dir /a C:\Users

# Change directory
E: | C:
cd C:\Users
cd ..
cd Desktop

# Create directory
mkdir test

# Remove directory
rmdir test
rmdir /s non-empty-test

# Move files or folders
move file1.txt folder1
move a.txt b.txt
move folder1 folder2

# Print text
echo "Hello World"
echo "test" > file1.txt

# View content of a file
type file1.txt

# Delete file
del file1.txt

# Copy file
copy a.txt b.txt
copy a.txt folder1

# Rename file
ren a.txt newname.txt

# Change attributes of files or folders
attrib +h a.txt
attrib -h a.txt
attrib +h folder
dir /a:h

# List running processes
tasklist

# Kill running process
taskkill /f /pid 1547

# Display network information
ipconfig
ipconfig /all

# Display active connections
netstat
netstat /ano

# Test network connectivity
ping google.com

# Show path to destination
tracert google.com

# Create links
mklink softlink originalfile
mklink /H hardlink originalfile
mklink /D link dir # Soft link only
```

## File System

* **C:\** Top level point that hold all the system files.
* **C:\Windows\** Contains operating system files.
* **C:\Program Files\** Contains applications files.
* **C:\Program Files \(x86\)\** Contains 32bit applications on 64bit operating system.
* **C:\Users\** Contains users home directories.
* **C:\ProgramData\** Contains configuration files of applications \(Hidden\).

## File Permissions

* **Windows has 5 main permission types:**
  * Full Control
  * Modify
  * Read & execute
  * Read
  * Write
* **Each user and group on the system has its own permissions:**
  * User1: Read
  * User2: Read + Write
  * Group1: Full Control
* **Inherited permissions,** child inherit parent permissions \(This is what happens by default in windows\).
* **Explicit permissions,** permission applied specially to a file.
* **Explicit** Deny &gt; **Explicit** Allow &gt; **Inherited** Deny &gt; **Inherited** Allow.

![](../../.gitbook/assets/change-file-permissions-on-windows-7-step-20.jpg)

## Users & Groups

```c
# Create user
net user testuser * /add

# Delete user
net user testuser /delete

# Disable/Enable user account
net user testuser /active:no
net user testuser /active:yes

# Display password policy
net accounts

# Most used groups
Administrators: Full control over system.
Network Operators: Allow to modify network settings (IP, DNS).
Users: Allow access to needed functionality by most users.

# Display groups
net localgroup 

# List group members
net localgroup administrators 

# Create group
net localgroup testgroup /add

# Add user to group
net localgroup testgroup testuser /add

# Delete user from group
net localgroup testgroup testuser /del 

# Delete group
net localgroup testgroup /del
```

## UAC

UAC gives you administrator access for one command, you trigger it by clicking right click on any file then choose run as administrator.

![](../../.gitbook/assets/uac_2.png)

## Runas

```c
# runas lets you run command as another user like in linux (su testuser -c whoami)
runas /user:testuser whoami 
runas /user:testuser "ping google.com"
```

## Credentials

* Credentials \(usernames & passwords\) are stored in the SAM file.
* SAM file location: C:\Windows\System32\config\SAM.
* Mostly stores the users' passwords in the NTLM hash.

```c
# Example
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
test:1001:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
test123:1008:aad3b435b51404eeaad3b435b51404ee:7a21990fcd3d759941e45c490f143d5f:::
```

## Security Policy

* **Audit policy**: Control logs through event viewer.
* **User rights**: Control permissions on the OS \(change time, backup, shutdown\).
* **Security options**: Rename administrator account, Account policy Password length, complexity, expiration period. Account lockout threshold, duration.

## Registry

Registry stores configuration settings and there are 5 types of registry hives:

* **HKEY\_Classes\_Root \(HKCR\)**: settings for applications
* **HKEY\_Current\_User \(HKCU\)**: settings for the current user
* **HKEY\_Local\_Machine \(HKLM\)**: local machine settings
* **HKEY\_USERS \(HKU\)**: Settings for every user
* **HKEY\_Current\_Config \(HKCC\)**: settings for hardware

```c
# Print help
reg /?

# Print help on specific function
reg query /?

# Query specific registry hives
reg query hkcu
reg query hklm
reg query hkcu\software

# Print all startup apps recursively
reg query hklm\software\microsoft\windows\currentversion\run /s

# Add startup key called EvilTest which started the Calculator app at boot time
reg add hklm\software\microsoft\windows\currentversion\run /v "EvilTest" /d "calc.exe"

# Delete EvilTest key
reg delete hklm\software\microsoft\windows\currentversion\run /v "EvilTest"
```

## Windows Sharing \(SMB\)

```c
# Print help
net /?

# List all devices that is enable sharing on the network
net view

# list all shared resources on specific system
net view \\computer-name

# mapping resource to drive letter
net use z: \\computer-name\resource

# show mapping resources
net use

# delete mapping resource
net use z: /delete
```

## Services

```c
# Display active services
sc query

# Display info about spooler service
sc query spooler

# Display inactive services
sc query state=inactive

# Display all services
sc query state=all

# Start/Stop the spooler service
sc start spooler
sc stop spooler

# Enable/Disable spooler service at boot time
sc config spooler start=disabled
sc config spooler start=auto
```

## Processes

```c
# List all tasks
tasklist

# Find task by it’s name or id
tasklist /fi "imagename eq calc*"
tasklist /pid 4054

# Force kill process
taskkill /f /pid 4054

# List all tasks
wmic process list brief

# Create calculator process
wmic process call create calc.exe

# Search for calculator process
wmic process where (name = "calculator.exe") list brief
wmic process where (name like "calc%") list brief

# Delete calculator process
wmic process where (name = "calculator.exe") delete
```

## Task Scheduling

```c
# Display all scheduled tasks
schtasks

# Create new task to run every minute
schtasks /create /sc minute /mo 1 /tn eviloo /tr calc.exe

# Display information about specific scheduled task
schtasks /query /tn eviloo

# Delete scheduled task
schtasks /delete /tn eviloo
```
