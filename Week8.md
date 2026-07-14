# COM398 Systems Security

# Week 8 - Configuring Secure Remote Access with SSH

---

# Introduction

In today's lab, we are going to configure **secure remote access to a Cisco switch using SSH**.

Before entering any commands, let us understand the problem.

Imagine you are working as a network engineer.

An organisation may have switches located:

* in different rooms
* across different buildings
* inside data centres
* in different offices

A network engineer cannot physically connect to every switch whenever a configuration needs to be changed.

Instead, network devices are often managed remotely.

```text
Network Engineer
       |
       v
      PC
       |
       | Remote Management
       v
 Cisco Network Device
```

In today's lab:

```text
PC1
 |
 | Remote Management
 |
 v
S1 Switch
```

The main security question is:

> How can PC1 securely connect to and manage S1?

We will first configure **Telnet**.

We will observe the security problem.

We will then replace Telnet with **SSH**.

---

# What We Will Do Today

The complete workflow is:

```text
Build the Network
        |
        v
Configure PC1
        |
        v
Configure S1
        |
        v
Configure VLAN 1
        |
        v
Test Connectivity
        |
        v
Configure Telnet
        |
        v
Observe the Security Problem
        |
        v
Protect Stored Passwords
        |
        v
Configure SSH
        |
        v
Disable Telnet
        |
        v
Verify SSH
```

By the end of the lab, you should be able to:

1. Build a simple network using Cisco Packet Tracer.
2. Connect a PC to a Cisco switch.
3. Configure a static IPv4 address.
4. Configure a switch management IP address.
5. Explain VLAN 1 and an SVI.
6. Test connectivity using `ping`.
7. Configure Telnet remote access.
8. Explain why Telnet is insecure.
9. Inspect the running configuration.
10. Protect plain-text passwords.
11. Configure an IP domain name.
12. Generate RSA keys.
13. Create a local user account.
14. Configure VTY lines.
15. Enable SSH-only remote access.
16. Verify that Telnet fails.
17. Verify that SSH succeeds.

---

# Part 1 - Understand the Network

We will build the following topology.

```text
+--------------------------+
|           PC1            |
|                          |
| IP: 10.10.10.10          |
| Mask: 255.255.255.0      |
+------------+-------------+
             |
             |
             | Ethernet Connection
             |
             |
+------------+-------------+
|            S1            |
|       Cisco Switch       |
|                          |
| VLAN 1: 10.10.10.2       |
| Mask: 255.255.255.0      |
+--------------------------+
```

PC1 represents the administrator's computer.

S1 is the Cisco switch we want to manage remotely.

---

# Addressing Table

| Device | Interface     | IP Address  | Subnet Mask   |
| ------ | ------------- | ----------- | ------------- |
| PC1    | FastEthernet0 | 10.10.10.10 | 255.255.255.0 |
| S1     | VLAN 1        | 10.10.10.2  | 255.255.255.0 |

Both devices belong to:

```text
10.10.10.0/24
```

---

# Quick Networking Recap

## What is an IP Address?

An IP address identifies a device on an IP network.

Example:

```text
10.10.10.10
```

In our lab:

```text
PC1 = 10.10.10.10

S1 = 10.10.10.2
```

These are different devices.

However, both addresses belong to the same local network.

---

# What is a Subnet Mask?

The subnet mask helps a device determine which IP addresses belong to its local network.

We use:

```text
255.255.255.0
```

This can also be written as:

```text
/24
```

Therefore:

```text
10.10.10.10/24
```

and:

```text
10.10.10.2/24
```

are in the same network.

For today's lab, the simple idea is:

> PC1 and S1 are neighbours on the same local network.

---

# Part 2 - Build the Network in Packet Tracer

Open:

```text
Cisco Packet Tracer
```

Create a new blank Packet Tracer project.

Do not start with SSH commands yet.

We first need a working network.

---

# Activity 1 - Add the Switch

At the bottom-left of Packet Tracer:

1. Select **Network Devices**.
2. Select **Switches**.
3. Choose a **2960** switch.
4. Drag the switch into the logical workspace.

Use:

```text
Cisco 2960
```

---

# Why Are We Using a Cisco 2960?

The Cisco 2960 is a Layer 2 switch commonly used in Packet Tracer networking activities.

In today's activity, S1 has two purposes:

```text
Network Connectivity
```

and:

```text
Remote Device Management
```

Our goal is to securely manage the switch from PC1.

---

# Rename the Switch

Click the switch.

Change the display name to:

```text
S1
```

The letter `S` represents:

```text
Switch
```

Therefore:

```text
S1 = Switch 1
```

Meaningful device names are important.

Compare:

```text
Switch
Switch
Switch
```

with:

```text
S1
S2
S3
```

In a real organisation, names may look like:

```text
LON-SW-01
LON-SW-02
MAN-SW-01
```

For this lab, use:

```text
S1
```

---

# Activity 2 - Add PC1

At the bottom-left of Packet Tracer:

1. Select **End Devices**.
2. Select **PC**.
3. Drag the PC into the workspace.

Rename the PC:

```text
PC1
```

You should now have:

```text
PC1                         S1
```

---

# What Does PC1 Represent?

PC1 represents the network administrator's computer.

```text
Network Administrator
          |
          v
         PC1
```

The administrator wants to manage:

```text
S1
```

remotely.

---

# Activity 3 - Connect PC1 to S1

At the bottom-left of Packet Tracer, select:

```text
Connections
```

Choose:

```text
Copper Straight-Through
```

Click:

```text
PC1
```

Select:

```text
FastEthernet0
```

Then click:

```text
S1
```

Select:

```text
FastEthernet0/1
```

The connection is:

```text
PC1 FastEthernet0
        |
        |
        | Copper Straight-Through
        |
        |
S1 FastEthernet0/1
```

Wait for the connection indicators to become green.

---

# What Does the Green Link Mean?

A green link indicates that the network connection is operational.

The switch port may initially appear orange.

Wait a few seconds.

The link should eventually become green.

---

# Quick Question

Have we configured SSH?

```text
No
```

Have we configured Telnet?

```text
No
```

Can we remotely manage S1 yet?

```text
No
```

At this point, we have only completed the physical connection.

The workflow is:

```text
Physical Connection
        |
        v
IP Configuration
        |
        v
Network Connectivity
        |
        v
Remote Access Protocol
```

SSH comes later.

---

# Part 3 - Configure PC1

# Activity 4 - Open IP Configuration

Click:

```text
PC1
```

Select:

```text
Desktop
```

Open:

```text
IP Configuration
```

Select:

```text
Static
```

---

# Configure the PC1 IPv4 Address

Enter:

```text
IPv4 Address: 10.10.10.10
```

Enter:

```text
Subnet Mask: 255.255.255.0
```

The configuration should be:

```text
IPv4 Address:    10.10.10.10
Subnet Mask:     255.255.255.0
Default Gateway:
DNS Server:
```

Leave the default gateway blank.

Leave the DNS server blank.

---

# Why Is the Default Gateway Blank?

A default gateway is used when a device needs to communicate with another network.

Example:

```text
PC
 |
 v
Router
 |
 v
Another Network
```

Our topology is:

```text
PC1
 |
 v
S1
```

Both devices belong to:

```text
10.10.10.0/24
```

There is no router in this topology.

Therefore, a default gateway is not required.

---

# Why Is DNS Blank?

DNS converts domain names into IP addresses.

Example:

```text
www.google.com
       |
       v
IP Address
```

In this lab, PC1 will directly connect to:

```text
10.10.10.2
```

We do not need DNS to locate S1.

---

# Verify PC1 Configuration

Close IP Configuration.

Go to:

```text
PC1
```

then:

```text
Desktop
```

Open:

```text
Command Prompt
```

Run:

```text
ipconfig
```

Confirm that you can see:

```text
IP Address: 10.10.10.10
```

and:

```text
Subnet Mask: 255.255.255.0
```

---

# Part 4 - Configure the Cisco Switch

# Activity 5 - Open the S1 CLI

Click:

```text
S1
```

Select:

```text
CLI
```

You may see:

```text
Continue with configuration dialog? [yes/no]:
```

Enter:

```text
no
```

Press Enter.

You may then see:

```text
Press RETURN to get started!
```

Press Enter.

You should reach:

```text
Switch>
```

---

# Understanding Cisco CLI Modes

Cisco IOS uses different command modes.

---

## User EXEC Mode

Prompt:

```text
Switch>
```

This provides basic access.

---

## Privileged EXEC Mode

Prompt:

```text
Switch#
```

This provides administrative and verification commands.

Enter privileged EXEC mode using:

```text
enable
```

The flow is:

```text
Switch>
   |
   | enable
   v
Switch#
```

---

## Global Configuration Mode

Prompt:

```text
Switch(config)#
```

This mode is used to change the switch configuration.

Enter it using:

```text
configure terminal
```

The complete flow is:

```text
Switch>
   |
   | enable
   v
Switch#
   |
   | configure terminal
   v
Switch(config)#
```

---

# Activity 6 - Enter Configuration Mode

At:

```text
Switch>
```

run:

```text
enable
```

You should see:

```text
Switch#
```

Run:

```text
configure terminal
```

You should see:

```text
Switch(config)#
```

---

# Activity 7 - Configure the Hostname

Run:

```text
hostname S1
```

The prompt should change from:

```text
Switch(config)#
```

to:

```text
S1(config)#
```

---

# What is a Hostname?

A hostname identifies a network device.

Before:

```text
Switch
```

After:

```text
S1
```

The hostname helps administrators identify the device.

The hostname is also required as part of the SSH configuration process.

---

# Part 5 - Configure the Switch Management Interface

Before entering the commands, we need to understand VLAN 1 and an SVI.

---

# What is VLAN 1?

VLAN stands for:

```text
Virtual Local Area Network
```

A VLAN logically groups devices inside a switched network.

Cisco switches normally have a default VLAN:

```text
VLAN 1
```

For this introductory lab, VLAN 1 will be used for switch management.

---

# What is a Management IP Address?

PC1 has:

```text
10.10.10.10
```

But PC1 needs an IP address to contact on S1.

We will configure:

```text
10.10.10.2
```

on VLAN 1.

```text
PC1
10.10.10.10
      |
      v
S1 VLAN 1
10.10.10.2
```

The address:

```text
10.10.10.2
```

is the switch management IP address.

---

# What is an SVI?

SVI stands for:

```text
Switch Virtual Interface
```

An SVI is a logical interface on a switch.

In our lab:

```text
interface vlan 1
```

is the SVI.

The switch management IP address is configured on the VLAN interface.

We do not configure:

```text
10.10.10.2
```

directly on:

```text
FastEthernet0/1
```

The structure is:

```text
FastEthernet0/1
        |
        | Carries Ethernet Traffic
        v
VLAN 1
        |
        | Management Interface
        v
10.10.10.2
```

---

# Activity 8 - Configure VLAN 1

At:

```text
S1(config)#
```

run:

```text
interface vlan 1
```

The prompt changes to:

```text
S1(config-if)#
```

Now configure the IP address:

```text
ip address 10.10.10.2 255.255.255.0
```

Enable the interface:

```text
no shutdown
```

The complete configuration is:

```text
interface vlan 1
ip address 10.10.10.2 255.255.255.0
no shutdown
```

Run:

```text
exit
```

---

# What Does `no shutdown` Mean?

The command:

```text
shutdown
```

means:

```text
Administratively Disabled
```

The command:

```text
no shutdown
```

means:

```text
Administratively Enabled
```

Simple idea:

```text
shutdown = OFF

no shutdown = ON
```

---

# Activity 9 - Verify VLAN 1

Run:

```text
end
```

You should see:

```text
S1#
```

Run:

```text
show ip interface brief
```

Find:

```text
Vlan1
```

You should see:

```text
10.10.10.2
```

Example:

```text
Interface              IP-Address      OK? Method Status   Protocol
Vlan1                  10.10.10.2     YES manual up       up
FastEthernet0/1        unassigned      YES unset  up       up
```

The important information is:

```text
Vlan1 = 10.10.10.2
```

Ideally, VLAN 1 should show:

```text
up    up
```

---

# What Does `up/up` Mean?

The first `up` represents the interface status.

The second `up` represents the line protocol status.

```text
Status      Protocol

up          up
```

For today's lab:

> `up/up` means the interface is operational.

---

# Troubleshooting - VLAN 1 is Down

If you see:

```text
Vlan1     10.10.10.2     down     down
```

check the following.

## Check 1

Is PC1 connected to S1?

```text
PC1 FastEthernet0
        |
        v
S1 FastEthernet0/1
```

## Check 2

Is the link green?

## Check 3

Did you enter:

```text
no shutdown
```

under:

```text
interface vlan 1
```

## Check 4

Run:

```text
show ip interface brief
```

again.

Do not continue until VLAN 1 is correctly configured.

---

# Part 6 - Test Network Connectivity

Before configuring Telnet or SSH, verify that the network works.

A network engineer should troubleshoot in the correct order.

```text
Physical Connection
        |
        v
IP Configuration
        |
        v
Network Connectivity
        |
        v
Remote Access Protocol
```

Do not immediately blame SSH if the network itself is not working.

---

# Activity 10 - Ping S1 from PC1

Click:

```text
PC1
```

Select:

```text
Desktop
```

Open:

```text
Command Prompt
```

Run:

```text
ping 10.10.10.2
```

---

# Expected Result

You should receive replies from:

```text
10.10.10.2
```

The first ping may fail while Packet Tracer resolves local network information.

Run the ping again if required.

---

# What is Ping?

Ping tests whether one IP device can reach another IP device.

```text
PC1
 |
 | ICMP Echo Request
 v
S1
 |
 | ICMP Echo Reply
 v
PC1
```

---

# What is ICMP?

ICMP stands for:

```text
Internet Control Message Protocol
```

ICMP is used for network control and diagnostic messages.

Ping normally uses:

```text
ICMP Echo Request
```

and:

```text
ICMP Echo Reply
```

When we run:

```text
ping 10.10.10.2
```

we are asking:

> Can PC1 communicate with the S1 management interface?

---

# If Ping Fails

Do not continue to SSH.

Check PC1:

```text
IP Address: 10.10.10.10
Subnet Mask: 255.255.255.0
```

Check S1:

```text
show ip interface brief
```

Confirm:

```text
Vlan1    10.10.10.2
```

Check the cable.

Check the link indicators.

Check the running configuration:

```text
show running-config
```

Find:

```text
interface Vlan1
 ip address 10.10.10.2 255.255.255.0
```

Once ping succeeds, continue.

---

# Checkpoint 1

| Item                    | Expected Result         |
| ----------------------- | ----------------------- |
| End device              | PC                      |
| PC name                 | PC1                     |
| Switch                  | Cisco 2960              |
| Switch hostname         | S1                      |
| Cable                   | Copper Straight-Through |
| PC1 port                | FastEthernet0           |
| S1 port                 | FastEthernet0/1         |
| PC1 IP                  | 10.10.10.10             |
| PC1 subnet mask         | 255.255.255.0           |
| S1 management interface | VLAN 1                  |
| S1 IP                   | 10.10.10.2              |
| S1 subnet mask          | 255.255.255.0           |
| Ping                    | Successful              |

Do not continue if ping is unsuccessful.

---

# Part 7 - Understand Remote Management

Our network now works.

The next question is:

> How can PC1 remotely access the S1 command line?

We will compare:

```text
Telnet
```

and:

```text
SSH
```

---

# What is Telnet?

Telnet is a remote terminal protocol.

It allows a user to connect to another device and access its command-line interface.

```text
PC1
 |
 | Telnet
 v
S1 CLI
```

A network administrator can enter Cisco commands remotely.

---

# Why is Telnet a Security Problem?

Telnet does not provide secure encrypted remote communication.

Information may be transmitted in readable form.

This can include:

```text
User Input
Passwords
Commands
Management Information
```

Imagine sending your password on a postcard.

```text
Password: cisco
```

Anyone who can read the postcard can see the password.

That is the simple security problem with Telnet.

---

# What is Plain Text?

Plain text means information is directly readable.

Example:

```text
password cisco
```

The word:

```text
cisco
```

is directly visible.

---

# What is SSH?

SSH stands for:

```text
Secure Shell
```

SSH also provides remote command-line access.

However, SSH protects the communication using cryptography.

```text
PC1
 |
 | Encrypted SSH Connection
 v
S1
```

---

# Telnet vs SSH

| Feature                  | Telnet | SSH |
| ------------------------ | ------ | --- |
| Remote CLI access        | Yes    | Yes |
| Encrypted communication  | No     | Yes |
| Secure remote management | No     | Yes |
| Recommended              | No     | Yes |

The key idea is:

```text
Telnet = Insecure Remote Management

SSH = Secure Remote Management
```

---

# Part 8 - Configure Initial Telnet Access

Why are we configuring Telnet if it is insecure?

Because we want to observe the security problem before improving the system.

```text
Insecure Configuration
        |
        v
Observe the Problem
        |
        v
Apply Security Controls
        |
        v
Verify the Improvement
```

---

# Activity 11 - Configure the Enable Secret

Click:

```text
S1
```

Open:

```text
CLI
```

Enter privileged mode:

```text
enable
```

Enter global configuration mode:

```text
configure terminal
```

Run:

```text
enable secret cisco
```

---

# What is the Enable Secret?

When a user is at:

```text
S1>
```

they are in User EXEC mode.

To access:

```text
S1#
```

the user runs:

```text
enable
```

The enable secret protects privileged EXEC mode.

For this lab:

```text
Enable Secret = cisco
```

---

# Activity 12 - Configure VTY Lines for Telnet

At:

```text
S1(config)#
```

run:

```text
line vty 0 15
```

The prompt becomes:

```text
S1(config-line)#
```

Run:

```text
password cisco
```

Then:

```text
login
```

Then:

```text
transport input telnet
```

The full configuration is:

```text
line vty 0 15
password cisco
login
transport input telnet
```

Run:

```text
end
```

---

# What are VTY Lines?

VTY stands for:

```text
Virtual Teletype
```

VTY lines provide remote terminal sessions to Cisco devices.

```text
Remote User
     |
     v
VTY Remote Access Lines
     |
     v
Cisco CLI
```

We configured:

```text
line vty 0 15
```

This means VTY lines:

```text
0 through 15
```

---

# What Does `password cisco` Do?

The command:

```text
password cisco
```

sets the VTY line password.

---

# What Does `login` Do?

The command:

```text
login
```

tells the switch to request the configured line password.

---

# What Does `transport input telnet` Do?

The command:

```text
transport input telnet
```

allows incoming Telnet connections.

The current design is:

```text
PC1
 |
 | Telnet
 v
VTY Lines
 |
 | Password: cisco
 v
S1 CLI
```

---

# Activity 13 - Save the Initial Configuration

Run:

```text
copy running-config startup-config
```

When asked:

```text
Destination filename [startup-config]?
```

press Enter.

---

# Running Configuration vs Startup Configuration

## Running Configuration

The currently active configuration.

```text
running-config
```

## Startup Configuration

The saved configuration loaded when the device starts.

```text
startup-config
```

The process is:

```text
Running Configuration
        |
        | Save
        v
Startup Configuration
```

---

# Part 9 - Connect Using Telnet

# Activity 14 - Open PC1 Command Prompt

Click:

```text
PC1
```

Go to:

```text
Desktop
```

Open:

```text
Command Prompt
```

---

# Activity 15 - Telnet to S1

Run:

```text
telnet 10.10.10.2
```

The IP address:

```text
10.10.10.2
```

belongs to:

```text
S1 VLAN 1
```

When prompted for the password, enter:

```text
cisco
```

You should reach:

```text
S1>
```

---

# Question

What does:

```text
S1>
```

mean?

Answer:

```text
User EXEC Mode
```

---

# Activity 16 - Enter Privileged EXEC Mode

Run:

```text
enable
```

Enter:

```text
cisco
```

You should see:

```text
S1#
```

---

# Question

What is the difference between:

```text
S1>
```

and:

```text
S1#
```

## Answer

```text
S1>
```

is User EXEC mode.

```text
S1#
```

is Privileged EXEC mode.

Privileged EXEC mode provides greater administrative access.

---

# Part 10 - Inspect the Current Configuration

# Activity 17 - View the Running Configuration

At:

```text
S1#
```

run:

```text
show running-config
```

Read through the configuration.

Look for the VTY configuration.

Look for a readable password such as:

```text
password cisco
```

---

# Security Question

Why is a readable password in the configuration a security problem?

```text
Attacker
   |
   | Gains Access to Configuration
   v
show running-config
   |
   v
Readable Password
```

A person who gains access to the configuration may discover the password.

---

# Part 11 - Protect Plain-Text Passwords

# Activity 18 - Enter Global Configuration Mode

Run:

```text
configure terminal
```

You should see:

```text
S1(config)#
```

---

# Activity 19 - Enable Password Encryption

Run:

```text
service password-encryption
```

---

# What Does `service password-encryption` Do?

The command protects certain plain-text passwords displayed in the Cisco configuration by converting them into an encoded representation.

Before:

```text
password cisco
```

After enabling password encryption, the stored representation changes.

```text
Readable Password
       |
       v
service password-encryption
       |
       v
Encoded Password Representation
```

---

# Important Security Note

Do not confuse:

```text
service password-encryption
```

with strong modern password hashing or encryption.

The main concept for this lab is:

> Passwords should not be unnecessarily displayed as directly readable plain text.

---

# Activity 20 - Verify Password Encryption

Run:

```text
end
```

Then:

```text
show running-config
```

Inspect the VTY password again.

---

# Student Observation

Can you still see:

```text
password cisco
```

in exactly the same readable form?

```text
Answer:

____________________________________________________

____________________________________________________
```

---

# Part 12 - Replace Telnet with SSH

We have improved the stored password representation.

However, the remote connection is still:

```text
Telnet
```

The problem remains:

```text
Telnet
   |
   v
Insecure Remote Communication
```

We want:

```text
SSH
 |
 v
Encrypted Remote Communication
```

---

# What Does SSH Need?

The SSH configuration requires:

```text
Hostname
   +
Domain Name
   +
RSA Keys
   +
Local User
   +
VTY Configuration
        |
        v
SSH Remote Access
```

We already configured:

```text
Hostname = S1
```

Next, configure the domain name.

---

# Part 13 - Configure the Domain Name

# Activity 21 - Enter Configuration Mode

Run:

```text
configure terminal
```

---

# Activity 22 - Configure the Domain Name

Run:

```text
ip domain-name netacad.pka
```

---

# What is a Domain Name?

A domain name identifies a network domain.

Examples:

```text
company.com
university.ac.uk
netacad.pka
```

For this lab:

```text
netacad.pka
```

The switch now has:

```text
Hostname: S1

Domain Name: netacad.pka
```

Conceptually:

```text
S1.netacad.pka
```

The hostname and domain name are required before generating RSA keys.

---

# Part 14 - Understand RSA

RSA is a public-key cryptographic algorithm.

For this lab, you do not need to manually calculate RSA.

You need to understand its role.

SSH requires cryptographic key material.

```text
RSA Keys
    |
    v
SSH Configuration
    |
    v
Secure Remote Communication
```

Think of RSA keys as part of the cryptographic setup required to protect the SSH connection.

---

# Activity 23 - Generate RSA Keys

At:

```text
S1(config)#
```

run:

```text
crypto key generate rsa
```

The switch will ask for a modulus size.

Enter:

```text
1024
```

---

# RSA Modulus Size

The lab requires:

```text
1024 bits
```

For this Packet Tracer activity, use:

```text
1024
```

The key idea is:

```text
RSA Key Pair
      |
      v
Used by SSH
      |
      v
Secure Remote Access
```

---

# Part 15 - Create a Local SSH User

SSH must authenticate the person trying to connect.

We will create a user account on S1.

---

# What is a Local User?

A local user is an account stored on the Cisco device.

Our account is:

```text
Username: administrator

Password: cisco
```

The authentication workflow is:

```text
SSH User
   |
   | Username + Password
   v
S1
   |
   | Check Local User Database
   v
Allow or Reject Login
```

---

# Activity 24 - Create the Administrator User

Run:

```text
username administrator secret cisco
```

---

# Break Down the Command

## `username`

Creates a local user account.

## `administrator`

The username.

## `secret`

Stores the credential as a protected secret.

## `cisco`

The password required by the lab.

---

# SSH Login Details

Remember:

```text
Username: administrator

Password: cisco
```

---

# Part 16 - Configure VTY Lines for SSH

The current VTY configuration allows Telnet.

We need to change it.

Our goal is:

```text
Telnet
   |
   X
Blocked

SSH
 |
 v
Allowed
```

---

# Activity 25 - Enter VTY Configuration

Run:

```text
line vty 0 15
```

You should see:

```text
S1(config-line)#
```

---

# Activity 26 - Use Local Authentication

Run:

```text
login local
```

---

# What Does `login local` Mean?

The command:

```text
login local
```

tells the switch:

> Use the local username database for remote authentication.

We created:

```text
username administrator secret cisco
```

Therefore:

```text
SSH Login
    |
    v
login local
    |
    v
Check Local User Database
    |
    v
administrator / cisco
```

---

# Activity 27 - Allow SSH Only

Run:

```text
transport input ssh
```

---

# What Does `transport input ssh` Mean?

The command means:

> Only accept incoming SSH connections on the VTY lines.

The result is:

```text
Telnet
   |
   X
Rejected

SSH
 |
 v
Accepted
```

---

# Activity 28 - Remove the Old VTY Password

Run:

```text
no password
```

The old VTY line password is no longer required because we are now using:

```text
login local
```

with the local user:

```text
administrator
```

---

# Final VTY Configuration

```text
line vty 0 15
login local
transport input ssh
no password
```

---

# Final Authentication Architecture

```text
PC1
 |
 | SSH Connection
 v
S1 VTY Lines
 |
 | transport input ssh
 v
SSH Allowed
 |
 | login local
 v
Local User Database
 |
 | administrator / secret
 v
Authenticated Session
```

---

# Part 17 - Save the SSH Configuration

# Activity 29 - Exit Configuration Mode

Run:

```text
end
```

You should see:

```text
S1#
```

---

# Activity 30 - Save the Configuration

Run:

```text
copy running-config startup-config
```

Press Enter when prompted.

---

# Part 18 - Verify the Security Configuration

Never assume a security configuration works.

We need to test it.

Expected result:

```text
Telnet = FAIL

SSH = SUCCESS
```

We changed a security control.

Now we verify the control.

---

# Activity 31 - Exit the Existing Telnet Session

If you are still connected through Telnet, run:

```text
exit
```

Return to:

```text
PC1 Command Prompt
```

---

# Activity 32 - Test Telnet Again

Run:

```text
telnet 10.10.10.2
```

---

# Expected Result

The Telnet connection should fail.

---

# Why Does Telnet Fail?

We configured:

```text
transport input ssh
```

This tells the VTY lines to accept:

```text
SSH only
```

Therefore:

```text
Telnet Request
      |
      v
VTY Lines
      |
      | transport input ssh
      v
Telnet Rejected
```

---

# Security Principle

> If an insecure service is not required, disable it.

We did not simply add SSH and leave Telnet enabled.

We configured:

```text
SSH Only
```

---

# Part 19 - Connect Using SSH

# Activity 33 - View SSH Command Syntax

At the PC1 Command Prompt, enter:

```text
ssh
```

Press Enter.

Packet Tracer displays the SSH command syntax.

Important:

```text
-l
```

uses the letter:

```text
L
```

It is not the number:

```text
1
```

---

# Activity 34 - Connect to S1 Using SSH

Run:

```text
ssh -l administrator 10.10.10.2
```

---

# Break Down the Command

## `ssh`

Starts the Secure Shell client.

## `-l`

Specifies the login username.

## `administrator`

The local user created on S1.

## `10.10.10.2`

The management IP address of S1.

The complete command means:

> Start an SSH connection to 10.10.10.2 and log in as administrator.

---

# Activity 35 - Enter the SSH Password

When prompted, enter:

```text
cisco
```

The SSH connection should succeed.

You should see:

```text
S1>
```

---

# What Just Happened?

```text
PC1
 |
 | ssh -l administrator 10.10.10.2
 v
S1
 |
 | SSH Request
 v
VTY Lines
 |
 | SSH Allowed
 v
Local Authentication
 |
 | Check administrator
 v
Password Verified
 |
 v
S1>
```

---

# Activity 36 - Enter Privileged EXEC Mode

Run:

```text
enable
```

Enter:

```text
cisco
```

You should reach:

```text
S1#
```

---

# Activity 37 - Save the Final Configuration

Run:

```text
copy running-config startup-config
```

Press Enter when prompted.

---

# Part 20 - Final Verification

| Test                    | Expected Result |
| ----------------------- | --------------- |
| Ping S1 from PC1        | Success         |
| Telnet to S1            | Fail            |
| SSH to S1               | Success         |
| S1 management IP        | 10.10.10.2      |
| PC1 IP                  | 10.10.10.10     |
| SSH username            | administrator   |
| SSH password            | cisco           |
| Allowed remote protocol | SSH only        |

---

# Student Task 1 - Explain the Security Improvement

Complete the table.

| Before Configuration       | After Configuration        |
| -------------------------- | -------------------------- |
| Remote access used Telnet  | __________________________ |
| Communication was insecure | __________________________ |
| VTY used a line password   | __________________________ |
| Telnet was accepted        | __________________________ |
| No local SSH administrator | __________________________ |

---

# Student Task 2 - Explain the Architecture

Explain the following workflow:

```text
PC1
 |
 | SSH Request
 v
S1 VLAN 1
 |
 | 10.10.10.2
 v
VTY Lines
 |
 | SSH Only
 v
Local Authentication
 |
 | administrator
 v
Cisco CLI
```

Answer:

1. Why does S1 need an IP address?
2. Why is the IP address configured on VLAN 1?
3. What is an SVI?
4. What do VTY lines provide?
5. Why should SSH replace Telnet?
6. What does `login local` do?
7. What does `transport input ssh` do?
8. Why are RSA keys generated?
9. Why does Telnet fail after the configuration change?
10. Why does SSH succeed?

---

# Student Task 3 - Troubleshooting Challenge

A student runs:

```text
ssh -l administrator 10.10.10.2
```

The SSH connection fails.

You are the network engineer.

Troubleshoot in the correct order.

---

## Step 1 - Check PC1

Run:

```text
ipconfig
```

Does PC1 have:

```text
10.10.10.10
```

and:

```text
255.255.255.0
```

---

## Step 2 - Check Connectivity

Run:

```text
ping 10.10.10.2
```

Can PC1 reach S1?

---

## Step 3 - Check VLAN 1

On S1, run:

```text
show ip interface brief
```

Does VLAN 1 have:

```text
10.10.10.2
```

Is the interface operational?

---

## Step 4 - Check the Domain Name

Run:

```text
show running-config
```

Find:

```text
ip domain-name netacad.pka
```

---

## Step 5 - Check the Local User

Find:

```text
username administrator
```

---

## Step 6 - Check the VTY Lines

Confirm:

```text
login local
transport input ssh
```

---

# Troubleshooting Logic

Do not randomly enter commands.

Use:

```text
Physical Connection
        |
        v
IP Configuration
        |
        v
Ping
        |
        v
Switch Management Interface
        |
        v
SSH Configuration
        |
        v
Authentication
```

---

# Student Challenge - Configure SSH Without Looking at the Guide

Create or reset a switch if instructed by your seminar tutor.

Use the following requirements:

```text
Switch Hostname: S1

Domain Name: netacad.pka

Switch Management IP: 10.10.10.2

Subnet Mask: 255.255.255.0

PC IP: 10.10.10.10

SSH Username: administrator

SSH Password: cisco

RSA Modulus: 1024
```

The final result must be:

```text
Ping = Success

Telnet = Fail

SSH = Success
```

---

# Commands Used in This Lab

## Cisco CLI Modes

```text
enable
configure terminal
end
exit
```

## Configure Hostname

```text
hostname S1
```

## Configure VLAN 1

```text
interface vlan 1
ip address 10.10.10.2 255.255.255.0
no shutdown
```

## Verify Interfaces

```text
show ip interface brief
```

## View Running Configuration

```text
show running-config
```

## Save Configuration

```text
copy running-config startup-config
```

## Configure Enable Secret

```text
enable secret cisco
```

## Initial Telnet Configuration

```text
line vty 0 15
password cisco
login
transport input telnet
```

## Protect Plain-Text Passwords

```text
service password-encryption
```

## Configure Domain Name

```text
ip domain-name netacad.pka
```

## Generate RSA Keys

```text
crypto key generate rsa
```

Enter:

```text
1024
```

## Create Local User

```text
username administrator secret cisco
```

## Configure SSH-Only VTY Access

```text
line vty 0 15
login local
transport input ssh
no password
```

## Test Connectivity

```text
ping 10.10.10.2
```

## Test Telnet

```text
telnet 10.10.10.2
```

## Test SSH

```text
ssh -l administrator 10.10.10.2
```

---

# Full Initial Switch Configuration

Do not copy these commands without understanding them.

```text
enable
configure terminal
hostname S1

interface vlan 1
ip address 10.10.10.2 255.255.255.0
no shutdown
exit

enable secret cisco

line vty 0 15
password cisco
login
transport input telnet

end
copy running-config startup-config
```

---

# Full SSH Security Configuration

After observing the Telnet configuration:

```text
configure terminal

service password-encryption

ip domain-name netacad.pka

crypto key generate rsa
```

Enter:

```text
1024
```

Continue:

```text
username administrator secret cisco

line vty 0 15
login local
transport input ssh
no password

end

copy running-config startup-config
```

---

# Important Concepts from Today's Lab

## IP Address

Identifies a device on an IP network.

```text
10.10.10.10
```

## Subnet Mask

Identifies the local IP network.

```text
255.255.255.0
```

## VLAN

Virtual Local Area Network.

For this activity:

```text
VLAN 1
```

is used for switch management.

## SVI

Switch Virtual Interface.

```text
interface vlan 1
```

A logical interface on the switch.

## Management IP Address

An IP address used to communicate with and remotely manage a network device.

For S1:

```text
10.10.10.2
```

## Ping

Tests IP connectivity.

```text
ping 10.10.10.2
```

## ICMP

Internet Control Message Protocol.

Ping normally uses ICMP Echo Request and Echo Reply.

## Telnet

An insecure remote terminal protocol.

```text
Telnet = Insecure Remote Access
```

## SSH

Secure Shell.

```text
SSH = Encrypted Remote Access
```

## Plain Text

Information that is directly readable.

```text
password cisco
```

## RSA

A public-key cryptographic algorithm.

RSA key generation is part of the SSH configuration used in this lab.

## Local User

A user account stored on the Cisco switch.

```text
administrator
```

## VTY Lines

Virtual terminal lines used for remote CLI connections.

```text
line vty 0 15
```

## `login local`

Use the local username database for authentication.

## `transport input ssh`

Allow incoming SSH remote connections only.

## Running Configuration

The currently active configuration.

```text
running-config
```

## Startup Configuration

The configuration loaded during device startup.

```text
startup-config
```

---

# Final Security Architecture

```text
                  NETWORK ADMINISTRATOR
                           |
                           v
                          PC1
                     10.10.10.10
                           |
                           |
                    SSH CONNECTION
                           |
                           v
                   +----------------+
                   |       S1       |
                   |   10.10.10.2   |
                   |     VLAN 1     |
                   +----------------+
                           |
                           v
                       VTY LINES
                           |
                  transport input ssh
                           |
                           v
                     SSH ONLY ACCESS
                           |
                           v
                       login local
                           |
                           v
                  LOCAL USER DATABASE
                           |
                           v
                     administrator
                           |
                           v
                  AUTHENTICATED CLI
```

---

# Final Summary

Today we started with a blank Packet Tracer workspace.

We selected:

```text
PC
```

and:

```text
Cisco 2960 Switch
```

We created:

```text
PC1
```

and:

```text
S1
```

We connected:

```text
PC1 FastEthernet0
```

to:

```text
S1 FastEthernet0/1
```

using:

```text
Copper Straight-Through
```

We configured PC1 with:

```text
10.10.10.10
255.255.255.0
```

We configured S1 VLAN 1 with:

```text
10.10.10.2
255.255.255.0
```

We verified connectivity using:

```text
ping
```

We configured Telnet and observed why insecure remote management is a security concern.

We inspected the running configuration.

We used:

```text
service password-encryption
```

to protect plain-text password representations.

We configured:

```text
netacad.pka
```

as the domain name.

We generated RSA keys.

We created:

```text
administrator
```

as a local user.

We configured:

```text
login local
```

and:

```text
transport input ssh
```

Finally:

```text
Telnet = Failed

SSH = Succeeded
```

The main security lesson is:

```text
Do not simply make remote access work.

Make remote access secure.
```

---

# End of Week 8 Lab

By completing this activity, you have built a small network from scratch, configured a Cisco switch management interface, tested IP connectivity, observed insecure Telnet remote access, and replaced Telnet with SSH for secure remote management.
::: 
