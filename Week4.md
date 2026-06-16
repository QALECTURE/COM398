# COM398 – Systems Security

# Week 4 Lab – Hashing, Integrity Checking and Digital Signatures

---

# Before We Start

## Question 1

Imagine you download:

```text
Windows 11 ISO
Ubuntu ISO
Kali Linux ISO
```

How do you know it has not been modified by a hacker?

Students usually answer:

* Website is trusted
* HTTPS is secure
* Antivirus checks it

Then ask:

> How does Microsoft know the file you downloaded is exactly the same file they uploaded?

---

## Question 2

Suppose I send you:

```text
Pay £100
```

and someone changes it to:

```text
Pay £1000
```

while it is travelling across the network.

Can encryption alone detect this?

Most students say:

"Yes."

Answer:

> No.

Encryption protects confidentiality.
Hashing protects integrity.

Today we learn how.

---

# Why This Matters in Industry

Every day:

* Software downloads
* Windows updates
* Docker images
* GitHub releases
* Banking transactions
* Digital contracts

use hashing.

Companies in London hiring for:

* Security Analysts
* SOC Analysts
* Cyber Engineers
* Cloud Engineers
* DevSecOps Engineers

expect you to understand:

```text
Hashing
Checksums
Integrity
HMAC
Digital Signatures
```

---

# Learning Outcomes

By the end of this lab you should be able to:

* Explain hashing
* Generate SHA256 hashes
* Verify file integrity
* Explain avalanche effect
* Generate HMAC values
* Understand digital signatures
* Detect file tampering

---

# Concept 1 – What Is Hashing?

Think of hashing like a fingerprint.

Every person:

```text
Person
  ↓
Fingerprint
```

Every file:

```text
File
  ↓
Hash
```

Example:

```text
Hello World
```

SHA256:

```text
a591a6d40bf420404...
```

If even one character changes:

```text
hello World
```

Completely different hash appears.

---

# Architecture

```text
Original File
      │
      ▼
 Hash Function
 (SHA256)
      │
      ▼
Fixed Length Hash
```

No matter whether the file is:

```text
1 KB
1 MB
1 GB
100 GB
```

The SHA256 output length stays the same.

---

# Hashing vs Encryption

## Encryption

```text
Message
   │
   ▼
Encrypt
   │
   ▼
Ciphertext
   │
Decrypt
   ▼
Original Message
```

Reversible.

---

## Hashing

```text
Message
   │
   ▼
Hash Function
   │
   ▼
Hash
```

Not reversible.

No such thing as:

```text
Unhash()
```

This is called a one-way function.

---

# Concept 2 – Integrity

Integrity means:

```text
Has the data changed?
```

Not:

```text
Can someone read it?
```

Example:

```text
Original:
Hello COM398

Modified:
Hello COM399
```

One character changes.

Integrity fails.

---

# Concept 3 – Avalanche Effect

One tiny change.

Huge hash difference.

Example:

```text
Hi
```

versus

```text
hi
```

Completely different hashes.

Ask students:

> Why is this useful?

Answer:

Because attackers cannot make tiny hidden changes.

---

# LAB 1 – Generate Your First Hash

Create a file.

```bash
echo "Hello COM398" > test.txt
```

Generate SHA256.

```bash
openssl dgst -sha256 test.txt
```

Observe:

```text
SHA256(test.txt)= ...
```

---

# LAB 2 – Verify Integrity

Change file:

```bash
nano test.txt
```

Change:

```text
Hello COM398
```

to

```text
Hello COM399
```

Run again:

```bash
openssl dgst -sha256 test.txt
```

Question:

> Did only one character change?

Yes.

> Did the hash change completely?

Yes.

This demonstrates the avalanche effect.

---

# Concept 4 – Why Companies Publish Checksums

When downloading software:

```text
Kali Linux
Ubuntu
OpenSSL
Windows
```

You often see:

```text
SHA256
```

next to the download.

Purpose:

```text
Download File
      │
      ▼
Generate Hash
      │
      ▼
Compare Hash
```

If both match:

```text
File Authentic
```

If different:

```text
File Modified
```

---

# LAB 3 – Real Integrity Verification

Download any file.

Generate:

```bash
openssl dgst -sha256 filename
```

Compare against the vendor checksum.

---

# Concept 5 – HMAC

Problem:

Hashing alone proves integrity.

But who created the file?

We don't know.

Solution:

```text
Hash
+
Secret Key
=
HMAC
```

This provides:

* Integrity
* Authentication

---

# HMAC Architecture

```text
Message
   │
Secret Key
   │
   ▼
HMAC Function
   │
   ▼
HMAC Output
```

---

# LAB 4 – Generate HMAC

Create file:

```bash
echo "This is COM398" > secret.txt
```

Generate HMAC:

```bash
openssl dgst -sha256 -hmac "abcdef" secret.txt
```

Observe output.

---

# LAB 5 – Bit Flipping Experiment

Open:

```text
secret.txt
```

Change:

```text
This
```

to

```text
Uhis
```

Only one character.

Generate HMAC again:

```bash
openssl dgst -sha256 -hmac "abcdef" secret.txt
```

Compare.

Question:

> Are the HMAC values similar?

No.

Completely different.

This proves integrity detection.

---

# Concept 6 – Digital Signatures

Hashing proves:

```text
Data unchanged
```

Digital signatures prove:

```text
Who sent it?
```

and

```text
They cannot deny sending it.
```

This is called:

```text
Non-Repudiation
```

---

# Digital Signature Workflow

## Sender

```text
Document
    │
    ▼
 Hash
    │
    ▼
Encrypt Hash
with Private Key
    │
    ▼
Signature
```

## Receiver

```text
Document
    │
    ▼
Hash Again

Signature
    │
    ▼
Decrypt
using Public Key

Compare Both Hashes
```

Match?

```text
Valid Signature
```

No match?

```text
Tampered Document
```

---

# Industry Examples

Where are hashes used?

* Password storage
* Git commits
* Docker images
* Blockchain
* TLS certificates
* Software downloads
* Digital signatures

---

# End of Lab Discussion

Ask students:

1. Why can't hashes be reversed?
2. Why is SHA256 preferred over MD5?
3. Why do software vendors publish checksums?
4. Why did changing one character create a completely different hash?
5. What extra security does HMAC provide?
6. Why are digital signatures important?

---

# Key Takeaways

```text
Encryption → Confidentiality

Hashing → Integrity

HMAC → Integrity + Authentication

Digital Signature →
Integrity +
Authentication +
Non-Repudiation
```

These are the four most important concepts from today's lab.
