# COM398 Systems Security  
## Mock Test Revision and Teaching Guide

> **Module:** COM398 – Systems Security  
> **Purpose:** Mock Test Revision  
> **Assessment Type:** Formative practice  
> **Marks:** 100  
> **Time Limit:** 60 minutes  
> **Attempts:** Unlimited  

---

# 1. Session Purpose

This session is designed to help students understand the concepts behind the mock test questions.

The aim is not simply to memorise the correct answers.

Students should understand:

- What concept each question is testing
- How to identify the important keywords
- How to eliminate incorrect answers
- How to calculate cipher results
- How the concepts are used in real-world cybersecurity
- What common mistakes to avoid

---

# 2. Topics Covered

The mock test covers:

- One-Time Pad encryption
- XOR operations
- Phishing
- Social engineering
- ROT13
- Caesar cipher
- Trojan malware
- Digital signatures
- Hashing
- Vigenère cipher
- Kasiski examination
- Symmetric encryption modes
- Output Feedback mode
- SHA-256 checksum verification

---

# 3. Security Goals

Before starting the questions, remember the main cybersecurity goals.

## Confidentiality

Confidentiality ensures that information can only be read by authorised people.

Examples:

- Encryption
- Password protection
- Access control

## Integrity

Integrity ensures that information has not been modified without authorisation.

Examples:

- Hashing
- Checksums
- Digital signatures

## Availability

Availability ensures that systems and information remain accessible when needed.

Examples:

- Backups
- Redundant servers
- Protection against denial-of-service attacks

## Authenticity

Authenticity confirms that a person, message, or system is genuine.

Examples:

- Digital signatures
- Digital certificates
- Multi-factor authentication

---

# 4. Question 1 – One-Time Pad and XOR

## Question

We want to encrypt the following message using a One-Time Pad:

```text
Message: 111000
Key:     1001001
```

Which ciphertext is correct?

- Option 1: `111110`
- Option 2: `0111001`
- Option 3: `1000110`
- Option 4: The key is not valid

## Correct Answer

```text
The key is not valid
```

---

## Concept

A One-Time Pad uses the XOR operation to combine a plaintext message with a secret key.

For a correct One-Time Pad:

- The key must be truly random
- The key must be secret
- The key must only be used once
- The key must be the same length as the message

---

## XOR Rules

XOR means **Exclusive OR**.

| Message Bit | Key Bit | Result |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

A simple way to remember XOR is:

```text
Same bits      = 0
Different bits = 1
```

---

## Example

```text
Message: 1011
Key:     1100
         ----
Cipher:  0111
```

Working:

```text
1 XOR 1 = 0
0 XOR 1 = 1
1 XOR 0 = 1
1 XOR 0 = 1
```

---

## Why the Provided Key Is Invalid

Count the number of bits.

```text
Message: 111000   = 6 bits
Key:     1001001  = 7 bits
```

The key and message are different lengths.

Therefore:

```text
The key is not valid.
```

---

## Common Mistake

Some students may use only the first six bits of the key.

```text
Message: 111000
Key:     100100
         ------
Result:  011100
```

However, the question provides a seven-bit key.

In a strict One-Time Pad question, the supplied key must match the plaintext length exactly.

---

## Real-World Analogy

Imagine six locked boxes.

Each box requires one unique key.

If you are given seven keys without being told which six to use, the instructions are incomplete.

---

## Classroom Activity

Encrypt the following:

```text
Message: 101101
Key:     110011
```

### Answer

```text
011110
```

---

# 5. Question 2 – Phishing

## Question

What is the primary purpose of phishing attacks?

- Option 1: Exploiting software vulnerabilities
- Option 2: Stealing sensitive information by posing as a trustworthy entity
- Option 3: Installing malware on a user's device
- Option 4: Disrupting network communication

## Correct Answer

```text
Stealing sensitive information by posing as a trustworthy entity
```

---

## Concept

Phishing is a form of **social engineering**.

Social engineering attacks human behaviour rather than directly attacking software.

The attacker pretends to be a trusted person or organisation.

Examples include pretending to be:

- A bank
- A university
- Microsoft
- Amazon
- A lecturer
- An employer
- An IT support team
- A delivery company

---

## What Attackers Want

A phishing attacker may try to steal:

- Usernames
- Passwords
- Banking details
- Credit card numbers
- One-time passwords
- Personal information
- Company information

They may also try to make the victim:

- Click a malicious link
- Download an attachment
- Install malware
- Approve a payment
- Reset a password through a fake website

---

## Real-World Example

A student receives the following email:

```text
Your university account will be disabled today.

Click the link below immediately to verify your password.
```

The link opens a fake university login page.

The student enters their username and password.

The attacker records the information.

---

## Why the Other Options Are Incorrect

### Exploiting software vulnerabilities

This describes technical attacks such as:

- SQL injection
- Buffer overflow
- Remote code execution
- Exploiting unpatched systems

Phishing mainly exploits human trust.

### Installing malware

Phishing can deliver malware, but malware installation is not always the primary purpose.

Some phishing attacks only steal credentials.

### Disrupting network communication

This is more closely related to:

- Denial-of-service attacks
- Network jamming
- Distributed denial-of-service attacks

---

## Types of Phishing

### General Phishing

The attacker sends the same message to many users.

### Spear Phishing

The attacker creates a personalised message for a specific victim.

Example:

```text
Hello Vishnu,

Please review the attached COM398 marks before tomorrow's meeting.
```

### Whaling

A phishing attack aimed at senior employees.

Examples:

- Chief executives
- Directors
- Finance managers
- Senior academics

### Smishing

Phishing through SMS messages.

### Vishing

Phishing through telephone or voice calls.

---

## Signs of Phishing

Look for:

- Urgent or threatening language
- Unexpected attachments
- Requests for passwords
- Suspicious links
- Spelling mistakes
- Unusual sender addresses
- Fake domain names
- Requests for immediate payment

Example:

```text
microsoft.com
```

may be impersonated as:

```text
micros0ft-login.com
```

Notice that the letter `o` has been replaced with the number `0`.

---

## Classroom Discussion

Consider this message:

```text
Your parcel is waiting.

Pay £1.49 for redelivery using the link below.
```

Discuss:

1. What looks suspicious?
2. What information may the attacker want?
3. What should the recipient do?
4. Should the recipient click the link?

---

# 6. Question 3 – ROT13

## Question

Apply ROT13 to:

```text
HELLO
```

- Option 1: `Uryyb`
- Option 2: `Trggy`
- Option 3: `Jryybo`
- Option 4: `Ubeeb`

## Correct Answer

```text
URYYB
```

The test displays the answer as `Uryyb`. The difference is only capitalisation.

---

## Concept

ROT13 means:

```text
Rotate each letter by 13 positions
```

It is a special type of Caesar cipher.

---

## ROT13 Alphabet

```text
Original: ABCDEFGHIJKLM NOPQRSTUVWXYZ
ROT13:    NOPQRSTUVWXYZ ABCDEFGHIJKLM
```

Examples:

```text
A → N
B → O
C → P
H → U
```

---

## Step-by-Step Calculation

```text
H → U
E → R
L → Y
L → Y
O → B
```

Therefore:

```text
HELLO → URYYB
```

---

## Special Property

Applying ROT13 twice returns the original message.

```text
HELLO → URYYB → HELLO
```

This happens because:

```text
13 + 13 = 26
```

There are 26 letters in the English alphabet.

---

## Real-World Example

ROT13 was sometimes used on internet forums to hide:

- Puzzle answers
- Spoilers
- Jokes
- Punchlines

It was not designed to provide serious security.

---

## Why ROT13 Is Weak

ROT13 has no secret key.

Anyone who knows ROT13 can reverse it immediately.

---

## Classroom Activity

Apply ROT13 to:

```text
SECURITY
```

### Answer

```text
FRPHEVGL
```

Apply ROT13 again:

```text
FRPHEVGL → SECURITY
```

---

# 7. Question 4 – Trojan Horse

## Question

What is a Trojan horse in the context of malicious software?

- Option 1: Software that spreads through email attachments
- Option 2: Malicious code that hides within a seemingly harmless program
- Option 3: A type of antivirus software
- Option 4: A program that encrypts files for ransom

## Correct Answer

```text
Malicious code that hides within a seemingly harmless program
```

---

## Concept

A Trojan disguises itself as useful or legitimate software.

The victim may be persuaded to:

- Download it
- Install it
- Open it
- Grant it permissions

---

## Why It Is Called a Trojan Horse

The name comes from the ancient story of the Trojan Horse.

A wooden horse appeared to be a gift, but soldiers were hidden inside.

A computer Trojan works similarly:

- The program appears harmless
- Malicious code is hidden inside

---

## Example

A user downloads:

```text
FreePremiumGame.exe
```

The program appears to install a game.

In the background, it may:

- Steal passwords
- Record keystrokes
- Install ransomware
- Create a backdoor
- Disable antivirus software
- Give an attacker remote access

---

## Trojan vs Virus vs Worm vs Ransomware

| Malware Type | Description |
|---|---|
| Trojan | Disguises itself as legitimate software |
| Virus | Attaches itself to a file and spreads when the file is executed |
| Worm | Spreads automatically across networks |
| Ransomware | Encrypts or locks files and demands payment |

A Trojan may install ransomware, but the two terms do not mean the same thing.

---

## Why the Other Options Are Incorrect

### Software that spreads through email attachments

This may describe a virus or worm.

### Antivirus software

Antivirus software protects systems from malware.

### A program that encrypts files for ransom

This describes ransomware.

---

## Real-World Analogy

A USB drive is labelled:

```text
Exam Answers
```

A student opens the files because they appear valuable.

The file secretly installs malware.

The useful-looking file is the disguise.

The malicious program is the hidden payload.

---

## Classroom Discussion

Which of the following could be a Trojan?

1. A cracked version of paid software
2. A fake antivirus program
3. An unexpected invoice viewer
4. A genuine update from the operating system settings

The first three could potentially be Trojans.

---

# 8. Question 5 – Digital Signatures

## Question

How does a digital signature ensure the integrity of a message?

- Option 1: It encrypts the entire message content
- Option 2: It attaches a timestamp to the message
- Option 3: It generates a hash value of the message and encrypts it with the sender's private key
- Option 4: It uses a checksum to verify the message integrity

## Correct Answer

```text
It generates a hash value of the message and encrypts it with the sender's private key
```

---

## Technical Clarification

The answer uses simplified introductory language.

A more accurate explanation is:

> The sender hashes the message and uses a digital signature algorithm with their private key to create a signature.

Modern digital signature algorithms do not always literally encrypt the hash.

---

## What a Digital Signature Provides

### Integrity

The message has not been changed.

### Authentication

The signature was created using the private key associated with the sender.

### Non-Repudiation

The sender cannot easily deny signing the message, provided their private key remained secure.

---

## What a Digital Signature Does Not Automatically Provide

A digital signature does not automatically provide confidentiality.

The message may still be readable.

Encryption must be used separately when confidentiality is required.

---

## Signing Process

Suppose Alice wants to send a document to Bob.

### Step 1 – Create the Message

```text
Transfer £100 to Bob.
```

### Step 2 – Hash the Message

```text
Hash(message) = A7F91C...
```

### Step 3 – Create the Signature

Alice uses her private key.

```text
Signature = Sign(Alice's private key, message hash)
```

### Step 4 – Send the Message and Signature

```text
Message + Digital Signature
```

---

## Verification Process

Bob receives:

```text
Message + Digital Signature
```

Bob then:

1. Hashes the received message
2. Uses Alice's public key to verify the signature
3. Checks whether the signature is valid

---

## Modified Message Example

Original:

```text
Transfer £100 to Bob.
```

Modified:

```text
Transfer £900 to Bob.
```

The modified message produces a different hash.

The signature verification fails.

---

## Real-World Uses

Digital signatures are used for:

- Software updates
- Digital certificates
- Electronic documents
- Secure email
- Code signing
- Financial transactions
- Application packages

---

## Digital Signature vs Encryption

| Digital Signature | Encryption |
|---|---|
| Provides integrity and authenticity | Provides confidentiality |
| Created using the sender's private key | Often uses the recipient's public key or a shared key |
| Verified using the sender's public key | Decrypted using the correct private or shared key |
| Message may remain readable | Message becomes unreadable |

---

## Why a Hash Alone Is Not Enough

An attacker could replace both:

```text
Document
Hash
```

The attacker could calculate a new hash for the modified document.

A digital signature protects authenticity because the attacker should not possess the sender's private key.

---

## Classroom Question

Does digitally signing a document mean nobody can read it?

```text
No.
```

A signature provides integrity and authenticity, not automatic confidentiality.

---

# 9. Question 6 – Perfect Secrecy of the One-Time Pad

## Question

Why is the One-Time Pad considered to provide perfect secrecy?

- Option 1: Because it is immune to brute-force attacks
- Option 2: Because it uses a complex algorithm
- Option 3: Because it uses a public-key infrastructure
- Option 4: Because the key is as long as the message and is used only once

## Correct Answer

```text
Because the key is as long as the message and is used only once
```

---

## Concept

A correctly used One-Time Pad provides **information-theoretic security**.

Even an attacker with unlimited computing power cannot uniquely determine the plaintext from the ciphertext alone.

---

## Conditions for Perfect Secrecy

The key must be:

1. Truly random
2. The same length as the message
3. Completely secret
4. Used only once
5. Securely shared with the recipient

---

## Simple Example

Suppose the ciphertext is:

```text
1010
```

It could represent many possible plaintexts.

Example 1:

```text
Plaintext: 0000
Key:       1010
```

Example 2:

```text
Plaintext: 1111
Key:       0101
```

Both possibilities produce the same ciphertext.

Without the key, the attacker cannot prove which plaintext is correct.

---

## Why Key Reuse Is Dangerous

Suppose the same key is used for two messages.

```text
C1 = P1 XOR K
C2 = P2 XOR K
```

The attacker calculates:

```text
C1 XOR C2
```

Because:

```text
K XOR K = 0
```

the keys cancel:

```text
C1 XOR C2 = P1 XOR P2
```

This reveals information about the two plaintext messages.

---

## Historical Example

The Venona project succeeded partly because One-Time Pad key material was reused.

The algorithm was secure.

The key-management process was not.

---

## Common Mistake

XOR itself does not provide perfect secrecy.

The security comes from:

```text
Random key
+
Message-length key
+
Single use
+
Secret distribution
```

---

# 10. Question 7 – Vigenère Cipher and Kasiski Examination

## Question

Which method is used to break the Vigenère cipher by finding the length of the key?

- Option 1: Frequency analysis
- Option 2: Kasiski test
- Option 3: Brute-force attack
- Option 4: Differential cryptanalysis

## Correct Answer

```text
Kasiski examination
```

The mock test currently spells the name as `Kasisky`.

The standard spelling is:

```text
Kasiski
```

---

## Concept

The Vigenère cipher uses a repeating keyword.

Example:

```text
Plaintext: ATTACKATDAWN
Key:       KEYKEYKEYKEY
```

The repeating key applies different Caesar shifts.

Because the key repeats, patterns can appear in the ciphertext.

---

## Kasiski Examination

### Step 1 – Find Repeated Sequences

Look for groups of letters that appear more than once in the ciphertext.

Example:

```text
ABC ... ABC
```

### Step 2 – Measure the Distances

Suppose repeated sequences occur at distances:

```text
18
24
30
```

### Step 3 – Find Common Factors

```text
18 = 2 × 3 × 3
24 = 2 × 2 × 2 × 3
30 = 2 × 3 × 5
```

A common factor is:

```text
3
```

The likely key length may therefore be three characters.

### Step 4 – Split the Ciphertext

If the key length is three:

```text
Group 1: Characters 1, 4, 7, 10...
Group 2: Characters 2, 5, 8, 11...
Group 3: Characters 3, 6, 9, 12...
```

Each group can then be analysed like a Caesar cipher.

---

## Why Frequency Analysis Is Not the Best Answer

Frequency analysis can help after the key length has been estimated.

The question specifically asks which technique finds the key length.

Therefore:

```text
Kasiski examination
```

is the intended answer.

---

## Alternative Technique

Another technique used to estimate Vigenère key length is:

```text
Index of Coincidence
```

---

## Classroom Activity

Repeated sequences occur at distances:

```text
12
18
24
```

Find a likely common factor.

### Answer

```text
6
```

A key length of six may be a strong candidate.

---

# 11. Question 8 – OFB Mode and Noisy Channels

## Question

Which symmetric encryption mode is best suited for stream-oriented transmission over a noisy channel such as satellite communication?

- Option 1: Electronic Codebook
- Option 2: Cipher Block Chaining
- Option 3: Output Feedback
- Option 4: Counter mode

## Correct Answer

```text
Output Feedback mode
```

---

## Concept

A block cipher such as AES processes fixed-size blocks.

A mode of operation determines how the cipher is used to encrypt larger messages or streams of data.

The main options in the question are:

- ECB
- CBC
- OFB
- CTR

---

## Why OFB Is the Intended Answer

OFB converts a block cipher into a stream-like cipher.

It generates a keystream.

```text
Ciphertext = Plaintext XOR Keystream
```

A bit error in the transmitted ciphertext normally affects only the corresponding plaintext bit.

The error does not automatically corrupt the following blocks.

This is useful over noisy communication channels.

---

## Example

Original ciphertext:

```text
10110110
```

One bit is changed by transmission noise:

```text
10100110
```

With OFB, the corresponding plaintext bit is affected.

Later bits can still decrypt correctly, provided synchronisation is maintained.

---

## Comparing Encryption Modes

### Electronic Codebook

Each block is encrypted independently.

Problem:

- Identical plaintext blocks produce identical ciphertext blocks
- Patterns remain visible

ECB should generally not be used for sensitive information.

### Cipher Block Chaining

Each plaintext block depends on the previous ciphertext block.

Benefits:

- Hides repeated patterns better than ECB

Disadvantages:

- Error propagation
- Sequential processing
- Padding may be required

### Output Feedback

OFB generates a stream of pseudorandom bits.

Benefits:

- Stream-oriented
- Limited error propagation
- No padding required

Disadvantages:

- Sender and receiver must remain synchronised
- The initialisation value must not be reused with the same key

### Counter Mode

CTR uses a counter to generate a keystream.

Benefits:

- Fast
- Parallelisable
- Supports random access
- Limited error propagation

---

## Technical Note

CTR also has properties suitable for noisy channels.

However, introductory cryptography questions traditionally associate:

```text
OFB = stream transmission over noisy channels
```

Therefore, OFB is the expected answer.

---

## Security Warning

OFB provides confidentiality but not automatic integrity.

Modern systems often use authenticated encryption such as:

```text
AES-GCM
```

or:

```text
ChaCha20-Poly1305
```

---

# 12. Question 9 – Disadvantage of the One-Time Pad

## Question

What is a significant disadvantage of the One-Time Pad?

- Option 1: It is too slow to compute
- Option 2: It does not guarantee confidentiality
- Option 3: The key must be as long as the plaintext
- Option 4: It is vulnerable to frequency analysis

## Correct Answer

```text
The key must be as long as the plaintext
```

---

## Concept

The One-Time Pad is mathematically secure but difficult to manage.

---

## Example

To encrypt:

```text
1 GB of data
```

the sender needs:

```text
1 GB of random key material
```

To encrypt:

```text
100 GB of data
```

the sender needs:

```text
100 GB of random key material
```

---

## Key-Management Problems

The organisation must:

- Generate truly random keys
- Securely transfer the keys
- Store large amounts of key material
- Prevent key theft
- Track which key sections have been used
- Prevent key reuse
- Securely destroy used key material

---

## Why the Other Options Are Incorrect

### It is too slow to compute

The XOR operation is extremely fast.

### It does not guarantee confidentiality

A correctly used One-Time Pad provides perfect confidentiality.

### It is vulnerable to frequency analysis

A correctly used One-Time Pad does not preserve useful frequency patterns.

---

## One-Time Pad vs AES

| One-Time Pad | AES |
|---|---|
| Key is as long as the message | Uses a fixed-length key |
| Key must only be used once | Key can be reused safely through a correct protocol |
| Provides perfect secrecy | Provides computational security |
| Difficult key management | More practical key management |
| Uses simple XOR | Uses a more complex encryption algorithm |

---

## Key Teaching Point

> The One-Time Pad is easy to calculate but extremely difficult to manage securely.

---

# 13. Question 10 – Caesar Cipher

## Question

Encrypt the following word using a Caesar cipher with a shift of three positions to the right:

```text
BALLLOON
```

- Option 1: `EDOOORRQ`
- Option 2: `DOOORLOO`
- Option 3: `EDOORRPO`
- Option 4: `LOOBBOON`

## Correct Answer

```text
EDOOORRQ
```

---

## Concept

A Caesar cipher shifts every letter by a fixed number of positions.

For a shift of three:

```text
A → D
B → E
C → F
D → G
```

At the end of the alphabet, the letters wrap around.

```text
X → A
Y → B
Z → C
```

---

## Step-by-Step Calculation

```text
B → E
A → D
L → O
L → O
L → O
O → R
O → R
N → Q
```

Therefore:

```text
BALLLOON → EDOOORRQ
```

---

## Important Spelling Note

The question uses:

```text
BALLLOON
```

with three consecutive `L` characters.

Students should encrypt exactly what is written.

The normal spelling:

```text
BALLOON
```

would produce:

```text
EDOORRQ
```

The question's spelling:

```text
BALLLOON
```

produces:

```text
EDOOORRQ
```

---

## Mathematical Formula

Represent the alphabet using numbers:

```text
A = 0
B = 1
C = 2
...
Z = 25
```

Encryption is:

```text
Ciphertext = (Plaintext + Shift) mod 26
```

Example for `B`:

```text
B = 1

(1 + 3) mod 26 = 4

4 = E
```

Example for `Y`:

```text
Y = 24

(24 + 3) mod 26 = 1

1 = B
```

---

## Why Caesar Cipher Is Weak

There are only 25 useful shifts.

An attacker can try every possible shift.

Example:

```text
KHOOR
```

Shift three positions backwards:

```text
HELLO
```

This is a brute-force attack.

---

## Classroom Activity

Encrypt:

```text
CYBER
```

using a shift of three.

### Answer

```text
FBEHU
```

---

# 14. Practical Activity – SHA-256 File Verification

## Task

Download a small open-source file from a trusted website.

Examples:

- OpenSSL
- Python
- Ubuntu
- Linux distributions
- Open-source GitHub releases

Find the published SHA-256 checksum and compare it with the hash calculated on your computer.

---

## What Is a Cryptographic Hash?

A cryptographic hash function takes data of any size and produces a fixed-size output.

SHA-256 produces a:

```text
256-bit hash
```

It is normally displayed as 64 hexadecimal characters.

---

## Properties of SHA-256

### Deterministic

The same input always produces the same hash.

### Fixed Length

A small file and a large file both produce a 256-bit SHA-256 hash.

### Avalanche Effect

A tiny change in the file produces a very different hash.

### One-Way

It should be computationally impractical to reconstruct the original file from the hash.

### Collision Resistance

It should be extremely difficult to find two different files with the same hash.

---

## Why Verify a Download?

A checksum can help confirm that:

- The file was downloaded correctly
- The file was not accidentally corrupted
- The file was not modified during transfer

---

## Important Limitation

The published checksum must come from a trusted source.

If an attacker changes both:

- The file
- The published checksum

the comparison may still appear valid.

Digital signatures provide stronger authenticity.

---

# 15. SHA-256 Commands

## macOS

```bash
shasum -a 256 filename
```

Example:

```bash
shasum -a 256 openssl-file.tar.gz
```

Alternative:

```bash
openssl dgst -sha256 openssl-file.tar.gz
```

---

## Linux

```bash
sha256sum filename
```

Example:

```bash
sha256sum openssl-file.tar.gz
```

---

## Windows PowerShell

```powershell
Get-FileHash .\filename -Algorithm SHA256
```

Example:

```powershell
Get-FileHash .\openssl-file.tar.gz -Algorithm SHA256
```

---

# 16. Comparing the Checksums

Suppose the website shows:

```text
Published checksum:
5d41402abc4b2a76b9719d911017c592
```

The local computer shows:

```text
Local checksum:
5d41402abc4b2a76b9719d911017c592
```

The student should write:

```text
Checksum matches.
```

If even one character is different:

```text
Published:
5d41402abc4b2a76b9719d911017c592

Local:
5d41402abc4b2a76b9719d911017c593
```

The student should write:

```text
Checksum does not match.
```

Do not install or execute a file when the checksum unexpectedly differs.

---

# 17. Suggested Submission Evidence

Students should submit:

1. A screenshot showing the locally calculated SHA-256 hash
2. The name of the downloaded file
3. The published checksum
4. The locally calculated checksum
5. A short conclusion

Example:

```text
The published SHA-256 checksum and my locally calculated checksum are identical.

Checksum matches.
```

---

# 18. Suggested Classroom Delivery

## Step 1 – Hide the Options

Show only the question.

Ask students to explain the concept before revealing the answers.

## Step 2 – Identify Keywords

Examples:

| Keyword | Concept |
|---|---|
| Same length as message | One-Time Pad |
| Pretends to be trusted | Phishing |
| Shift by 13 | ROT13 |
| Harmless-looking software | Trojan |
| Private key | Digital signature |
| Repeating keyword | Vigenère |
| Noisy channel | OFB |
| Shift letters | Caesar cipher |

## Step 3 – Apply the Rule

Students should calculate or explain the rule before choosing an option.

## Step 4 – Eliminate Incorrect Options

Ask why each incorrect option is wrong.

## Step 5 – Connect to Real Life

Use examples involving:

- University emails
- Fake parcel messages
- Software downloads
- Banking applications
- Operating-system updates
- Password theft
- Malware disguised as useful software

---

# 19. Quick Revision Summary

## One-Time Pad

```text
Ciphertext = Plaintext XOR Key
```

Requirements:

```text
Truly random key
Key is as long as the message
Key remains secret
Key is used only once
```

---

## XOR

```text
Same bits      = 0
Different bits = 1
```

---

## Phishing

```text
An attacker pretends to be trustworthy to steal information or manipulate a victim.
```

---

## ROT13

```text
Caesar cipher with a shift of 13.
```

Applying ROT13 twice returns the original text.

---

## Trojan Horse

```text
Malware disguised as legitimate or harmless software.
```

---

## Digital Signature

```text
Hash the message
Create a signature using the private key
Verify using the public key
```

Provides:

- Integrity
- Authenticity
- Non-repudiation

Does not automatically provide confidentiality.

---

## Vigenère Cipher

```text
Uses a repeating keyword.
```

Kasiski examination:

```text
Find repeated sequences
Measure distances
Find common factors
Estimate key length
```

---

## OFB

```text
Turns a block cipher into a stream-like cipher.
```

A transmission error normally affects only the corresponding bit.

---

## Caesar Cipher

```text
Shifts every letter by the same number of positions.
```

It is weak because there are very few possible keys.

---

## SHA-256

```text
Creates a fixed-size digital fingerprint of data.
```

Used mainly to verify integrity.

---

# 20. Corrections and Clarifications for the Mock Test

## Question 1

The plaintext has six bits.

The key has seven bits.

Therefore, the supplied key is invalid for a strict One-Time Pad question.

## Question 3

The conventional uppercase ROT13 answer is:

```text
URYYB
```

The test answer `Uryyb` is equivalent apart from capitalisation.

## Question 5

The phrase “encrypts the hash using the private key” is acceptable for introductory teaching.

A more accurate explanation is:

```text
The private key is used by a digital signature algorithm to create a signature over the message or its hash.
```

## Question 7

The correct spelling is:

```text
Kasiski
```

not:

```text
Kasisky
```

## Question 8

OFB is the expected textbook answer.

CTR also provides limited error propagation and is commonly used in modern systems.

## Question 10

The question intentionally or accidentally uses three consecutive `L` characters:

```text
BALLLOON
```

Students should encrypt the exact text shown.

---

# 21. Final Revision Questions

Before completing the mock test, students should be able to answer:

1. What does XOR do?
2. Why must a One-Time Pad key be used only once?
3. How is phishing different from a software exploit?
4. What is the difference between a Trojan and a worm?
5. What security properties are provided by a digital signature?
6. Does a digital signature encrypt the message?
7. How does the Kasiski examination estimate a Vigenère key length?
8. Why does OFB have limited error propagation?
9. Why is the One-Time Pad difficult to use in practice?
10. How do you verify a downloaded file using SHA-256?

---

# 22. Final Message to Students

> Do not memorise only the option number.  
> Understand the rule behind the answer.  
> The wording may change in the real assessment, but the cybersecurity concept will remain the same.
