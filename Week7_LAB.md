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

# Packet Tracer Lab — Wireless LAN Basics
### Access Point, SSID, Security, and Wireless Connectivity

---

## 1. What You're Doing

In this activity you will build a small **wireless network** in Cisco Packet Tracer: one Access Point and two wireless-enabled PCs. You'll configure a network name (SSID), secure it with a password, connect your devices, and test connectivity — then compare it directly to the wired switch network you already built in Lab 1.

**Estimated time:** 25–35 minutes

**Prerequisite:** Completion of Lab 1 (Basic LAN Connectivity), since this activity builds on IP addressing and `ping` concepts you already know.

---

## 2. What You Need

- Cisco Packet Tracer (installed, any recent version)
- No prior wireless configuration required — you're building from a blank workspace

---

## 3. Background: Key Concepts You Need Before Starting

### 3.1 Wired vs Wireless — what actually changes?

In Lab 1, PCs connected to a switch using physical cables. In this lab, PCs will connect to an **Access Point (AP)** using radio waves instead of cables. Here's the important point:

> **Wireless only changes how devices connect physically (Layer 1/2). Everything above that — IP addresses, subnet masks, ping — works exactly the same way as your wired lab.**

So an Access Point is functionally similar to a switch: it lets multiple devices on the same LAN talk to each other. The difference is the connection method, not the networking logic.

### 3.2 What is an Access Point (AP)?

An Access Point is a device that creates a wireless network and allows Wi-Fi capable devices to join it. In Packet Tracer, you'll use a standalone AP (or a Wireless Router, which is an AP + router combined — for this activity we'll use a plain AP so it behaves like a switch, not a router).

### 3.3 What is an SSID?

**SSID (Service Set Identifier)** is simply the name of the wireless network — what you'd see in a phone's Wi-Fi list (e.g. "Home-WiFi", "University-Guest"). Every AP broadcasts (or optionally hides) an SSID so devices know which network to join.

### 3.4 Why do we secure wireless networks?

Unlike a cable, radio signals travel through walls and can be picked up by anyone nearby — there's no physical boundary stopping an outsider from trying to connect. This is why wireless networks use authentication:

| Security Type | Notes |
|---|---|
| **Open (none)** | No password — anyone in range can connect. Not recommended, but useful to show students the risk. |
| **WEP** | Old, broken encryption — no longer considered secure. Worth mentioning historically. |
| **WPA2-PSK** | Current standard for small networks — a shared password (Pre-Shared Key) all devices use to join. This is what you'll configure. |

### 3.5 What is a wireless NIC?

A **NIC (Network Interface Card)** is the hardware that lets a device connect to a network. Wired PCs in Packet Tracer come with an Ethernet NIC by default. To make a PC wireless-capable, you need to physically swap in a **Wireless NIC** — you'll do this yourself in Part A, which is a good hands-on reminder that wireless isn't automatic; it requires the right hardware.

---

## 4. Devices Reference (Quick Lookup)

| Item | Where to find it in Packet Tracer | Purpose |
|---|---|---|
| Access Point | Network Devices → Wireless Devices | Creates the wireless network (SSID) |
| PC | End Devices | End device — needs a wireless NIC added |
| Wireless NIC (e.g. WMP300N) | PC → Physical tab → Modules | Gives a PC the ability to connect wirelessly |

**Note:** No cables are used to connect the PCs to the AP — that's the whole point of this lab. You will still need a power connection (automatic in Packet Tracer) but no Ethernet cable between PC and AP.

---

## 5. Part A — Add Wireless Capability to Your PCs

By default, PCs in Packet Tracer have a wired Ethernet card, not a wireless one. You need to swap it.

1. Drag **2 PCs** onto the workspace: `PC0`, `PC1`.
2. Click **PC0** → go to the **Physical** tab (not Desktop).
3. **Power off** the PC first — click the power button (green switch icon) on the device image, or it won't let you swap modules.
4. On the left-hand module list, find a wireless NIC (e.g. **WMP300N**).
5. Drag the existing wired NIC module **out** of the PC (drag it to the module list to remove it).
6. Drag the **wireless NIC** module **into** the empty slot.
7. **Power the PC back on**.
8. Repeat steps 2–7 for **PC1**.

**Check:** Go to each PC's **Desktop** tab. You should now see a **PC Wireless** option instead of just IP Configuration — this confirms the wireless NIC installed correctly.

---

## 6. Part B — Add and Configure the Access Point

1. Drag an **Access Point** (Network Devices → Wireless Devices) onto the workspace: `AP0`.
2. Click **AP0** → **Config** tab → **Port 1 (Radio)** or the SSID field.
3. Set the SSID to something identifiable, e.g.:
   ```
   ClassroomWiFi
   ```
4. Leave the AP powered on (default).

At this point, PC0 and PC1 should automatically detect the AP within range (Packet Tracer places wireless devices within range of each other by default on the same canvas).

---

## 7. Part C — Connect PCs to the Wireless Network

1. Click **PC0** → **Desktop** tab → **PC Wireless**.
2. Go to the **Connect** tab — you should see `ClassroomWiFi` listed.
3. Select it and click **Connect**.
4. If prompted for security, for now leave it as **no authentication** (we'll secure it in Part E).
5. Repeat for **PC1**.

**Check:** the AP's link light (or the small wireless signal icon at each PC) should turn green, indicating an active wireless association.

---

## 8. Part D — Assign IPs and Test Connectivity

Just like Lab 1 — wireless doesn't change this part at all.

1. On **PC0** → PC Wireless (or IP Configuration) → set:
   - IP: `192.168.10.1`
   - Subnet mask: `255.255.255.0`
2. On **PC1** → set:
   - IP: `192.168.10.2`
   - Subnet mask: `255.255.255.0`
3. Open **Command Prompt** on PC0 and run:
   ```
   ping 192.168.10.2
   ```
4. You should get 4 successful replies — exactly like the wired lab, just over radio instead of copper.

**Optional:** switch to **Simulation Mode** and send the ping again. Watch the packet travel PC0 → AP0 → PC1 — notice the animation shows a wireless signal (radiating circle) instead of a cable-based hop.

---

## 9. Part E — Secure the Network (WPA2)

Right now your wireless network is **open** — anyone could connect. Let's fix that.

1. Click **AP0** → **Config** tab.
2. Find the **Security** dropdown and change it from "Disabled" to **WPA2-PSK**.
3. Set a passphrase, e.g.:
   ```
   ClassSecure2026
   ```
4. Now go to **PC0** → PC Wireless → **Connect** tab.
   - Notice `ClassroomWiFi` now shows a padlock icon.
   - You'll need to reconnect and enter the passphrase to re-associate.
5. Repeat for **PC1**.
6. Confirm ping still works between PC0 and PC1 after reconnecting with the password.

### Question to answer in your own words (write this down):
> Why is securing a wireless network with WPA2 more important than securing a wired switch connection? What's physically different about how someone could try to access each one?

---

## 10. Part F — Break It (On Purpose) and Explain Why

1. On **PC1**, disconnect from `ClassroomWiFi` (PC Wireless → Connect tab → Disconnect).
2. From **PC0**, try to ping PC1 again.
3. Observe the failure (e.g. "Request timed out").
4. Reconnect PC1 with the correct passphrase and confirm ping succeeds again.

This reinforces: association to the AP is a *prerequisite* for IP connectivity — no different in principle from a cable being unplugged in Lab 1, just wireless instead of physical.

---

## 11. Bonus Challenge (if time allows)

- Add a **3rd wireless PC** and confirm all three can ping each other.
- Try setting an **incorrect** WPA2 passphrase on one PC deliberately and observe what happens (it should fail to associate at all, not just fail to ping).
- Compare: open AP vs WPA2 AP — capture a ping in Simulation Mode for both and discuss what would happen if someone outside the classroom tried to join an open network within range.

---

## 12. Deliverable — What to Submit

Submit a short document (1–2 pages) containing:

1. A screenshot of your topology (2 wireless PCs + 1 AP)
2. A screenshot of the AP's SSID and WPA2 configuration
3. A screenshot of a successful `ping` between the two PCs **after** WPA2 was enabled
4. Your written answer to the Part E question (wireless vs wired security)
5. A short note on what happened in Part F (disconnect/reconnect test)

---

## Quick Troubleshooting Checklist

- [ ] Did you **power off** the PC before swapping in the wireless NIC? (Packet Tracer won't allow module changes on a powered device.)
- [ ] Did you power the PC back **on** after installing the wireless NIC?
- [ ] Is the PC actually **connected** to the SSID (not just "in range")? Check the Connect tab shows "Connected."
- [ ] After enabling WPA2, did you **reconnect** each PC with the new passphrase? Existing connections don't auto-upgrade.
- [ ] Are both PCs on the **same subnet** (same logic as the wired lab)?
- [ ] Is the AP powered on and within range on the canvas (Packet Tracer treats proximity on the workspace as signal range)?
