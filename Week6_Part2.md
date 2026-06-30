# Week 6 – Seminar Activities

---

# Activity 1 – Identify the Active Network Interface

## Objective

Before capturing packets, we need to identify **which network interface is currently connected to the Internet**.

A computer may have multiple interfaces:

- Ethernet
- Wi-Fi
- Loopback
- VPN
- Bluetooth

Wireshark captures packets from a specific interface, so choosing the correct one is important.

---

## Step 1

Open Terminal.

Run:

```bash
ifconfig
```

Look for an interface that contains:

```
status: active

inet xxx.xxx.xxx.xxx
```

Example:

```
en1

inet 192.168.1.73

status: active
```

This means

- Interface = en1
- IP Address = 192.168.1.73
- Active = Yes

This is the interface we will monitor.

---

## Discussion

Ask students:

> Why shouldn't we capture on an inactive interface?

Expected answer:

No packets are travelling through that interface.

---

# Activity 2 – Open Wireshark

Launch Wireshark.

The home page displays all available interfaces.

Select the interface discovered previously.

Example:

```
en1
```

Do NOT select:

- Loopback
- Bluetooth
- Inactive Ethernet
- VPN interfaces

unless instructed.

---

## Discussion

Ask:

> What do you think happens if we capture on the wrong interface?

Expected answer:

No traffic is captured.

---

# Activity 3 – Start Packet Capture

Click

```
Start Capturing Packets
```

Do not apply any capture filters.

Allow Wireshark to begin capturing.

Explain:

Our computer constantly communicates with:

- DNS servers
- Routers
- Background services
- Browsers
- Cloud applications

You should immediately observe packets appearing.

---

## Discussion

Ask students:

> Why are packets already appearing even though we haven't opened a website?

Expected answer:

The operating system constantly communicates across the network.

---

# Activity 4 – Generate Network Traffic

Open Terminal.

Run:

```bash
curl https://example.com
```

or

```bash
curl https://www.uwa.edu.au/
```

This command downloads a webpage without opening a browser.

Explain:

We need traffic before we can analyse packets.

Think of curl as creating a conversation that Wireshark can capture.

---

Return to Wireshark.

Stop the capture.

---

# Activity 5 – Display Filters

Wireshark has now captured many packets.

Instead of scrolling through hundreds of packets, we use Display Filters.

Remember:

Capture Filter

↓

Limits what Wireshark captures.

Display Filter

↓

Limits what Wireshark displays.

---

Filter only IPv4 packets.

```
ip.version == 4
```

Press Enter.

Only IPv4 packets remain.

---

Filter TCP packets.

```
tcp
```

---

Filter UDP packets.

```
udp
```

---

Filter DNS packets.

```
dns
```

---

## Discussion

Ask:

Why is filtering useful?

Expected answer:

Large captures contain thousands of packets.

Filtering makes analysis easier.

---

# Activity 6 – Explore an IPv4 Packet

Choose any packet that shows:

```
Protocol

TCP

or

TLS
```

Expand

```
Internet Protocol Version 4
```

You will now see the IPv4 Header.

Explain each field.

---

## Version

Shows:

```
Version: 4
```

Meaning:

This packet is using IPv4.

---

## Header Length

Usually:

```
20 Bytes
```

This tells us the size of the IPv4 Header.

The header contains networking information.

It does NOT contain the webpage or file itself.

---

## Total Length

Shows:

```
Header

+

Payload

=

Total Length
```

The payload contains the actual information being transmitted.

---

## Source Address

Example

```
192.168.1.73
```

Ask:

Who sent this packet?

Expected answer:

My computer.

---

## Destination Address

Example

```
104.18.5.186
```

Ask:

Who receives the packet?

Expected answer:

Remote server.

---

## Protocol

This field tells IPv4

what protocol exists inside the packet.

Examples:

```
TCP

UDP

ICMP
```

IPv4 acts as the delivery mechanism.

---

# Activity 7 – Understanding TTL

Locate:

```
Time To Live
```

Explain:

TTL stands for

```
Time To Live
```

Although the name suggests time,

TTL actually represents

the number of routers a packet may pass through.

---

Draw:

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

Every router subtracts

```
1
```

from the TTL.

---

## Why?

Suppose two routers accidentally create a routing loop.

```
Router A

↓

Router B

↓

Router A

↓

Router B
```

Without TTL,

the packet would never disappear.

TTL prevents infinite routing loops.

---

## Discussion

Ask:

What happens when TTL becomes zero?

Expected answer:

The router discards the packet.

---

# Activity 8 – Discover the Internet Path

Open Terminal.

Run

Mac

```bash
traceroute www.uwa.edu.au
```

Windows

```cmd
tracert www.uwa.edu.au
```

Example

```
1 192.168.1.254

2 172.16.17.237

3 *

4 62.172.102.232

5 104.18.5.186
```

Explain:

Every line represents one router.

The packet travels through each router before reaching the destination.

---

## What does * mean?

Some routers do not respond to traceroute requests.

This does NOT mean the router failed.

It simply ignored the request.

---

## Discussion

Ask:

How many routers did the packet travel through?

Students count the hops.

---

# Activity 9 – Draw the Internet Path

Students should sketch the route.

Example

```
My Computer

↓

Home Router

↓

ISP

↓

Internet Backbone

↓

Cloudflare

↓

Remote Server
```

Explain

Packets do NOT travel directly.

They pass through many networking devices.

---

# Activity 10 – Header Checksum

Locate

```
Header Checksum
```

Explain:

The checksum allows routers to detect errors in the IPv4 Header.

Think of it like a spelling checker.

If one bit changes,

the checksum changes.

The receiving device knows the header has become corrupted.

Today's objective is simply to identify the field,

not calculate it manually.

---

# Practical Summary

Today we learnt

✔ How to identify an active interface

✔ How to capture packets

✔ How to generate network traffic

✔ How to filter packets

✔ How to inspect IPv4 headers

✔ How routers forward packets

✔ How TTL works

✔ How traceroute discovers network paths

✔ Why IPv4 headers contain checksums

---

# Reflection Questions

1.

What is IPv4?

2.

Why do devices require IP addresses?

3.

What is the purpose of a router?

4.

What information does the IPv4 Header contain?

5.

What happens when TTL reaches zero?

6.

Why do we use traceroute?

7.

What is the difference between Capture Filters and Display Filters?

8.

Why is Header Checksum important?

---

# Key Takeaways

```
Wireshark

↓

Captures Packets

↓

IPv4

↓

Provides Addressing

↓

Routers

↓

Forward Packets

↓

TTL

↓

Prevents Infinite Loops

↓

Traceroute

↓

Shows Network Path
```

---

# End of Week 6

Next week we will continue exploring network communication and security by analysing additional networking protocols and understanding how different layers of the TCP/IP model work together to provide reliable communication across modern computer networks.
