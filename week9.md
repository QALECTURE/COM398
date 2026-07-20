# COM398 Systems Security

# Week 9 – Access Control Lists (ACLs)

---

# Learning Objectives

Today we are going to learn how Access Control Lists (ACLs) are used to control network traffic between different networks.

By the end of today's lab you should be able to:

• Understand what an ACL does
• Identify where an ACL is applied
• Verify whether an ACL is blocking traffic
• Remove an ACL
• Create a Standard IPv4 ACL
• Apply an ACL to the correct interface
• Verify ACL functionality using ping and show commands

---

# Practical Activities

Today's seminar consists of two practical activities.

## Activity 1

Investigating an Existing ACL

You will:

• Verify network connectivity
• Identify why some devices cannot communicate
• Locate the ACL
• Remove the ACL
• Verify connectivity again

---

## Activity 2

Configuring Your Own ACL

You will:

• Create a Standard IPv4 ACL
• Apply it to the correct router interface
• Verify the ACL
• Test the network after applying the ACL

---

# Before You Start

Open Cisco Packet Tracer.

Open the Packet Tracer file supplied with the lab.

Do not modify anything yet.

First, verify the existing topology.

---

# Check the Topology

You should see:

• Router R1
• Router R2
• Router R3

• PC1
• PC2
• PC3

• Web Server

• Three switches

If your topology does not match, ask your seminar tutor before continuing.

---

# Verify the Addressing

Before running any commands, check the addressing table.

## PC Addresses

PC1

192.168.10.10 /24

Gateway

192.168.10.1

---

PC2

192.168.11.10 /24

Gateway

192.168.11.1

---

PC3

192.168.30.10 /24

Gateway

192.168.30.1

---

Web Server

192.168.20.254

Gateway

192.168.20.1

---

# Router Interfaces

R1

G0/0 → 192.168.10.1

G0/1 → 192.168.11.1

---

R2

G0/0 → 192.168.20.1

---

R3

G0/0 → 192.168.30.1

---

# Activity 1

Verify Connectivity Before Looking at ACLs

Remember:

Never troubleshoot an ACL until you know the network itself works.

The first job of a network engineer is always to verify connectivity.

---

# Step 1

Open:

PC1

Desktop

Command Prompt

---

# Step 2

Run the following commands one at a time.

ping 192.168.10.20

ping 192.168.11.10

ping 192.168.30.10

ping 192.168.20.254

Record which pings succeed and which fail.

Do not investigate yet.

---

# Step 3

Repeat the same tests from PC2.

Open

PC2

Desktop

Command Prompt

Run:

ping 192.168.30.10

ping 192.168.20.254

Again, record your observations.

---

# Step 4

Only after completing the ping tests should you investigate the routers.

Select:

R1

CLI

Enter:

enable

Run:

show access-lists

What ACL is configured?

Write down:

ACL Number

ACL Rules

Interfaces affected

---

# Step 5

Run:

show running-config

Locate:

ip access-group

Answer the following:

Which interface is using the ACL?

Is it inbound or outbound?

---

# Step 6

After identifying the ACL, remove it exactly as instructed in the lab sheet.

Verify the configuration again.

---

# Step 7

Repeat every ping.

Did connectivity improve?

Which devices can now communicate?

Record your results before moving to Activity 2.

---

# Activity 2

Configuring Your Own ACL

Open the second Packet Tracer file.

Do not begin typing commands immediately.

First verify:

• PC addresses
• Router addresses
• Web Server address
• Router interfaces

Match them with the addressing table.

---

# Step 1

Open

R2

CLI

Enter privileged mode.

Enter global configuration mode.

---

# Step 2

Create ACL 1 exactly as shown in today's lab.

---

# Step 3

Permit all remaining traffic.

---

# Step 4

Apply the ACL to the correct interface.

---

# Step 5

Repeat the same process on R3.

---

# Step 6

Verify both ACLs.

Run:

show access-lists

show running-config

show ip interface

---

# Step 7

Test the following.

✓ PC1 → PC2

✓ PC1 → Web Server

✓ PC2 → Web Server

✓ PC1 → PC3

✓ PC2 → PC3

✓ PC3 → Web Server

Record:

Pass or Fail

Explain why.

---

# End of Lab Checklist

✓ Verified topology

✓ Verified addressing

✓ Tested connectivity

✓ Located an ACL

✓ Removed an ACL

✓ Configured a Standard ACL

✓ Applied ACLs to router interfaces

✓ Verified ACL configuration

✓ Tested the completed network
