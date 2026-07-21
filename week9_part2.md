# COM398 Systems Security

# Week 9 – Part 2

## Router Configuration and Network Verification

At this stage:

- All PCs have been configured.
- The DNS Server has been configured.
- The physical topology has been connected.

The next step is to configure the routers so that every network can communicate correctly.

---

# Step 1 – Open Router 2

Click:

```
Router2
```

↓

Select

```
CLI
```

When prompted:

```
Would you like to enter the initial configuration dialog?
```

Type

```text
no
```

Press Enter until you see

```text
Router>
```

---

# Step 2 – Enter Privileged Mode

```text
enable
```

You should now see

```text
Router#
```

---

# Step 3 – Enter Global Configuration Mode

```text
configure terminal
```

You should now see

```text
Router(config)#
```

---

# Step 4 – Configure GigabitEthernet0/0

This interface connects Router2 to Switch1.

Enter:

```text
interface g0/0
```

Assign the IP address.

```text
ip address 192.168.10.1 255.255.255.0
```

Enable the interface.

```text
no shutdown
```

Return.

```text
exit
```

---

# Step 5 – Configure GigabitEthernet0/1

Enter

```text
interface g0/1
```

Configure

```text
ip address 192.168.11.1 255.255.255.0
```

Enable

```text
no shutdown
```

Return

```text
exit
```

---

# Step 6 – Verify Router2

Run

```text
end
```

then

```text
show ip interface brief
```

Verify both interfaces show

```text
up
up
```

If an interface is still down, return to configuration mode and ensure you entered

```text
no shutdown
```

---

# Step 7 – Configure Router3

Select

```
Router3
```

↓

CLI

Enter

```text
enable
```

```text
configure terminal
```

---

# Step 8 – Configure GigabitEthernet0/0

Enter

```text
interface g0/0
```

Configure

```text
ip address 192.168.30.1 255.255.255.0
```

Enable

```text
no shutdown
```

Exit

```text
exit
```

---

# Step 9 – Configure GigabitEthernet0/1

Enter

```text
interface g0/1
```

Configure

```text
ip address 192.168.31.1 255.255.255.0
```

Enable

```text
no shutdown
```

Exit

```text
exit
```

---

# Step 10 – Verify Router3

Run

```text
end
```

then

```text
show ip interface brief
```

Verify

```text
GigabitEthernet0/0

up

up
```

and

```text
GigabitEthernet0/1

up

up
```

---

# Step 11 – Verify Physical Connectivity

If everything has been configured correctly:

- Router interfaces should show green.
- Switch links should show green.
- PC links should show green.
- DNS Server link should show green.

If any link is red:

- Check the cable.
- Check the interface.
- Verify the IP address.
- Ensure `no shutdown` has been entered.

---

# Step 12 – Verify PC Configuration

Open each PC.

Select

```
Desktop
```

↓

```
IP Configuration
```

Verify the following.

| Device | IP Address | Subnet Mask | Default Gateway |
|---------|------------|-------------|-----------------|
| PC1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1 |
| PC3 | 192.168.11.10 | 255.255.255.0 | 192.168.11.1 |
| PC4 | 192.168.30.10 | 255.255.255.0 | 192.168.30.1 |
| DNS Server | 192.168.31.10 | 255.255.255.0 | 192.168.31.1 |

---

# Step 13 – Test Connectivity

Open

```
PC1
```

↓

Desktop

↓

Command Prompt

Run

```text
ping 192.168.10.20
```

Expected:

```
Success
```

Run

```text
ping 192.168.11.10
```

Observe whether communication succeeds.

Repeat similar tests from:

- PC2
- PC3
- PC4

Record your observations.

---

# Step 14 – Verify Router Configuration

On Router2

Run

```text
show running-config
```

Verify:

- Both interfaces have IP addresses.
- Interfaces are not shutdown.

Repeat on Router3.

---

# Step 15 – Save the Configuration

On both routers execute

```text
copy running-config startup-config
```

When prompted

```text
Destination filename?
```

Press

```
Enter
```

---

# Ready for ACL Configuration

Your network is now fully configured.

At this stage you should have:

- Router2 configured.
- Router3 configured.
- All interfaces active.
- PCs configured.
- DNS Server configured.
- Connectivity verified.
- Router configurations saved.

You are now ready to begin the Access Control List (ACL) activities provided in the Week 9 lab.
