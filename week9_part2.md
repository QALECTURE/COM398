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

---

# Part 3 – Access Control List (ACL) Configuration

Now that the network has been configured and basic connectivity has been verified, we can begin configuring Access Control Lists (ACLs).

In this activity you will investigate an existing ACL, remove it, verify the network, and then configure your own Standard IPv4 ACL.

---

# Activity 1 – Investigating an Existing ACL

## Step 1 – Verify Network Connectivity

Before investigating the routers, first test the network.

Open:

```
PC1
```

↓

Desktop

↓

Command Prompt

Run the following commands one at a time.

```text
ping 192.168.10.20
```

```text
ping 192.168.11.10
```

```text
ping 192.168.30.10
```

```text
ping 192.168.31.10
```

Record which devices respond successfully and which devices fail.

Do not investigate the cause yet.

---

## Step 2 – Repeat the Tests

Repeat the same tests from:

- PC2
- PC3
- PC4

Record your observations.

---

## Step 3 – Investigate Router2

Open:

```
Router2
```

↓

CLI

Enter privileged mode.

```text
enable
```

Display the configured ACLs.

```text
show access-lists
```

Record the following:

- ACL Number
- ACL Type
- ACL Rules

---

## Step 4 – Locate the ACL

Display the running configuration.

```text
show running-config
```

Locate the following command.

```text
ip access-group
```

Record:

- Which interface is using the ACL?
- Is the ACL applied inbound or outbound?

---

## Step 5 – Remove the ACL

Follow the instructions provided in the lab worksheet.

Remove the ACL from the interface.

Verify the configuration again.

```text
show running-config
```

```text
show access-lists
```

---

## Step 6 – Verify Connectivity Again

Repeat all ping tests.

Open:

PC1

↓

Desktop

↓

Command Prompt

Run:

```text
ping 192.168.10.20
```

```text
ping 192.168.11.10
```

```text
ping 192.168.30.10
```

```text
ping 192.168.31.10
```

Compare the results with the previous tests.

Record:

- Which devices can now communicate?
- Which devices are still blocked?

---

# Activity 2 – Configure a Standard IPv4 ACL

Open:

```
Week 9 Lab – ACL Part 2.pkt
```

Verify that all devices and IP addresses match the previous activity before making any changes.

---

## Step 1 – Open Router2

Select:

```
Router2
```

↓

CLI

Enter privileged mode.

```text
enable
```

Enter global configuration mode.

```text
configure terminal
```

---

## Step 2 – Create ACL 1

Create Standard ACL 1 exactly as shown in the lab worksheet.

Use the ACL number provided in the activity.

Configure the required permit and deny statements.

---

## Step 3 – Apply the ACL

Apply the ACL to the interface specified in the lab worksheet.

Ensure that it is applied in the correct direction:

- Inbound
- Outbound

Verify your configuration.

```text
show running-config
```

---

## Step 4 – Configure Router3

Repeat the same process on Router3 if instructed in the activity.

Verify the configuration using:

```text
show access-lists
```

```text
show running-config
```

```text
show ip interface
```

---

## Step 5 – Verify the ACL

Check that:

- The ACL number is correct.
- The ACL contains the correct rules.
- The ACL is applied to the correct interface.
- The ACL direction is correct.

---

## Step 6 – Test the Network

Repeat the connectivity tests.

From PC1 run:

```text
ping 192.168.10.20
```

```text
ping 192.168.11.10
```

```text
ping 192.168.30.10
```

```text
ping 192.168.31.10
```

Repeat similar tests from:

- PC2
- PC3
- PC4

Record whether each test:

- Passed
- Failed

Explain why the ACL allowed or denied the traffic.

---

# Verification Commands

Use the following commands throughout the activity.

Display configured ACLs.

```text
show access-lists
```

Display the running configuration.

```text
show running-config
```

Display interface information.

```text
show ip interface
```

Display interface status.

```text
show ip interface brief
```

---

# End of Practical Checklist

Before leaving today's lab, ensure that you have completed the following tasks.

- [ ] Verified the network topology
- [ ] Configured all router interfaces
- [ ] Configured all PCs
- [ ] Configured the DNS Server
- [ ] Verified connectivity
- [ ] Located an existing ACL
- [ ] Identified where the ACL was applied
- [ ] Removed the ACL
- [ ] Verified connectivity after removal
- [ ] Configured a Standard IPv4 ACL
- [ ] Applied the ACL to the correct interface
- [ ] Verified the ACL using show commands
- [ ] Tested the completed network
- [ ] Saved the router configurations

---

# Summary

In today's seminar you successfully:

- Configured a multi-network topology.
- Verified IP addressing and interface status.
- Investigated an existing Access Control List.
- Removed an ACL and observed the change in network behaviour.
- Configured your own Standard IPv4 ACL.
- Applied the ACL to the correct router interface.
- Verified the ACL using Cisco IOS commands.
- Tested network connectivity to confirm the ACL behaved as expected.

You have now completed the Week 9 ACL practical.
