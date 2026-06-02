# COM398 System Security - Week 2 Lab

## Cryptography with OpenSSL: DES, AES, Encryption, Decryption and File Integrity

## 1. Introduction

In this lab, you will use Kali Linux and OpenSSL to encrypt, decrypt, and check files.

The lab uses two encryption methods:

| Method | What to remember |
| --- | --- |
| DES | An older encryption method. You will use it to understand the basic process. |
| AES | A stronger encryption method. You will use it after DES and compare the workflow. |

You will create a small text file, encrypt it, decrypt it, and then use hashing to check whether files match.

> **Important**  
> This lab is only about your own files inside your own Kali Linux lab machine.

## 2. Learning Outcomes

By the end of this lab, you should be able to:

1. Explain plaintext, ciphertext, encryption, decryption, and keys.
2. Explain symmetric encryption.
3. Open Kali Linux and use the terminal.
4. Use basic OpenSSL commands.
5. Create a plaintext file.
6. Generate key files.
7. Encrypt and decrypt a file using DES.
8. Encrypt and decrypt a file using AES.
9. Use hashing to compare files.
10. Explain integrity in simple terms.

## 3. Core Concepts

| Term | Short meaning |
| --- | --- |
| Cryptography | Protecting information using mathematical methods. |
| Plaintext | The original readable message or file. |
| Ciphertext | The encrypted unreadable version of the message or file. |
| Encryption | Turning plaintext into ciphertext. |
| Decryption | Turning ciphertext back into plaintext. |
| Key | Secret data used to encrypt and decrypt. |
| Symmetric encryption | The same key is used for encryption and decryption. |
| DES | An older symmetric encryption method. |
| AES | A stronger symmetric encryption method used in modern systems. |
| Hashing | Creating a short fingerprint of a file. |
| Integrity | Knowing whether a file has changed. |
| OpenSSL | A terminal tool used in this lab for encryption, decryption, random keys, and hashes. |

## 4. Architecture Diagram

This is the full lab workflow:

```text
VirtualBox
    |
    v
Kali Linux
    |
    v
Terminal
    |
    v
OpenSSL
    |
    v
Create plaintext file
    |
    v
Generate key file
    |
    v
Encrypt file  --->  Ciphertext file
    |
    v
Decrypt file  --->  Plaintext file again
    |
    v
Hash files to check integrity
```

The file workflow looks like this:

```text
SystemSec.txt
    |
    | encrypt with a key
    v
SystemSec-enc.enc
    |
    | decrypt with the same key
    v
SystemSec-dec.dec
    |
    | compare hashes
    v
Check whether the original and decrypted files match
```

## 5. Kali Setup

### Step 5.1: Start Kali Linux

What to do:

1. Open VirtualBox.
2. Click your Kali Linux virtual machine.
3. Click **Start**.
4. Wait for Kali to load.
5. Log in if asked.

Why we do this:

Kali Linux is the lab operating system. OpenSSL will be run inside Kali.

Expected output:

You should see the Kali desktop.

### Step 5.2: Open the Terminal

What to do:

Open the terminal from the Kali menu or click the terminal icon.

Why we do this:

The terminal lets you type commands. This lab is command-based.

Expected output:

You should see a terminal window with a prompt where you can type.

## 6. OpenSSL Basics

### Command 1: Check OpenSSL Version

Run:

```bash
openssl version
```

What it does:

Shows the installed OpenSSL version.

Why we run it:

To check that OpenSSL is available before starting the lab.

Expected output:

You should see a line beginning with `OpenSSL`, for example:

```text
OpenSSL 3.x.x
```

### Command 2: Show OpenSSL Help

Run:

```bash
openssl help
```

What it does:

Shows a help list for OpenSSL commands.

Why we run it:

To confirm that OpenSSL can display its available command options.

Expected output:

You should see a long list of OpenSSL commands and options.

### Command 3: Create Your Working Folder

Run:

```bash
mkdir com398-week2
```

What it does:

Creates a folder called `com398-week2`.

Why we run it:

To keep all Week 2 lab files in one place.

Expected output:

There may be no message. That usually means the command worked.

### Command 4: Move Into the Working Folder

Run:

```bash
cd com398-week2
```

What it does:

Moves your terminal into the `com398-week2` folder.

Why we run it:

The files you create next should be stored inside this folder.

Expected output:

There may be no message. Your prompt may change slightly.

### Command 5: Check Your Current Folder

Run:

```bash
pwd
```

What it does:

Prints the folder you are currently inside.

Why we run it:

To confirm you are working in the correct folder.

Expected output:

The output should end with:

```text
com398-week2
```

## 7. DES Lab

DES uses symmetric encryption. This means the same key is used to encrypt and decrypt.

### Step 7.1: Create a Plaintext File

Run:

```bash
echo "COM398 Systems Security - This is my secret file." > SystemSec.txt
```

What it does:

Creates a file called `SystemSec.txt` containing one line of readable text.

Why we run it:

We need a plaintext file to encrypt.

Expected output:

There may be no message. The file should be created.

### Step 7.2: Display the Plaintext File

Run:

```bash
cat SystemSec.txt
```

What it does:

Displays the contents of `SystemSec.txt`.

Why we run it:

To confirm the plaintext file contains the expected message.

Expected output:

```text
COM398 Systems Security - This is my secret file.
```

### Step 7.3: List the Files

Run:

```bash
ls -l
```

What it does:

Lists files in the current folder with extra detail.

Why we run it:

To confirm that `SystemSec.txt` exists.

Expected output:

You should see `SystemSec.txt` in the list.

### Step 7.4: Generate a DES Key File

Run:

```bash
openssl rand -out des_keySS 56
```

What it does:

Creates a file called `des_keySS` containing random data.

Why we run it:

DES needs key material for encryption and decryption.

Expected output:

There may be no message. A file called `des_keySS` should be created.

### Step 7.5: Encrypt the File with DES

Run:

```bash
openssl des -e -kfile des_keySS -in SystemSec.txt -out SystemSec-enc.enc
```

What it does:

Encrypts `SystemSec.txt` using DES and writes the encrypted output to `SystemSec-enc.enc`.

Why we run it:

To turn the readable plaintext file into unreadable ciphertext.

Expected output:

There may be no message. A file called `SystemSec-enc.enc` should be created.

### Step 7.6: View the Encrypted File

Run:

```bash
cat SystemSec-enc.enc
```

What it does:

Displays the encrypted file.

Why we run it:

To see that ciphertext does not look like normal readable text.

Expected output:

You should see unreadable or strange-looking output. This is normal.

### Step 7.7: Decrypt the DES File

Run:

```bash
openssl des -d -kfile des_keySS -in SystemSec-enc.enc -out SystemSec-dec.dec
```

What it does:

Decrypts `SystemSec-enc.enc` and writes the result to `SystemSec-dec.dec`.

Why we run it:

To turn the ciphertext back into readable plaintext.

Expected output:

There may be no message. A file called `SystemSec-dec.dec` should be created.

### Step 7.8: Display the Decrypted DES File

Run:

```bash
cat SystemSec-dec.dec
```

What it does:

Displays the decrypted file.

Why we run it:

To check that decryption recovered the original message.

Expected output:

```text
COM398 Systems Security - This is my secret file.
```

## 8. AES Lab

AES also uses symmetric encryption. In this lab, AES follows the same basic workflow as DES: generate a key file, encrypt, decrypt, and check the result.

### Step 8.1: Generate an AES Key File

Run:

```bash
openssl rand -out aes_keySS 128
```

What it does:

Creates a file called `aes_keySS` containing random data.

Why we run it:

AES needs key material for encryption and decryption.

Expected output:

There may be no message. A file called `aes_keySS` should be created.

### Step 8.2: Encrypt the File with AES

Run:

```bash
openssl aes256 -e -kfile aes_keySS -in SystemSec.txt -out SystemSec-aes.enc
```

What it does:

Encrypts `SystemSec.txt` using AES and writes the encrypted output to `SystemSec-aes.enc`.

Why we run it:

To practise encryption with AES after using DES.

Expected output:

There may be no message. A file called `SystemSec-aes.enc` should be created.

### Step 8.3: View the AES Encrypted File

Run:

```bash
cat SystemSec-aes.enc
```

What it does:

Displays the AES encrypted file.

Why we run it:

To confirm that encrypted data is not readable as normal text.

Expected output:

You should see unreadable or strange-looking output.

### Step 8.4: Decrypt the AES File

Run:

```bash
openssl aes256 -d -kfile aes_keySS -in SystemSec-aes.enc -out SystemSec-aes.dec
```

What it does:

Decrypts `SystemSec-aes.enc` and writes the result to `SystemSec-aes.dec`.

Why we run it:

To recover the original plaintext from the AES ciphertext.

Expected output:

There may be no message. A file called `SystemSec-aes.dec` should be created.

### Step 8.5: Display the Decrypted AES File

Run:

```bash
cat SystemSec-aes.dec
```

What it does:

Displays the decrypted AES file.

Why we run it:

To confirm that AES decryption recovered the original message.

Expected output:

```text
COM398 Systems Security - This is my secret file.
```

## 9. Hashing and Integrity Lab

Hashing creates a short fingerprint of a file. If two files have the same hash, they are very likely to have the same contents. In this lab, we use hashes to check integrity.

### Step 9.1: Hash the Original File with MD5

Run:

```bash
openssl md5 SystemSec.txt
```

What it does:

Creates an MD5 hash of `SystemSec.txt`.

Why we run it:

To get a fingerprint of the original plaintext file.

Expected output:

You should see output similar to:

```text
MD5(SystemSec.txt)= a_long_hash_value
```

### Step 9.2: Hash the DES Decrypted File with MD5

Run:

```bash
openssl md5 SystemSec-dec.dec
```

What it does:

Creates an MD5 hash of the DES decrypted file.

Why we run it:

To compare the DES decrypted file with the original file.

Expected output:

The hash value should match the MD5 hash from `SystemSec.txt`.

### Step 9.3: Hash the Original File with SHA-256

Run:

```bash
openssl sha256 SystemSec.txt
```

What it does:

Creates a SHA-256 hash of `SystemSec.txt`.

Why we run it:

To practise using another hashing command.

Expected output:

You should see output similar to:

```text
SHA256(SystemSec.txt)= a_long_hash_value
```

### Step 9.4: Hash the AES Decrypted File with SHA-256

Run:

```bash
openssl sha256 SystemSec-aes.dec
```

What it does:

Creates a SHA-256 hash of the AES decrypted file.

Why we run it:

To compare the AES decrypted file with the original file.

Expected output:

The hash value should match the SHA-256 hash from `SystemSec.txt`.

## 10. Tampering Activity

Tampering means changing a file. In this activity, you will change an encrypted file and prove that its hash changes.

### Step 10.1: Create a New Plaintext File

Run:

```bash
echo "Alice salary record: GBP 50000" > Sec.txt
```

What it does:

Creates a file called `Sec.txt` with readable text.

Why we run it:

We need a simple file for the tampering activity.

Expected output:

There may be no message. A file called `Sec.txt` should be created.

### Step 10.2: Generate a Key File

Run:

```bash
openssl rand -out aes256_key 32
```

What it does:

Creates a file called `aes256_key` containing random data.

Why we run it:

The file will be used as key material for encryption.

Expected output:

There may be no message. A file called `aes256_key` should be created.

### Step 10.3: Encrypt the File

Run:

```bash
openssl aes-256-ctr -e -kfile aes256_key -in Sec.txt -out Sec.enc
```

What it does:

Encrypts `Sec.txt` and writes the encrypted output to `Sec.enc`.

Why we run it:

To create an encrypted file that will later be changed.

Expected output:

There may be no message. A file called `Sec.enc` should be created.

### Step 10.4: Hash the Encrypted File Before Tampering

Run:

```bash
openssl sha256 Sec.enc
```

What it does:

Creates a SHA-256 hash of `Sec.enc`.

Why we run it:

To record the fingerprint of the encrypted file before it is changed.

Expected output:

You should see a SHA-256 hash. Write it down or take a screenshot.

### Step 10.5: Tamper with the Encrypted File

Run:

```bash
echo "tampered" >> Sec.enc
```

What it does:

Adds the word `tampered` to the end of `Sec.enc`.

Why we run it:

To simulate a file being changed after encryption.

Expected output:

There may be no message. The encrypted file has now been changed.

### Step 10.6: Hash the Encrypted File After Tampering

Run:

```bash
openssl sha256 Sec.enc
```

What it does:

Creates a new SHA-256 hash of the changed encrypted file.

Why we run it:

To check whether the file fingerprint changed.

Expected output:

The new hash should be different from the hash before tampering.

What this shows:

Encryption changes readable data into unreadable data. Hashing helps you notice when a file has changed.

## 11. Reflection Questions

Answer these questions after completing the lab:

1. What is plaintext?
2. What is ciphertext?
3. What is encryption?
4. What is decryption?
5. Why does symmetric encryption need the same key for encryption and decryption?
6. What happened when you used `cat` on an encrypted file?
7. Did the decrypted DES file match the original plaintext file?
8. Did the decrypted AES file match the original plaintext file?
9. What does a hash help you check?
10. What happened to the hash after you tampered with `Sec.enc`?
