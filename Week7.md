# Packet Tracer Lab 1 — Basic LAN Connectivity
### 1 Switch, Multiple PCs — Build, Configure, Test, Troubleshoot

---

## 1. What You're Doing

In this activity you will build the simplest possible network in Cisco Packet Tracer: a few PCs connected through a single switch. You'll assign IP addresses, test connectivity with `ping`, and then deliberately break and fix the network to understand **why** subnet masks matter.

This is the foundational skill behind every larger network you'll build later in the course.

**Estimated time:** 20–30 minutes

---

## 2. What You Need

- Cisco Packet Tracer (installed, any recent version)
- No prior configuration required — you're building from a blank workspace

---

## 3. Background: Key Concepts You Need Before Starting

Read this section fully before touching Packet Tracer. Everything you do in the activity is just a hands-on version of these ideas.

### 3.1 What is a network device?

A **network** is just a group of devices that can send data to each other. To do that, they need hardware that connects them and makes decisions about where data goes. The three you'll meet immediately are:

**PC / End Device**
The source or destination of traffic — a computer, laptop, server, or printer. It doesn't make forwarding decisions; it just sends and receives.

**Switch**
A switch connects multiple devices **within the same local network (LAN)** — think of it as a smart multi-port connector for a single room or building floor. It operates at **Layer 2** of the OSI model, meaning it makes decisions based on **MAC addresses** (a unique hardware address burned into every network card). A switch *learns* which MAC address sits on which port by watching traffic go past, and builds a **MAC address table** to forward frames efficiently instead of blasting them everywhere.

**Router**
A router connects **different networks** together — for example, your home network to the internet, or one office's LAN to another. It operates at **Layer 3**, meaning it makes decisions based on **IP addresses**, not MAC addresses. You won't use a router in this first activity (switch-only), but it's coming in the next lab.

**One-line distinction to remember:**
> *A switch connects devices inside one network. A router connects separate networks to each other.*

### 3.2 What are cables, and why does the type matter?

Devices don't talk to each other by magic — the physical link matters, and Packet Tracer models this realistically. The two cable types relevant to this activity:

| Cable | Used for | Why |
|---|---|---|
| **Copper Straight-Through** | PC ↔ Switch, Switch ↔ Router | The standard connection between two *different* device types |
| **Copper Crossover** | Switch ↔ Switch, PC ↔ PC directly | Historically needed to connect two *same* device types without a switch in between |

In Packet Tracer, if you use the wrong cable, the link light at the connection point turns **red** — it's the software telling you the physical layer isn't working, before you've even tried to send traffic. This mirrors reality: a bad or wrong cable means nothing above it (IP, ping, applications) will work, no matter how correctly you configure it.

*(Note: modern real-world hardware, and Packet Tracer's newer device models, often auto-detect and correct this — called Auto-MDIX — so you may see straight-through cables working even in "crossover" scenarios. Still worth understanding the traditional rule.)*

### 3.3 What is an IP address, and what is a subnet mask?

Every device on a network needs an **IP address** — a logical address (not tied to hardware) that identifies it, like `10.0.0.1`. Two devices can only communicate directly if they are on the **same network**.

But how does a device know if another IP is "on the same network" or not? That's the job of the **subnet mask**.

- The subnet mask tells a device which part of an IP address is the "network" part and which part is the "host" (individual device) part.
- Example: `255.0.0.0` means only the **first number** (`10` in `10.0.0.1`) defines the network — everything else identifies the specific device.
- Example: `255.255.255.0` means the **first three numbers** define the network — a much smaller, stricter network boundary.

**Why this matters practically:** if two devices have IP addresses that look similar (e.g. both start with `10.0.0.x`) but *disagree* on the subnet mask, they will disagree about whether they're on the same network — and communication can break, even though the addresses "look" compatible at a glance. You'll see this happen firsthand in Part D of this activity.

### 3.4 What is `ping`, and what does it actually test?

`ping` sends a small test message (an **ICMP Echo Request**) from one device to another and waits for a reply (**ICMP Echo Reply**). It's the simplest way to answer the question *"can these two devices actually reach each other?"* A successful ping means: physical link is up, IP addressing is compatible, and basic routing/switching along the path is working.

---

## 4. Devices and Cables Reference (Quick Lookup)

| Item | Where to find it in Packet Tracer | Purpose |
|---|---|---|
| PC | End Devices (bottom-left icon bar) | Generates/receives traffic |
| Switch (2960) | Network Devices → Switches | Connects devices on the same LAN |
| Copper Straight-Through cable | Connections (lightning bolt icon) | Connects PC ↔ Switch |

**Tip:** If you pick the wrong cable, Packet Tracer will show a **red dot** at the connection point — that's your signal something's wrong before you even test it.

---

## 5. Part A — Build the Topology

1. Open a new Packet Tracer file.
2. Drag **3 PCs** onto the workspace: `PC0`, `PC1`, `PC2`.
3. Drag **1 Switch** (2960 model) onto the workspace: `Switch0`.
4. Select the **Copper Straight-Through** cable tool.
5. Connect:
   - PC0 → Switch0
   - PC1 → Switch0
   - PC2 → Switch0
6. Wait for the link lights at each end of the cables to turn from **amber/orange** to **green** (this takes a few seconds — it's the switch port initializing).

Your topology should look like this:

```
   PC0 ----\
             \
   PC1 -------[ Switch0 ]
             /
   PC2 ----/
```

---

## 6. Part B — Assign IP Addresses

For each PC:

1. Click the PC.
2. Go to the **Desktop** tab.
3. Click **IP Configuration**.
4. Enter the following:

| Device | IP Address | Subnet Mask |
|---|---|---|
| PC0 | 10.0.0.1 | 255.0.0.0 |
| PC1 | 10.0.0.2 | 255.0.0.0 |
| PC2 | 10.0.0.3 | 255.0.0.0 |

5. Close the IP Configuration window.

---

## 7. Part C — Test Connectivity

1. Click **PC0** → **Desktop** tab → **Command Prompt**.
2. Run:
   ```
   ping 10.0.0.2
   ```
3. You should see 4 successful replies, e.g.:
   ```
   Reply from 10.0.0.2: bytes=32 time=1ms TTL=128
   ```
4. Now try:
   ```
   ping 10.0.0.3
   ```
   This should also succeed.

**Optional — see it happen step by step:**
- Switch to **Simulation Mode** (bottom-right toggle).
- Send the same ping again.
- Click **Capture/Forward** repeatedly to watch the packet travel PC0 → Switch0 → PC1, and the reply travel back.
- Click on Switch0 during simulation and check its **MAC address table** — it will now have learned the MAC addresses of the PCs it has seen traffic from.

---

## 8. Part D — Break It (On Purpose) and Explain Why

This is the important part. Understanding *failure* teaches you more than success does.

1. Click **PC1** → IP Configuration.
2. Change **only the subnet mask** to `255.255.255.0` (leave the IP address, `10.0.0.2`, unchanged).
3. Go back to **PC0**'s Command Prompt and run:
   ```
   ping 10.0.0.2
   ```

**You will likely see it fail or behave differently** (e.g., "Request timed out" or destination unreachable), even though the IP addresses still look like they're in the same range.

### Question to answer in your own words (write this down):
> Why does changing PC1's subnet mask break connectivity, even though PC0 and PC1 still have IP addresses that both start with `10.0.0.x`?

*Hint: the subnet mask determines what a device believes counts as "the same network." If two devices disagree on the mask, they disagree on where the network boundary is.*

---

## 9. Part E — Fix It

1. Change PC1's subnet mask back to `255.0.0.0`.
2. Re-run the ping from PC0 to confirm connectivity is restored.
3. Confirm all three PCs can ping each other successfully:
   - PC0 → PC1
   - PC0 → PC2
   - PC1 → PC2

---

## 10. Bonus Challenge (if time allows)

- Add a 4th PC to the switch, assign it a correct IP/mask on the same subnet, and confirm it can reach all others.
- Open the switch's CLI (click switch → CLI tab) and run:
  ```
  show mac address-table
  ```
  Identify which MAC addresses are listed and match them to the correct PCs.

---

## 11. Deliverable — What to Submit

Submit a short document (1 page max) containing:

1. A screenshot of your completed topology (3 PCs + 1 switch, all green links)
2. A screenshot of a successful `ping` result between two PCs
3. Your written answer to the Part D question above
4. A screenshot showing the fixed/working state after Part E

---

## Quick Troubleshooting Checklist

If something isn't working, check these in order:

- [ ] Are the cable link lights green on **both** ends?
- [ ] Did you use **Copper Straight-Through**, not crossover?
- [ ] Do the IP addresses of the devices you're testing actually match on subnet (same mask logic)?
- [ ] Did you wait a few seconds after cabling for the switch port to finish initializing (amber → green)?
- [ ] Are you testing from the **Command Prompt** inside the Desktop tab, not typing into the wrong window?
