# COM398 Systems Security - Week 2 Lab

# Cryptography with OpenSSL: DES, AES, Encryption, Decryption and File Integrity

---

# Introduction

Last week, we used Wireshark to observe network traffic and understand how data travels between systems.

This week, we will answer an important security question:

> What happens if someone captures our network traffic or steals a file from our computer?

How can we prevent them from reading our information?

The answer is **Cryptography**.

In this lab, you will use Kali Linux and OpenSSL to encrypt files, decrypt them again, and verify whether files have been modified.

---

# Why Cryptography Exists

Imagine Alice wants to send a secret message to Bob.

```text
Alice
  |
  | Secret Message
  |
Internet
  |
Eve (Attacker)
```

If Eve intercepts the message, she can read it.

Now imagine Alice encrypts the message first.

```text
Alice
  |
Encrypt
  |
Ciphertext
  |
Internet
  |
Eve sees unreadable data
  |
Bob decrypts
```

Even if Eve captures the message, she cannot understand it.

This is why cryptography exists.

---

# Learning Outcomes

By the end of this lab, you should be able to:

* Explain cryptography in simple terms
* Explain plaintext and ciphertext
* Explain encryption and decryption
* Explain symmetric encryption
* Explain the purpose of DES and AES
* Use OpenSSL from the terminal
* Generate random key files
* Encrypt and decrypt files
* Use hashes to compare files
* Explain integrity

---

# Core Concepts

## Cryptography

Cryptography is the process of protecting information using mathematical techniques.

Examples:

* Online banking
* WhatsApp messages
* Secure file storage
* Password protection

---

## Plaintext

Plaintext is readable information.

Example:

```text
Hello COM398
```

---

## Ciphertext

Ciphertext is encrypted unreadable information.

Example:

```text
hJ82Lk9Pq72J...
```

---

## Encryption

Encryption converts plaintext into ciphertext.

```text
Plaintext
    ↓
Encryption
    ↓
Ciphertext
```

---

## Decryption

Decryption converts ciphertext back into plaintext.

```text
Ciphertext
    ↓
Decryption
    ↓
Plaintext
```

---

## Key

A key is secret information used during encryption and decryption.

Think of it as:

```text
Lock = Encryption Algorithm

Key = Secret Key
```

Without the correct key, recovering the original data should be extremely difficult.

---

# OpenSSL Explained

## What is OpenSSL?

OpenSSL is not an encryption algorithm.

OpenSSL is a toolkit that allows us to perform cryptographic operations.

Think of it like:

```text
Microsoft Word → Create Documents

OpenSSL → Perform Cryptographic Operations
```

---

## What Can OpenSSL Do?

```text
Generate Keys
Encrypt Files
Decrypt Files
Create Hashes
```

Architecture:

```text
           OpenSSL
               │
 ┌─────────────┼─────────────┐
 │             │             │
 ▼             ▼             ▼
Encrypt     Decrypt      Hash
 Files       Files       Files
```

---

# DES Explained

## Full Name

```text
Data Encryption Standard (DES)
```

---

## What Does DES Do?

DES converts readable information into unreadable information using a secret key.

```text
Plaintext
     │
     ▼
DES Encryption
     │
 Secret Key
     │
     ▼
Ciphertext
```

---

## DES Decryption

```text
Ciphertext
     │
     ▼
DES Decryption
     │
 Same Secret Key
     │
     ▼
Plaintext
```

---

## Why Is DES Important?

DES was one of the earliest widely used encryption standards.

Banks, governments and businesses used DES for many years.

Today DES is considered weak because its key size is too small compared to modern standards.

---

# AES Explained

## Full Name

```text
Advanced Encryption Standard (AES)
```

---

## What Does AES Do?

AES performs the same job as DES but uses stronger encryption.

```text
Plaintext
     │
     ▼
AES Encryption
     │
 Secret Key
     │
     ▼
Ciphertext
```

---

## AES Decryption

```text
Ciphertext
     │
     ▼
AES Decryption
     │
 Same Secret Key
     │
     ▼
Plaintext
```

---

## Why Is AES Important?

AES is the modern encryption standard used today.

Examples:

* WiFi security
* Online banking
* Mobile devices
* Cloud storage

---

# DES vs AES

| Feature    | DES                      | AES                          |
| ---------- | ------------------------ | ---------------------------- |
| Full Name  | Data Encryption Standard | Advanced Encryption Standard |
| Type       | Symmetric Encryption     | Symmetric Encryption         |
| Key Size   | 56-bit                   | 128/192/256-bit              |
| Security   | Weak Today               | Strong                       |
| Status     | Historical               | Current Standard             |
| Used Today | Rarely                   | Widely Used                  |

---

# Symmetric Encryption

Both DES and AES use symmetric encryption.

This means the same key is used for both encryption and decryption.

```text
Same Key
    │
    ▼
Encrypt
    │
    ▼
Ciphertext
    │
    ▼
Decrypt
    │
    ▼
Plaintext
```

---

# Lab Architecture

This is the workflow we will follow throughout the lab.

```text
VirtualBox
    │
    ▼
Kali Linux
    │
    ▼
Terminal
    │
    ▼
OpenSSL
    │
    ▼
Encrypt / Decrypt / Hash Files
```

File workflow:

```text
SystemSec.txt
      │
      ▼
 Encryption
      │
      ▼
SystemSec-enc.enc
      │
      ▼
 Decryption
      │
      ▼
SystemSec-dec.dec
      │
      ▼
 Compare Hashes
      │
      ▼
Integrity Verified
```

---

# Activity 1 - Start Kali Linux

1. Open VirtualBox.
2. Select Kali Linux.
3. Click Start.
4. Wait for Kali to load.
5. Open Terminal.

---

# Activity 2 - Check OpenSSL

Run:

```bash
openssl version
```

Purpose:

Check whether OpenSSL is installed.

---

Run:

```bash
openssl help
```

Purpose:

Display available OpenSSL commands.

---

Run:

```bash
openssl ciphers -v
```

Purpose:

Display supported encryption algorithms.

---

# Activity 3 - Create Working Folder

```bash
mkdir com398-week2
cd com398-week2
pwd
```

Purpose:

Create and enter a dedicated folder for today's lab.

---

# Activity 4 - Create Plaintext File

```bash
echo "COM398 Systems Security - This is my secret file." > SystemSec.txt
```

Display the file:

```bash
cat SystemSec.txt
```

This file is our plaintext.

---

# Activity 5 - Generate DES Key

```bash
openssl rand -out des_keySS 56
```

Purpose:

Generate random key material for DES.

---

# Activity 6 - DES Encryption

```bash
openssl des -e -kfile des_keySS -in SystemSec.txt -out SystemSec-enc.enc
```

View encrypted content:

```bash
cat SystemSec-enc.enc
```

Observe:

The contents are unreadable.

---

# Activity 7 - DES Decryption

```bash
openssl des -d -kfile des_keySS -in SystemSec-enc.enc -out SystemSec-dec.dec
```

Display decrypted file:

```bash
cat SystemSec-dec.dec
```

Observe:

The original plaintext is recovered.

---

# Activity 8 - Verify Integrity

Generate MD5 hashes:

```bash
openssl md5 SystemSec.txt
openssl md5 SystemSec-dec.dec
```

Observe:

Matching hashes indicate identical files.

---

# Activity 9 - Generate AES Key

```bash
openssl rand -out aes_keySS 128
```

Purpose:

Generate key material for AES.

---

# Activity 10 - AES Encryption

```bash
openssl aes256 -e -kfile aes_keySS -in SystemSec.txt -out SystemSec-aes.enc
```

Display encrypted file:

```bash
cat SystemSec-aes.enc
```

Observe:

The contents appear unreadable.

---

# Activity 11 - AES Decryption

```bash
openssl aes256 -d -kfile aes_keySS -in SystemSec-aes.enc -out SystemSec-aes.dec
```

Display decrypted file:

```bash
cat SystemSec-aes.dec
```

Observe:

The original plaintext is recovered.

---

# Activity 12 - Tampering Activity

Create a file:

```bash
echo "Alice salary record: GBP 50000" > Sec.txt
```

Generate key:

```bash
openssl rand -out aes256_key 32
```

Encrypt file:

```bash
openssl aes-256-ctr -e -kfile aes256_key -in Sec.txt -out Sec.enc
```

Generate hash:

```bash
openssl sha256 Sec.enc
```

Record the hash.

---

Modify the encrypted file:

```bash
echo "tampered" >> Sec.enc
```

Generate hash again:

```bash
openssl sha256 Sec.enc
```

Observe:

The hash value changes.

This demonstrates file integrity checking.

---

# Reflection Questions

1. What is plaintext?
2. What is ciphertext?
3. What is encryption?
4. What is decryption?
5. What is a key?
6. Why are DES and AES called symmetric encryption algorithms?
7. Why is AES preferred over DES today?
8. What is hashing?
9. Why did the hash change after tampering?
10. What does integrity mean?

---

# Final Summary

```text
OpenSSL
↓
Cryptography Toolkit

DES
↓
Older Encryption Algorithm

AES
↓
Modern Encryption Algorithm

DES and AES
↓
Symmetric Encryption

Hashing
↓
Integrity Checking

Encryption
↓
Confidentiality
```
