# Kali Linux Installation and Setup (Optional Setup Guide)

> Complete this section only if Kali Linux has not already been installed on your machine.

---

## Step 1 - Download Kali Linux

Visit the Kali Linux download page.

Download the ARM64 installer if you are using an Apple Silicon Mac (M1/M2/M3/M4).

Download the AMD64 installer if you are using a Windows PC or Intel-based machine.

### Apple Silicon Users

Download:

```text
kali-linux-xxxx-installer-arm64.iso
```

### Windows / Intel Users

Download:

```text
kali-linux-xxxx-installer-amd64.iso
```

---

## Step 2 - Open Oracle VirtualBox

Launch Oracle VirtualBox.

You should see the VirtualBox Manager window.

---

## Step 3 - Create a New Virtual Machine

Click:

```text
New
```

---

## Step 4 - Configure Virtual Machine

### Name

```text
Kali Linux
```

### ISO Image

Select the downloaded Kali Linux ISO file.

Example:

```text
kali-linux-xxxx-installer-arm64.iso
```

or

```text
kali-linux-xxxx-installer-amd64.iso
```

---

## Step 5 - Configure Hardware

### Memory (RAM)

Recommended:

```text
4096 MB
```

Minimum:

```text
2048 MB
```

### Processors

Recommended:

```text
2 CPUs
```

If your machine has sufficient resources:

```text
4 CPUs
```

---

## Step 6 - Create Virtual Hard Disk

Select:

```text
VDI
```

Disk Size:

```text
40 GB
```

Recommended:

```text
50 GB
```

Click:

```text
Finish
```

---

## Step 7 - Configure Network

Right-click the VM.

Select:

```text
Settings
```

Navigate to:

```text
Network
```

Adapter 1:

```text
NAT
```

Leave all other settings as default.

Network Architecture:

```text
Host Machine
     │
     ▼
VirtualBox NAT
     │
     ▼
Internet
```

This allows Kali Linux to access the internet safely through the host machine.

---

## Step 8 - Start Kali Linux

Select the VM.

Click:

```text
Start
```

The Kali Linux installer should load.

---

## Step 9 - Begin Installation

Choose:

```text
Graphical Install
```

---

## Step 10 - Installation Settings

### Language

```text
English
```

### Location

```text
United Kingdom
```

### Keyboard

```text
British English
```

---

## Step 11 - Host Configuration

Hostname:

```text
kali
```

Domain Name:

Leave blank.

---

## Step 12 - Create User Account

Full Name:

```text
Student
```

Username:

```text
kali
```

Password:

```text
kali
```

---

## Step 13 - Disk Partitioning

Select:

```text
Guided - Use Entire Disk
```

Then:

```text
All files in one partition
```

Finally:

```text
Finish partitioning and write changes to disk
```

Choose:

```text
Yes
```

when prompted.

---

## Step 14 - Install Kali Linux

Wait for the installation process to complete.

Typical installation time:

```text
10–20 minutes
```

depending on your hardware.

---

## Step 15 - Install GRUB Bootloader

When prompted:

```text
Install GRUB Bootloader?
```

Choose:

```text
Yes
```

Install to the default disk shown by the installer.

---

## Step 16 - Finish Installation

Select:

```text
Continue
```

The virtual machine will reboot.

---

## Step 17 - Login

Username:

```text
kali
```

Password:

```text
kali
```

---

## Step 18 - Verify Installation

Open Terminal and run:

```bash
whoami
```

Expected Output:

```text
kali
```

Check IP Address:

```bash
ip a
```

Check Internet Connectivity:

```bash
ping google.com
```

Stop the ping command using:

```text
Ctrl + C
```

---

## Step 19 - Verify OpenSSL

Run:

```bash
openssl version
```

You should see the installed OpenSSL version.

---

## Step 20 - Create Lab Folder

Run:

```bash
mkdir COM398
cd COM398
```

You are now ready to begin the Week 2 Cryptography Lab.
