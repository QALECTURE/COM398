# COM398 – OWASP WebGoat + Wireshark Security Lab

## Observe a Web Attack at Network Level

### Learning goal

In this lab, you will use **OWASP WebGoat** as a deliberately vulnerable training application and **Wireshark** to observe what happens on the network when a web application receives normal and manipulated requests.

By the end of the lab, you should be able to:

- explain the difference between **network security** and **application security**
- identify HTTP requests and responses in Wireshark
- identify source/destination addresses and TCP ports
- distinguish a normal request from a manipulated request
- explain **authentication vs authorization**
- explain why HTTPS/TLS protects application data in transit
- connect OWASP application vulnerabilities with network evidence

> **Important:** Only use the supplied local OWASP WebGoat environment.  
> Do not test these techniques against websites or systems you do not own or have permission to test.

---

# Lab scenario

You are a junior security analyst investigating traffic between a user and a web application.

The environment is:

```text
Student Browser
      |
      | HTTP
      v
OWASP WebGoat
localhost:8080
      |
      |
   Wireshark
captures the traffic
```

Later, you will also look at:

```text
WebGoat :8080  = vulnerable training application
WebWolf :9090  = attacker-side helper application
```

---

# Lab timing

| Time | Activity |
|---|---|
| 0–10 min | Start WebGoat and Wireshark |
| 10–20 min | Capture normal HTTP traffic |
| 20–35 min | Inspect and manipulate a request |
| 35–45 min | Explore authentication / access control |
| 45–55 min | Compare HTTP with HTTPS/TLS |
| 55–60 min | Complete investigation questions |

---

# Part 1 – Start the lab environment

## Step 1 – Check Docker

Open Terminal / PowerShell and run:

```bash
docker --version
```

You should see a Docker version.

If Docker is not available, inform your tutor.

---

## Step 2 – Download OWASP WebGoat

Run:

```bash
docker pull webgoat/webgoat
```

Wait for the image to download.

---

## Step 3 – Start WebGoat

Run:

```bash
docker run --name webgoat-lab --rm -it \
-p 127.0.0.1:8080:8080 \
-p 127.0.0.1:9090:9090 \
webgoat/webgoat
```

Windows PowerShell users may run the same command on one line:

```powershell
docker run --name webgoat-lab --rm -it -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 webgoat/webgoat
```

Wait until WebGoat has fully started.

---

## Step 4 – Open WebGoat

Open a browser and visit:

```text
http://localhost:8080/WebGoat
```

You should see the WebGoat application.

WebWolf is available at:

```text
http://localhost:9090/WebWolf
```

Do not worry about WebWolf yet.

---

# Part 2 – Start Wireshark

## Step 5 – Open Wireshark

Start Wireshark.

Because WebGoat is running on your own computer, you need to capture traffic from the **loopback interface**.

Typical names are:

### Windows

```text
Npcap Loopback Adapter
```

### macOS

```text
lo0
```

### Linux

```text
lo
```

Select the appropriate interface and start the capture.

---

## Step 6 – Filter WebGoat traffic

Enter this filter:

```text
tcp.port == 8080
```

This displays traffic related to WebGoat.

If Wireshark recognises the traffic as HTTP, also try:

```text
http
```

and:

```text
http.request
```

---

# Part 3 – Observe a normal web request

## Step 7 – Generate traffic

Return to WebGoat in your browser.

Navigate between two or three pages.

Then return to Wireshark.

You should now see packets.

---

## Step 8 – Select one HTTP request

Look for a packet containing something similar to:

```text
GET
```

or:

```text
POST
```

Select the packet.

Expand:

```text
Hypertext Transfer Protocol
```

Look for:

- Request Method
- Request URI
- Host
- User-Agent
- HTTP headers

---

## Step 9 – Record what you find

Complete:

```text
HTTP Method:

Request URI:

Destination Port:

Host:

Browser/User-Agent:
```

### Think

What does the HTTP method tell us?

Examples:

```text
GET  → request information

POST → send information to the application
```

---

# Part 4 – Understand what Wireshark is showing

A simplified request looks like:

```text
Browser
   |
   | HTTP Request
   v
Web Server
   |
   | HTTP Response
   v
Browser
```

Underneath HTTP, TCP is transporting the data:

```text
Application Layer
HTTP
   |
Transport Layer
TCP
   |
Network Layer
IP
```

Wireshark allows us to inspect these layers.

---

# Part 5 – Compare normal and manipulated requests

## Step 10 – Find a WebGoat lesson involving parameters or access control

Inside WebGoat, open one of the beginner lessons related to:

- HTTP requests
- authentication
- access control
- ID-based resources
- request parameters

Follow the instructions shown inside WebGoat.

The exact lesson available may depend on the installed WebGoat version.

---

## Step 11 – Observe the original request

Before changing anything, perform the normal action once.

Return to Wireshark.

Find the corresponding request.

Look at the:

```text
Request URI
```

or HTTP request body.

Example concept:

```text
/profile?id=101
```

---

## Step 12 – Change the supplied parameter inside the WebGoat lesson

Follow WebGoat's own exercise and change the parameter as instructed.

A conceptual example could be:

```text
/profile?id=101
```

changed to:

```text
/profile?id=102
```

The important point is not the specific value.

The important question is:

> What changed in the HTTP request?

Return to Wireshark and locate the new request.

Compare the two packets.

---

# Investigation 1

Complete the table:

| Item | Normal Request | Modified Request |
|---|---|---|
| HTTP Method | | |
| URI | | |
| Destination Port | | |
| Parameter/value changed | | |
| Server response different? | | |

---

# Why is this important?

A manipulated application request may still look like completely valid HTTP traffic.

For example:

```text
Normal user:

GET /profile?id=101
```

and:

```text
Manipulated request:

GET /profile?id=102
```

Both are valid HTTP.

But the application must decide whether the logged-in user is **authorised** to access the requested resource.

---

# Part 6 – Authentication vs Authorization

## Authentication

Authentication asks:

```text
WHO are you?
```

Examples:

- username + password
- MFA
- security token
- certificate

---

## Authorization

Authorization asks:

```text
WHAT are you allowed to access?
```

Example:

```text
Student A is authenticated.

Student A requests Student B's private record.

The application must reject the request.
```

A user can therefore be:

```text
Authenticated ✅

but

Not authorised ❌
```

This distinction is extremely important in application security.

---

# Part 7 – Observe WebWolf

WebWolf is supplied with WebGoat as an attacker-side helper application.

Open:

```text
http://localhost:9090/WebWolf
```

Now update the Wireshark filter:

```text
tcp.port == 8080 || tcp.port == 9090
```

Observe traffic to both applications.

Conceptually:

```text
                WebGoat
                :8080
                   ^
                   |
Browser -----------+
   |
   |
   +--------------> WebWolf
                    :9090
```

WebGoat represents the vulnerable application.

WebWolf represents an attacker-controlled/helper endpoint used by some WebGoat lessons.

You do not need to complete an advanced WebWolf attack today.

The goal is to identify that traffic can move between different application endpoints and that network monitoring can reveal those connections.

---

# Investigation 2

Answer:

1. Which TCP port is used by WebGoat?
2. Which TCP port is used by WebWolf?
3. Can Wireshark distinguish traffic going to each service?
4. What filter did you use?

---

# Part 8 – HTTP vs HTTPS

So far, WebGoat is intentionally using local HTTP for training.

With HTTP, application information may be visible to a packet analyser.

Conceptually:

```text
HTTP

Browser
   |
   | readable application request
   v
Server
```

HTTPS adds TLS:

```text
HTTPS

Browser
   |
   | encrypted TLS application data
   v
Server
```

---

## Step 13 – Generate normal HTTPS traffic

Open a normal HTTPS website approved by your tutor.

Do **not** attempt any attack.

Return to Wireshark.

Use:

```text
tls
```

You should see TLS-related traffic.

---

## Step 14 – Compare what you can inspect

With HTTP you may see:

- method
- URI
- headers
- application parameters

With HTTPS/TLS you can still normally observe networking information such as:

- source/destination IP addresses
- TCP ports
- packet timing
- packet size
- TLS connection information

But the application payload is encrypted.

---

# Investigation 3

Complete:

| Question | HTTP | HTTPS/TLS |
|---|---|---|
| Can packets be captured? | Yes | Yes |
| Can source/destination IPs be seen? | Yes | Yes |
| Can TCP ports be seen? | Yes | Yes |
| Can application content normally be read directly? | Often | No |
| Is application data protected in transit? | No | Yes |

---

# Part 9 – What does an attack look like to the network?

Consider:

```text
ATTACKER / USER
      |
      | Valid HTTP request
      | but malicious/manipulated input
      v
NETWORK
      |
      | Wireshark can see traffic
      v
WEB SERVER
      |
      v
APPLICATION
      |
      | Application decides
      | whether request is allowed
      v
DATABASE / RESOURCE
```

A key lesson is:

> A request can be valid at the network and protocol level but still be dangerous at the application level.

This is why cybersecurity uses multiple layers of defence.

---

# Part 10 – Security controls

Match the problem with a suitable control.

| Problem | Possible security control |
|---|---|
| Someone steals a password | MFA |
| User accesses another user's record | Authorization / access control |
| HTTP data visible on network | HTTPS/TLS |
| Suspicious request patterns | Monitoring / IDS / WAF |
| Too much user privilege | Least privilege |
| Insecure API access | Authentication + authorization + validation |
| Sensitive data exposed | Encryption + access control |

---

# Final challenge

You are the security analyst.

You observe:

```text
GET /account?id=201
```

followed seconds later by:

```text
GET /account?id=202
GET /account?id=203
GET /account?id=204
GET /account?id=205
```

Answer:

1. What might this behaviour indicate?
2. Is the traffic itself valid HTTP?
3. Why might a traditional firewall allow it?
4. Which layer should enforce whether the user can access each account?
5. Which security control would you recommend?
6. What evidence could Wireshark provide to an investigation?

---

# Useful Wireshark filters

```text
# WebGoat
tcp.port == 8080

# WebWolf
tcp.port == 9090

# WebGoat + WebWolf
tcp.port == 8080 || tcp.port == 9090

# HTTP
http

# HTTP requests
http.request

# HTTP responses
http.response

# DNS
dns

# ICMP
icmp

# TLS
tls

# TCP SYN packets
tcp.flags.syn == 1
```

---

# Expected learning outcome

By the end of this exercise you should understand the relationship:

```text
NETWORK TRAFFIC
      |
      v
Wireshark
"What communication occurred?"
      |
      v
WEB APPLICATION
      |
      v
OWASP vulnerability
"Should the application have allowed it?"
      |
      v
SECURITY CONTROL
"How do we prevent or detect it?"
```

---

# Submission / discussion

Before finishing, be ready to explain:

### 1. What is the difference between HTTP and HTTPS?

### 2. What is the difference between authentication and authorization?

### 3. What information could you identify using Wireshark?

### 4. Why can an application attack still look like valid network traffic?

### 5. Give one security control that would reduce the risk.

---

## Key takeaway

Security is not handled by one tool.

```text
Wireshark
    ↓
Network visibility

Firewall / IDS
    ↓
Network protection and monitoring

TLS
    ↓
Protect data in transit

Authentication
    ↓
Verify identity

Authorization
    ↓
Control access

OWASP secure coding
    ↓
Protect the application
```

A secure system requires these controls to work together.
