# COM398 Systems Security

# Week 8 - Secure Remote Access with SSH

---

# Introduction

In today's lab, we are going to configure **secure remote access to a Cisco switch using SSH**.

Before entering any commands, let us understand the problem.

Imagine you are working as a network engineer.

Your organisation may have:

- 10 switches
- 50 switches
- 500 switches
- thousands of network devices across different locations

If a switch is located in another building, another city, or inside a data centre, you cannot physically walk to the switch every time you need to change its configuration.

Instead, network engineers use **remote management**.

The basic idea is:

```text
Network Engineer
       |
       v
      PC
       |
       | Remote Management
       v
 Cisco Network Device
