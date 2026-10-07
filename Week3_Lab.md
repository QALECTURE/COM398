# COM398 System Security
## Week 3 Lab: RSA Encryption and Digital Signatures

**Student reference guide**

This guide follows the Week 3 lab in two parts: **RSA encryption/decryption** and **digital signatures**. Work from top to bottom and check each output before moving on.

> **Core rule:** The private key stays private. The public key can be shared.

---

## 1. What you will learn

By the end of this lab, you should be able to:

- Generate an RSA private key.
- Generate the corresponding public key.
- Encrypt a small file using a public key.
- Decrypt it using the matching private key.
- Sign a document using a private key.
- Verify a signature using a public key.
- Demonstrate that changing a signed document causes verification to fail.
- Explain confidentiality, integrity and authentication in the Alice, Bob and Eve scenario.

The original lab uses two scenarios: Alice sends Bob a secret file while Eve may observe the communication, and Alice sends a digitally signed file so Bob can detect modification.

---

# 2. Before you start

Open a terminal and check OpenSSL:

```bash
openssl version
```

Create a clean working folder:

```bash
mkdir week3-rsa-lab
cd week3-rsa-lab
```

Check the folder regularly:

```bash
ls
```

Windows Command Prompt users can use:

```cmd
dir
```

If a command says a file cannot be found, first check that you are in the correct folder and that the filename is correct.

---

# PART 1: RSA ENCRYPTION AND DECRYPTION

## 3. Understand the scenario

```text
BOB
 |
 | creates RSA key pair
 |
 +---- private.pem  -> KEEP SECRET
 |
 +---- public.pem   -> SHARE WITH ALICE


ALICE
Secrets.txt
    |
    | encrypt using Bob's PUBLIC key
    v
encrypted.bin
    |
    | sent across the network
    | Eve may capture this
    v
BOB
    |
    | decrypt using Bob's PRIVATE key
    v
DecryptedSecrets.txt
```

Before typing commands, remember:

```text
Encrypt for Bob -> Bob's PUBLIC key
Decrypt by Bob  -> Bob's PRIVATE key
```

---

## 4. Generate Bob's private key

Run:

```bash
openssl genrsa -aes256 -out private.pem 2048
```

Choose a temporary lab passphrase that you can remember. Do not reuse a real password.

Check:

```bash
ls
```

You should see:

```text
private.pem
```

The `-aes256` option protects the private-key file with a passphrase.

Inspect the key if you want to see its structure:

```bash
openssl rsa -in private.pem -text -noout
```

### Security rule

`private.pem` must remain private. Do not email it, upload it to GitHub, or give it to Alice.

---

## 5. Generate Bob's public key

Derive the public key:

```bash
openssl rsa -in private.pem -pubout -out public.pem
```

Enter the private-key passphrase when requested.

Check:

```bash
ls
```

You should now have:

```text
private.pem
public.pem
```

Inspect the public key:

```bash
openssl pkey -pubin -in public.pem -text -noout
```

Remember:

```text
private.pem -> KEEP SECRET
public.pem  -> CAN BE SHARED
```

---

## 6. Create Alice's secret file

macOS/Linux:

```bash
echo "This is a secret message for Bob." > Secrets.txt
```

Windows Command Prompt:

```cmd
echo This is a secret message for Bob. > Secrets.txt
```

Read it:

```bash
cat Secrets.txt
```

Windows:

```cmd
type Secrets.txt
```

Your folder should now contain:

```text
private.pem
public.pem
Secrets.txt
```

---

## 7. Encrypt with Bob's public key

Run:

```bash
openssl pkeyutl -encrypt -pubin -inkey public.pem -in Secrets.txt -out encrypted.bin
```

Check:

```bash
ls
```

You should now see `encrypted.bin`.

Try:

```bash
cat encrypted.bin
```

It should not look like the original readable message.

Conceptually:

```text
Secrets.txt
    |
    | Bob's public key
    v
RSA encryption
    |
    v
encrypted.bin
```

Alice can send `encrypted.bin` to Bob. Eve may capture it, but in this scenario she does not possess Bob's private key.

---

## 8. Decrypt with Bob's private key

Run:

```bash
openssl pkeyutl -decrypt -inkey private.pem -in encrypted.bin -out DecryptedSecrets.txt
```

Enter the passphrase if requested.

Read the recovered file:

```bash
cat DecryptedSecrets.txt
```

Windows:

```cmd
type DecryptedSecrets.txt
```

You should recover the original message.

Compare the original and recovered files on macOS/Linux:

```bash
diff Secrets.txt DecryptedSecrets.txt
```

You can also compare SHA-256 hashes:

```bash
openssl dgst -sha256 Secrets.txt
openssl dgst -sha256 DecryptedSecrets.txt
```

Matching contents produce matching SHA-256 digests.

---

## 9. Part 1 checkpoint

You should now be able to explain this without looking at the commands:

```text
Bob creates private key
        |
        v
Bob derives public key
        |
        v
Bob shares public key with Alice
        |
        v
Alice encrypts Secrets.txt
        |
        v
Alice sends encrypted.bin
        |
        v
Bob decrypts with private key
        |
        v
Original message recovered
```

### Required Part 1 tasks

1. Create an RSA private key.
2. Generate the public key.
3. Create `Secrets.txt`.
4. Encrypt it with the public key.
5. Decrypt it with the private key.
6. Confirm that the recovered text is the same.

---

# PART 2: DIGITAL SIGNATURES

## 10. Encryption and signing are different

Do not mix these workflows.

```text
ENCRYPTION SCENARIO

Public key  -> encrypt
Private key -> decrypt
```

For the signing scenario:

```text
DIGITAL SIGNATURE

Private key -> sign
Public key  -> verify
```

Encryption in Part 1 is about **confidentiality**.

Digital signing in Part 2 lets the recipient check **integrity** and verify the signature using the corresponding public key.

---

## 11. Create the document

Create `contract.txt`.

macOS/Linux:

```bash
echo "Alice agrees to pay Bob 100 pounds." > contract.txt
```

Windows:

```cmd
echo Alice agrees to pay Bob 100 pounds. > contract.txt
```

Read it before continuing:

```bash
cat contract.txt
```

---

## 12. Sign the document

Run:

```bash
openssl dgst -sha256 -sign private.pem -out sign.sha256 contract.txt
```

Enter the private-key passphrase if requested.

Check:

```bash
ls
```

You should see:

```text
contract.txt
sign.sha256
```

Conceptually:

```text
contract.txt
     |
     | SHA-256 + private-key signing operation
     v
sign.sha256
```

The signature is a separate file.

---

## 13. Verify the signature

Bob needs:

```text
contract.txt
sign.sha256
public.pem
```

Run:

```bash
openssl dgst -sha256 -verify public.pem -signature sign.sha256 contract.txt
```

Expected result:

```text
Verified OK
```

Do not modify `contract.txt` before completing this successful verification.

---

## 14. Become Eve: modify the signed document

Now change:

```text
Alice agrees to pay Bob 100 pounds.
```

to:

```text
Alice agrees to pay Bob 900 pounds.
```

Save the file but **do not create a new signature**.

Run the same verification command:

```bash
openssl dgst -sha256 -verify public.pem -signature sign.sha256 contract.txt
```

Verification should now fail.

Why?

```text
ORIGINAL DOCUMENT
      +
ORIGINAL SIGNATURE
      |
      v
Verified OK


MODIFIED DOCUMENT
      +
ORIGINAL SIGNATURE
      |
      v
Verification Failure
```

The original signature corresponds to the original document. Modifying the document changes the data being verified.

---

## 15. Base64 representation of the signature

The worksheet also converts the binary signature to Base64 for text-based exchange.

Encode:

```bash
openssl base64 -in sign.sha256 -out signature.txt
```

Read it:

```bash
cat signature.txt
```

Decode it again:

```bash
openssl base64 -d -in signature.txt -out recovered-sign.sha256
```

Then verify:

```bash
openssl dgst -sha256 -verify public.pem -signature recovered-sign.sha256 contract.txt
```

If you previously modified `contract.txt`, restore the original text before expecting `Verified OK`.

> **Remember:** Base64 is encoding, not encryption. Anyone can decode Base64.

---

## 16. Part 2 required tasks

1. Generate an RSA private key.
2. Generate the public key.
3. Create `contract.txt`.
4. Sign the document.
5. Verify it and obtain `Verified OK`.
6. Modify `contract.txt`.
7. Verify it again.
8. Record what happened and explain why.

---

# 17. Troubleshooting

### `Could not open file` or `No such file`

Run:

```bash
ls
```

or on Windows:

```cmd
dir
```

Check the current folder and filename carefully.

### OpenSSL asks for a passphrase

That is expected because `private.pem` was protected when it was generated.

### Verification fails unexpectedly

Check:

```text
Correct public key?
Correct signature file?
Correct document?
Was the document modified?
```

### Older instructions use `rsautl`

Older OpenSSL examples may use `rsautl`. This reference guide uses `openssl pkeyutl` for the RSA encryption/decryption exercise.

---

# 18. Final folder check

At the end you may have:

```text
week3-rsa-lab/
|
|-- private.pem
|-- public.pem
|-- Secrets.txt
|-- encrypted.bin
|-- DecryptedSecrets.txt
|-- contract.txt
|-- sign.sha256
|-- signature.txt
`-- recovered-sign.sha256
```

Do not only check that the files exist. Make sure you know **what each file represents**.

---

# 19. Knowledge check

Answer these before finishing:

1. What is the difference between a public and private key?
2. Why can Bob share `public.pem`?
3. Why must `private.pem` remain secret?
4. Which key does Alice use to encrypt for Bob?
5. Which key does Bob use to decrypt?
6. What security property does Part 1 demonstrate?
7. Which key creates the digital signature?
8. Which key verifies it?
9. Why does verification fail after `contract.txt` changes?
10. Does Base64 provide confidentiality?
11. Why is SHA-256 present in the signing command?
12. What could happen if an attacker obtained the private key?
13. Why should a private key never be committed to a public repository?
14. What is the difference between confidentiality and integrity?
15. Why is signature verification different from decryption?

---

# 20. Challenge: Alice, Bob and Eve

Work in groups of three.

**Alice:** Create and sign a document.

**Bob:** Receive the document, signature and Alice's public key, then verify it.

**Eve:** Intercept the document and change one value, for example:

```text
Transfer amount: £100
```

to:

```text
Transfer amount: £900
```

Bob verifies again.

Discuss:

- Did Eve need the private key to modify the document?
- Can Eve produce Alice's valid signature without Alice's private key?
- What happened when Bob verified the modified document?
- Which security property allowed Bob to detect the change?

---

# 21. Further learning for interested students

The sections below go beyond the minimum Week 3 practical.

## 21.1 Why RSA is not normally used to encrypt large files directly

In real systems, public-key and symmetric cryptography are commonly combined.

A simplified **hybrid encryption** model is:

```text
                    RANDOM SESSION KEY
                         |
                +--------+--------+
                |                 |
                v                 v
          AES encrypts       Public-key
          actual data        mechanism helps
                             protect/establish
                             key material
```

Symmetric encryption is efficient for large amounts of data. Public-key cryptography helps solve key-management or key-establishment problems.

---

## 21.2 Why SHA-256 appears in the signature command

Digital signatures commonly operate with a cryptographic digest of the document.

Conceptually:

```text
DOCUMENT
   |
 SHA-256
   |
   v
DIGEST
   |
private-key signing operation
   |
   v
SIGNATURE
```

Verification checks whether the signature is valid for the document using the corresponding public key.

This is why the lab command includes:

```bash
dgst -sha256
```

---

## 21.3 Public keys still need trust

Suppose Eve intercepts Bob's public key before Alice receives it and substitutes Eve's key.

Alice might encrypt information for Eve while believing the key belongs to Bob.

This introduces an important question:

> How does Alice know that a public key really belongs to Bob?

Real systems use trust mechanisms such as **digital certificates** and **Public Key Infrastructure (PKI)**.

---

## 21.4 Connection to HTTPS

When you connect to an HTTPS website, cryptography is used to establish an authenticated, protected connection.

Digital certificates help connect an identity or domain with cryptographic key information.

The real protocol is more sophisticated than this classroom RSA exercise, but the same fundamental concepts appear:

```text
public/private keys
authentication
key establishment
symmetric encryption
integrity protection
```

---

## 21.5 Key size is not the whole security story

Cryptographic security depends on more than key size.

It also depends on:

- algorithm choice,
- correct protocol design,
- secure random-number generation,
- key storage,
- implementation quality,
- secure configuration,
- and keeping private material private.

A mathematically strong algorithm cannot protect a private key that has been leaked.

---

## 21.6 Modern RSA terminology

This lab is designed to make the public/private-key workflow visible.

In modern cryptographic systems you may also encounter terms such as:

```text
RSA-OAEP
RSA-PSS
```

These relate to secure constructions for RSA encryption and signatures. You do not need to implement them to complete this lab, but they are useful terms for further study.

---

# 22. Link to the Week 3 lecture

```text
LECTURE                         LAB

Asymmetric cryptography   ->    public.pem + private.pem
RSA                       ->    RSA key pair
Public key                ->    encryption / verification
Private key               ->    decryption / signing
Integrity                 ->    tamper with contract.txt
Attacker Eve              ->    interception/modification
Trust                     ->    does the public key really
                                belong to the claimed person?
```

The objective is not to memorise OpenSSL syntax.

The objective is to understand **what each command is doing, which key is involved, and which security property it provides**.

---

# 23. Final memory map

```text
                    RSA KEY PAIR
                   /                              /                            PUBLIC            PRIVATE
              SHARE             SECRET
                |                  |
          +-----+-----+      +-----+-----+
          |           |      |           |
       ENCRYPT      VERIFY  DECRYPT      SIGN
```

For the scenarios in this lab:

```text
CONFIDENTIALITY

Alice:
plaintext + Bob's public key
              |
              v
          ciphertext

Bob:
ciphertext + Bob's private key
              |
              v
           plaintext
```

```text
DIGITAL SIGNATURE

Alice:
document + Alice's private key
              |
              v
           signature

Bob:
document + signature + Alice's public key
              |
              v
       Verified OK / Failure
```

## Lab complete

Before leaving, make sure you can explain these three questions without looking at the commands:

1. Why can a public key be shared while a private key must remain secret?
2. Why does Bob need the corresponding private key to recover the encrypted message?
3. Why does changing a signed document cause signature verification to fail?
