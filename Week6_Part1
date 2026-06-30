# COM398 – Systems Security
# Week 6 – Understanding IPv4 using Wireshark

---

# Module

**Module:** COM398 – Systems Security

**Week:** 6

**Topic:** Internet Protocol Version 4 (IPv4)

---

# Introduction

Last week we were introduced to **Wireshark**, one of the most widely used network analysis tools in Cyber Security.

We learnt how to:

- Install Wireshark
- Capture packets
- Navigate through the Wireshark interface
- Observe packets travelling across a network

However, we did not focus on **what was actually inside those packets**.

Today we move one layer deeper.

Instead of simply capturing packets, we will inspect the **Internet Protocol Version 4 (IPv4)** header and understand how devices communicate across the Internet.

By the end of today's lab, you will understand:

- How devices identify each other on the Internet
- How routers know where packets should travel
- What happens when a packet travels across multiple routers
- Why packets contain information such as TTL, Protocol and Header Checksum
- How Wireshark allows us to inspect all these details

---

# Learning Objectives

By the end of today's lab you should be able to:

- Explain what IPv4 is
- Explain why IP addresses are needed
- Capture IPv4 packets using Wireshark
- Identify important IPv4 header fields
- Explain the purpose of TTL
- Explain packet fragmentation
- Understand how traceroute discovers network paths
- Identify source and destination IP addresses
- Interpret the information displayed in Wireshark

---

# Quick Recap – Wireshark

## What is Wireshark?

Wireshark is a **Network Protocol Analyser**.

Think of Wireshark as a microscope for networks.

Instead of looking at cells, Wireshark allows us to inspect network packets travelling across a computer network.

Whenever your computer communicates with another device, information is broken into small units called packets.

Wireshark captures these packets and allows us to inspect them.

---

## Where is Wireshark Used?

Wireshark is one of the most commonly used networking tools in:

- Cyber Security
- Digital Forensics
- Network Engineering
- Cloud Computing
- SOC (Security Operations Centres)
- Incident Response
- Malware Analysis

Many Cyber Security job interviews expect graduates to understand basic Wireshark usage.

---

# Quick Review of the Wireshark Interface

When Wireshark starts, the interface is divided into three sections.

```
+----------------------------------------------------------+
|                  Packet List                             |
|----------------------------------------------------------|
| Frame  Protocol  Source  Destination                     |
|                                                          |
+----------------------------------------------------------+

+----------------------------------------------------------+
|                  Packet Details                          |
|----------------------------------------------------------|
| Ethernet                                                 |
| Internet Protocol Version 4                              |
| TCP                                                      |
| HTTP                                                     |
+----------------------------------------------------------+

+----------------------------------------------------------+
|                  Packet Bytes                            |
|----------------------------------------------------------|
| 45 00 00 34 8a 9b ...                                    |
+----------------------------------------------------------+
```

### Packet List

The Packet List shows every captured packet.

Each row represents one packet.

Some common protocols you will see include:

- TCP
- UDP
- HTTP
- HTTPS
- DNS
- ARP
- ICMP

---

### Packet Details

The middle section contains all protocol information.

This is where we will spend most of today's lab.

Today's focus will be:

```
Internet Protocol Version 4
```

---

### Packet Bytes

The bottom section contains the actual binary information displayed in hexadecimal.

Networking professionals often inspect these bytes when troubleshooting complex networking problems.

For today's lab, we only need to understand that these bytes represent the packet transmitted across the network.

---

# Today's Question

Suppose you type:

```
www.google.com
```

into your browser.

How does your computer know:

- where Google is?
- where to send the packet?
- how to return the response?

This is where IPv4 becomes important.

---

# What is IPv4?

IPv4 stands for

**Internet Protocol Version 4**

IPv4 is responsible for delivering packets from one computer to another across the Internet.

Think of IPv4 as the postal service of the Internet.

Without IPv4 there is no addressing system.

Without addresses, packets have nowhere to go.

---

# Real World Analogy

Imagine sending a parcel.

The parcel contains:

```
Sender Address

Recipient Address

Instructions

Contents
```

An IPv4 packet works exactly the same way.

```
+--------------------------------------+
| Source Address                       |
| Destination Address                  |
| Instructions                         |
| Data                                 |
+--------------------------------------+
```

Every packet travelling across the Internet follows this basic idea.

---

# Internet Communication

Suppose you visit:

```
www.google.com
```

The communication looks like this.

```
Your Laptop
      │
      ▼
Home Router
      │
      ▼
Internet Service Provider
      │
      ▼
Several Internet Routers
      │
      ▼
Google Server
```

Each router reads the destination IP address and decides where the packet should travel next.

---

# What is an IP Address?

An IP Address uniquely identifies a device connected to a network.

Examples:

```
192.168.1.73

172.16.17.237

104.18.5.186
```

Think of an IP address like a home address.

Without an address:

- deliveries fail
- navigation fails
- communication fails

Similarly,

without an IP address,

Internet communication cannot happen.

---

# Public vs Private IP Addresses

Private IP Addresses are used inside homes and organisations.

Examples:

```
192.168.x.x

172.16.x.x

10.x.x.x
```

Public IP Addresses exist on the Internet.

Example:

```
104.18.5.186
```

In today's traceroute activity, you will observe both.

---

# What is a Router?

A Router connects different networks together.

Its job is simple.

```
Receive Packet

↓

Read Destination Address

↓

Decide Best Route

↓

Forward Packet
```

A router does NOT read your email.

A router simply decides where the packet should travel next.

---

# Understanding a Packet

An IPv4 packet consists of two main parts.

```
+-----------------------------+
| IPv4 Header                 |
+-----------------------------+
| Data (Payload)              |
+-----------------------------+
```

The Header contains networking information.

The Payload contains the actual data.

Examples of payload:

- Web page
- Email
- Image
- Video
- File

---

# The IPv4 Header

Today we will inspect the following fields.

```
Version

Header Length

Total Length

Identification

Flags

Fragment Offset

Time To Live (TTL)

Protocol

Header Checksum

Source Address

Destination Address
```

Each field performs a specific job.

During the practical session we will inspect each one using Wireshark.

---

# Understanding TTL (Time To Live)

TTL is one of the most important IPv4 fields.

Imagine the packet begins with

```
TTL = 64
```

As it travels:

```
Laptop

TTL = 64

↓

Router 1

TTL = 63

↓

Router 2

TTL = 62

↓

Router 3

TTL = 61

↓

Destination
```

Each router subtracts **1**.

---

## Why Does TTL Exist?

Imagine two routers accidentally keep sending the packet to each other forever.

```
Router A

↓

Router B

↓

Router A

↓

Router B

↓

Forever...
```

Without TTL,

that packet would never disappear.

TTL prevents infinite routing loops.

When TTL becomes zero,

the router discards the packet.

---

# What is Traceroute?

Traceroute is a networking tool that discovers the path between two computers.

Example:

```
Your Laptop

↓

Router 1

↓

Router 2

↓

Router 3

↓

Remote Server
```

Traceroute works by sending packets with different TTL values.

Example:

```
TTL = 1

↓

Stops at Router 1

----------------

TTL = 2

↓

Stops at Router 2

----------------

TTL = 3

↓

Stops at Router 3
```

Eventually the destination is reached.

This allows traceroute to identify every router along the path.

---

# Today's Practical

Today we will perform the following activities.

1. Identify our active network interface.
2. Open Wireshark.
3. Capture live network traffic.
4. Generate traffic using Terminal.
5. Filter IPv4 packets.
6. Inspect IPv4 header fields.
7. Understand TTL.
8. Run traceroute.
9. Draw the Internet path.
10. Inspect the IP Header Checksum.

By the end of the session you should be comfortable identifying and explaining the major fields in an IPv4 packet using Wireshark.

---

# Before We Begin

Before opening Wireshark, ask yourself:

- What is an IP address?
- Why do routers exist?
- What information must every packet contain?
- What happens if a packet never reaches its destination?

These are the questions today's practical will answer.

---

**Continue to Part 2**, where we will perform each activity step by step using Wireshark, Terminal, `curl`, and `traceroute`.
