# COM398 Systems Security
# Week 8 - Configuring Secure Remote Access with SSH

---

# Introduction

In this lab, we will configure secure remote access to a Cisco switch using **SSH**.

Before starting the commands, it is important to understand the problem we are solving.

Imagine that a network engineer needs to manage a switch from another computer.

The network engineer does not want to physically walk to the switch every time a configuration must be changed.

Instead, the engineer connects remotely.

A simple remote connection can look like this:

```text
PC1
 |
 | Remote Management Connection
 |
 v
S1 Switch
```

The question is:

> How can the network engineer connect to the switch securely?

This is the main problem we will solve today.

---

# What We Will Do Today

The lab has three main parts:

```text
Part 1
Secure Passwords
      |
      v
Part 2
Configure SSH
      |
      v
Part 3
Verify SSH Access
```

By the end of the lab, we will:

1. Connect to the switch using Telnet.
2. Observe why plain-text passwords are a security problem.
3. Encrypt passwords stored in the switch configuration.
4. Configure an IP domain name.
5. Generate RSA keys.
6. Create a local SSH user.
7. Configure VTY lines for SSH-only remote access.
8. Confirm that Telnet fails.
9. Confirm that SSH succeeds.

---

# Lab Topology

The lab uses the following simple network.

```text
+---------------------+                  +---------------------+
|        PC1          |                  |         S1          |
|                     |                  |       Switch        |
|  10.10.10.10/24     |------------------|  10.10.10.2/24     |
|                     |                  |      VLAN 1         |
+---------------------+                  +---------------------+
```

## Addressing Table

| Device | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| S1 | VLAN 1 | 10.10.10.2 | 255.255.255.0 |
| PC1 | NIC | 10.10.10.10 | 255.255.255.0 |

Both devices are in the same network:

```text
10.10.10.0/24
```

This means PC1 should be able to communicate directly with S1.

---

# Quick Concept Recap

Before we configure SSH, let us understand the main technical terms used in this lab.

---

# What is Remote Management?

Remote management means configuring or controlling a network device from another computer.

Example:

```text
Network Engineer
       |
       v
      PC
       |
       | Remote Connection
       v
    Cisco Switch
```

Instead of physically connecting a console cable every time, the engineer can connect through the network.

---

# What is Telnet?

Telnet is a protocol used for remote access.

It allows a user to connect to a network device and enter commands remotely.

Example:

```text
PC1
 |
 | Telnet
 |
 v
S1
```

The problem is that Telnet sends information in **plain text**.

This may include:

```text
Username
Password
Commands
Configuration Information
```

A person capturing the network traffic may be able to read this information.

## Simple Example

Imagine sending a password on a postcard.

```text
Password: cisco
```

Anyone who sees the postcard can read it.

Telnet works in a similar way because the communication is not securely encrypted.

---

# What is SSH?

SSH stands for:

```text
Secure Shell
```

SSH is also used for remote management.

However, SSH encrypts the communication between two devices.

```text
PC1
 |
 | Encrypted SSH Connection
 |
 v
S1
```

An attacker may capture the traffic, but the transmitted information is encrypted.

## Telnet vs SSH

| Feature | Telnet | SSH |
|---|---|---|
| Remote access | Yes | Yes |
| Encryption | No | Yes |
| Plain-text communication | Yes | No |
| Recommended for secure management | No | Yes |

The key idea for today's lab is:

```text
Telnet = Insecure Remote Access

SSH = Secure Remote Access
```

---

# What is Plain Text?

Plain text means information that can be read directly.

Example:

```text
password cisco
```

If a password is visible directly in the configuration, anyone who gains access to the configuration may be able to read it.

---

# What is Password Encryption?

Password encryption changes readable password information into a less readable stored form.

Example:

```text
Before

password cisco
```

After password encryption, the configuration may show an encrypted representation instead of the original plain-text value.

In this lab, we will use:

```text
service password-encryption
```

The purpose of this command is to encrypt plain-text passwords displayed in the Cisco configuration.

---

# Important Security Note

The password encryption used by `service password-encryption` is not the same as strong modern cryptographic protection.

For this lab, the important concept is:

> Passwords should not be unnecessarily displayed as readable plain text in the device configuration.

---

# What is a Domain Name?

A domain name identifies a network domain.

Examples include:

```text
company.com
university.ac.uk
netacad.pka
```

In this lab, we configure:

```text
netacad.pka
```

The Cisco switch requires a hostname and domain name before RSA keys can be generated for SSH.

---

# What is RSA?

RSA is a public-key cryptographic algorithm.

For this lab, you only need to understand one main idea:

> SSH requires cryptographic keys to protect the remote connection.

We will generate RSA keys on the switch.

The lab requires a key length of:

```text
1024 bits
```

The command is:

```text
crypto key generate rsa
```

When prompted for the key size, enter:

```text
1024
```

---

# What is a Local User?

A local user is an account stored on the Cisco switch.

Example:

```text
Username: administrator
Password: cisco
```

The switch can check these local credentials when someone tries to connect using SSH.

The command we will use is:

```text
username administrator secret cisco
```

---

# What are VTY Lines?

VTY stands for:

```text
Virtual Teletype
```

VTY lines are used for remote terminal connections to Cisco devices.

Think of them as virtual remote access channels.

```text
Remote User
     |
     v
VTY Lines
     |
     v
Cisco Switch CLI
```

In this lab, we configure:

```text
line vty 0 15
```

This means we are configuring VTY lines 0 through 15.

---

# What Does `login local` Mean?

The command:

```text
login local
```

tells the switch:

> Use the local username database to check login credentials.

Therefore, the switch checks the username created using:

```text
username administrator secret cisco
```

---

# What Does `transport input ssh` Mean?

The command:

```text
transport input ssh
```

means:

> Only allow SSH for incoming remote connections.

Telnet is no longer accepted.

The expected result is:

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

# Lab Workflow

The complete configuration workflow is:

```text
PC1
 |
 | Telnet to S1
 v
Observe Current Configuration
 |
 v
Encrypt Plain-Text Passwords
 |
 v
Configure Domain Name
 |
 v
Generate RSA Keys
 |
 v
Create Local User
 |
 v
Configure VTY Lines
 |
 v
Allow SSH Only
 |
 v
Test Telnet
Expected: Fail
 |
 v
Test SSH
Expected: Success
```

---

# Part 1 - Secure Passwords

---

# Activity 1 - Connect to S1 using Telnet

## Objective

The first step is to use the existing Telnet configuration.

This lets us observe the insecure remote access method before replacing it with SSH.

---

## Step 1

Open **PC1** in Packet Tracer.

Select:

```text
Desktop
```

Then open:

```text
Command Prompt
```

---

## Step 2

Run:

```text
telnet 10.10.10.2
```

The destination address is the VLAN 1 IP address of S1.

```text
S1 VLAN 1 = 10.10.10.2
```

---

## Step 3

When prompted for the password, enter:

```text
cisco
```

You should reach the switch user EXEC mode.

Example:

```text
S1>
```

---

## Step 4

Enter privileged EXEC mode.

Run:

```text
enable
```

Enter the password:

```text
cisco
```

You should now see:

```text
S1#
```

---

# Question

What is the difference between:

```text
S1>
```

and

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

Privileged EXEC mode provides access to more administrative commands.

---

# Activity 2 - Save the Current Configuration

## Why?

Before making changes, we should save the working configuration.

This is a basic network administration habit.

Run:

```text
copy running-config startup-config
```

Press Enter if asked to confirm the destination filename.

You may also use:

```text
write memory
```

The purpose is to save the current configuration.

---

# Running Configuration vs Startup Configuration

## Running Configuration

The configuration currently active in memory.

```text
running-config
```

## Startup Configuration

The configuration loaded when the device starts.

```text
startup-config
```

Think of it as:

```text
Running Configuration
        |
        | Save
        v
Startup Configuration
```

---

# Activity 3 - Inspect the Current Configuration

Run:

```text
show running-config
```

Look through the configuration.

Find password-related configuration.

The lab asks you to observe that some passwords are displayed in plain text.

---

# Question

Why is storing a password in plain text a security concern?

## Expected Answer

Anyone who gains access to the configuration may be able to read the password directly.

---

# Activity 4 - Encrypt Plain-Text Passwords

Enter global configuration mode.

Run:

```text
configure terminal
```

You should see:

```text
S1(config)#
```

Now run:

```text
service password-encryption
```

---

## What Did We Just Do?

The command instructs Cisco IOS to encrypt plain-text passwords stored in the configuration.

```text
Plain-Text Password
        |
        v
service password-encryption
        |
        v
Encrypted Representation
```

---

# Activity 5 - Verify Password Encryption

Exit configuration mode.

Run:

```text
end
```

Now run:

```text
show running-config
```

Inspect the password configuration again.

---

# Question

Can you still see the password `cisco` in the same plain-text form?

Record your observation.

```text
Answer:

____________________________________________________

____________________________________________________
```

---

# Part 2 - Encrypt Communications

We have now improved how passwords appear in the configuration.

However, there is still another problem.

We are using:

```text
Telnet
```

Telnet transfers remote management traffic without strong encryption.

The next objective is to replace Telnet with SSH.

---

# Activity 6 - Configure the Domain Name

Enter global configuration mode.

```text
configure terminal
```

Configure the domain name required by the lab:

```text
ip domain-name netacad.pka
```

---

## What Does This Command Do?

It configures the domain name:

```text
netacad.pka
```

The switch now has the required identity information for RSA key generation.

---

# Activity 7 - Generate RSA Keys

Run:

```text
crypto key generate rsa
```

The switch will ask for the modulus size.

Enter:

```text
1024
```

---

## What is the RSA Key Length?

The lab requires:

```text
1024 bits
```

For today's activity, remember:

```text
RSA Keys
     |
     v
Used by SSH
     |
     v
Help Secure Remote Communication
```

---

# Activity 8 - Create the SSH Administrator User

Run:

```text
username administrator secret cisco
```

---

## Break Down the Command

```text
username
```

Create a local user.

```text
administrator
```

The username.

```text
secret
```

Store the password as a protected secret.

```text
cisco
```

The password required by the lab.

---

# Current SSH Login Details

```text
Username: administrator

Password: cisco
```

Remember these values because we will use them when testing SSH.

---

# Activity 9 - Configure the VTY Lines

Run:

```text
line vty 0 15
```

You should now see:

```text
S1(config-line)#
```

---

## Configure Local Authentication

Run:

```text
login local
```

This tells the switch to use the local username database.

The switch will check:

```text
administrator
```

and the associated secret.

---

## Allow SSH Only

Run:

```text
transport input ssh
```

The switch now accepts SSH remote connections only.

---

## Remove the Existing VTY Password

Run:

```text
no password
```

---

# Final VTY Configuration

The important commands are:

```text
line vty 0 15
login local
transport input ssh
no password
```

---

# What Does the Configuration Mean?

```text
Remote Connection
       |
       v
VTY Lines
       |
       v
SSH Only
       |
       v
Local Username Check
       |
       v
administrator / cisco
```

---

# Activity 10 - Exit and Save the Configuration

Run:

```text
end
```

Save the configuration:

```text
copy running-config startup-config
```

Press Enter when prompted.

---

# Part 3 - Verify SSH Implementation

Now we test whether the security configuration works.

The expected behaviour is:

```text
Telnet = Fail

SSH = Success
```

---

# Activity 11 - Exit the Existing Telnet Session

If you are still connected through Telnet, run:

```text
exit
```

Return to the PC1 command prompt.

---

# Activity 12 - Test Telnet Again

Run:

```text
telnet 10.10.10.2
```

---

## Expected Result

The Telnet connection should fail.

---

# Question

Why does Telnet fail now?

## Expected Answer

The VTY lines were configured using:

```text
transport input ssh
```

Therefore, only SSH remote connections are allowed.

---

# Activity 13 - View the SSH Command Syntax

At the PC1 Command Prompt, type:

```text
ssh
```

Press Enter.

Packet Tracer should display the SSH command usage information.

The lab gives an important hint:

> The `-l` option is the letter L, not the number 1.

---

# Activity 14 - Connect using SSH

Run:

```text
ssh -l administrator 10.10.10.2
```

---

## Break Down the Command

```text
ssh
```

Start an SSH connection.

```text
-l
```

Specify the login username.

```text
administrator
```

The local username created on S1.

```text
10.10.10.2
```

The IP address of S1.

---

## Enter the Password

When prompted, enter:

```text
cisco
```

---

## Expected Result

The SSH login should succeed.

You should see:

```text
S1>
```

---

# Activity 15 - Enter Privileged EXEC Mode

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

# Activity 16 - Save the Final Configuration

Run:

```text
copy running-config startup-config
```

Press Enter when prompted.

---

# Final Verification

You should now have the following result:

| Test | Expected Result |
|---|---|
| Telnet to S1 | Fail |
| SSH to S1 | Success |
| SSH username | administrator |
| SSH password | cisco |
| S1 IP address | 10.10.10.2 |
| Allowed remote protocol | SSH only |

---

# Student Task - Explain the Security Improvement

Complete the following comparison.

| Before Configuration | After Configuration |
|---|---|
| Remote access used Telnet | __________________________ |
| Communication was plain text | __________________________ |
| VTY access used a line password | __________________________ |
| Telnet was allowed | __________________________ |

---

# Packet Tracer Challenge

Without looking at the commands above, explain the following security workflow:

```text
PC1
 |
 | Remote Access Request
 v
S1 VTY Lines
 |
 | SSH Only
 v
Local Username Database
 |
 | administrator / secret
 v
Authenticated SSH Session
```

Answer these questions:

1. Why should SSH replace Telnet?
2. Why are RSA keys required?
3. What does `login local` do?
4. What does `transport input ssh` do?
5. Why did the second Telnet test fail?
6. Why did the SSH test succeed?

---

# Commands Used in This Lab

## Privileged EXEC and Configuration

```text
enable
configure terminal
end
```

## Save Configuration

```text
copy running-config startup-config
```

## View Configuration

```text
show running-config
```

## Password Encryption

```text
service password-encryption
```

## SSH Preparation

```text
ip domain-name netacad.pka
crypto key generate rsa
```

RSA modulus:

```text
1024
```

## Create Local User

```text
username administrator secret cisco
```

## Configure VTY Lines

```text
line vty 0 15
login local
transport input ssh
no password
```

## Telnet Test

```text
telnet 10.10.10.2
```

## SSH Test

```text
ssh -l administrator 10.10.10.2
```

---

# Final Summary

```text
Telnet
   |
   v
Plain-Text Remote Communication
   |
   v
Security Risk
```

We replace it with:

```text
SSH
 |
 v
Encrypted Remote Communication
 |
 v
RSA Keys
 |
 v
Local User Authentication
 |
 v
Secure Switch Management
```

The key concepts from today's lab are:

```text
Telnet = Insecure Remote Access

SSH = Secure Remote Access

RSA Keys = Cryptographic Keys Required for SSH

Local User = Account Stored on the Switch

VTY Lines = Remote Access Lines

login local = Use Local User Database

transport input ssh = Allow SSH Only
```

---

# End of Week 8 Lab

By completing this lab, you have configured a Cisco switch to use SSH for secure remote management and verified that insecure Telnet access is no longer accepted.
