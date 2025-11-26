# HTTPS: Secure Web Communication
## Lecture 34 - Applied Cryptography (UE22CS342AA4)

---

## Table of Contents
1. [Understanding Symmetric vs Asymmetric Encryption](#understanding-symmetric-vs-asymmetric-encryption)
2. [Symmetric Key Encryption](#symmetric-key-encryption)
3. [Examples of Symmetric Encryption](#examples-of-symmetric-encryption)
4. [Advantages of Symmetric Encryption](#advantages-of-symmetric-encryption)
5. [Asymmetric Key Encryption](#asymmetric-key-encryption)
6. [Examples of Asymmetric Encryption](#examples-of-asymmetric-encryption)
7. [Advantages of Asymmetric Encryption](#advantages-of-asymmetric-encryption)
8. [Use Cases for Symmetric Encryption](#use-cases-for-symmetric-encryption)
9. [Introduction to HTTPS](#introduction-to-https)
10. [What is HTTPS](#what-is-https)
11. [HTTPS vs HTTP](#https-vs-http)
12. [SSL/TLS Protocols](#ssltls-protocols)
13. [Symmetric and Asymmetric in HTTPS](#symmetric-and-asymmetric-in-https)
14. [Session Keys](#session-keys)
15. [HTTPS Working: Step-by-Step](#https-working-step-by-step)
16. [Step 1: HTTPS Connection Request](#step-1-https-connection-request)
17. [Step 2: Public Key Transmission](#step-2-public-key-transmission)
18. [Step 3: Session Key Generation](#step-3-session-key-generation)
19. [Step 4: Session Key Encryption](#step-4-session-key-encryption)
20. [Step 5: Switch to Symmetric Encryption](#step-5-switch-to-symmetric-encryption)
21. [Banking Analogy](#banking-analogy)
22. [How Each Encryption Type is Used](#how-each-encryption-type-is-used)
23. [Security Considerations](#security-considerations)

---

## Understanding Symmetric vs Asymmetric Encryption

### The Core Question

**Which is more secure: Symmetric or Asymmetric?**

**Answer**: It depends on the context and use case

---

## Symmetric Key Encryption

### Definition

**In Symmetric-key encryption**:
- Message encrypted using a key
- **Same key** used to decrypt message
- Makes it easy to use
- But less secure than asymmetric

### Core Principle

**Key usage**:
- One key for both encryption and decryption
- Both sender and receiver have identical key
- Must share key securely

### Fundamental Problem

**Key distribution challenge**:
- Both parties need the same key
- Must transfer key securely
- If key intercepted, encryption broken
- How to securely transfer the key?

### Simple Example

```
Alice: plaintext + key → [Encryption Algorithm] → ciphertext
Bob:   ciphertext + key → [Decryption Algorithm] → plaintext
```

---

## Examples of Symmetric Encryption

### DES (Data Encryption Standard)

**Historical significance**:
- Published: 1977
- Standard for decades
- 56-bit key length
- Now considered weak

### Triple DES (3DES)

**Improvement over DES**:
- Applies DES three times
- 168-bit effective key
- Slower than modern algorithms
- Still used in legacy systems

### AES (Advanced Encryption Standard)

**Current standard**:
- Adopted: 2001
- 128, 192, or 256-bit key lengths
- Very secure
- Industry standard
- Fast implementation

**Security**: AES-256 considered secure for decades

### IDEA (International Data Encryption Algorithm)

**Characteristics**:
- 128-bit key
- Block cipher
- Designed in Switzerland
- Used in PGP

### TLS/SSL Protocol

**Uses symmetric encryption**:
- For actual data transmission
- After asymmetric key exchange
- Session keys for each connection
- Fast and efficient

---

## Advantages of Symmetric Encryption

### Advantage 1: Security

**Property**: Encryption very difficult to break

**Strength**:
- AES algorithms take billions of years to crack using brute-force
- 2^128 or 2^256 possible keys
- Computationally infeasible to crack

**Example**: AES-256 would take longer than age of universe to brute force

### Advantage 2: Speed

**Performance**:
- Symmetric encryption very fast
- Shorter key lengths (compared to asymmetric)
- Relative simplicity compared to asymmetric
- Much faster to execute

**Comparison**: Symmetric ~1000x faster than RSA

### Advantage 3: Industry Adoption and Acceptance

**Status**:
- AES (and similar) are gold standard
- Decades of industry adoption
- Accepted by governments
- Well-studied and trusted
- Standardized globally

**Trust**: Long track record of security

---

## Asymmetric Key Encryption

### Definition

**Asymmetric key encryption**:
- Uses two different keys
- One key encrypts data
- Different key decrypts data
- One public, one private

### Core Principle

**Key pair system**:
- Public key: Known to everyone
- Private key: Kept secret by owner
- Mathematically related
- Cannot derive private from public

### Fundamental Advantage

**No key distribution problem**:
- Public key can be shared openly
- No need to transfer private key
- Solves key distribution challenge
- More secure key exchange

### Simple Example

```
Alice: plaintext + Bob's public key → [Encryption Algorithm] → ciphertext
Bob:   ciphertext + Bob's private key → [Decryption Algorithm] → plaintext
```

### How It Works

**User A encrypts with User B's public key**:
- Only User B's private key can decrypt
- User A doesn't need User B's private key
- User B keeps private key secret

---

## Examples of Asymmetric Encryption

### RSA (Rivest Shamir Adleman)

**Overview**:
- Named after inventors
- Most common asymmetric algorithm
- Based on factorization problem
- 1024-bit, 2048-bit, 4096-bit typical

**Usage**:
- Digital signatures
- Key exchange
- Encryption

### DSS (Digital Signature Standard)

**Description**:
- Contains DSA (Digital Signature Algorithm)
- Used primarily for signatures
- Government standard
- 1024-bit keys typical

### ECC (Elliptic Curve Cryptography)

**Characteristics**:
- Newer approach
- Smaller keys for same security
- 256-bit ECC ≈ 2048-bit RSA security
- Growing in adoption

### Diffie-Hellman

**Purpose**:
- Key exchange algorithm
- Allows shared secret over insecure channel
- Not for encryption directly
- Foundation for many protocols

---

## Advantages of Asymmetric Encryption

### Advantage 1: Key Distribution Not Necessary

**Benefit**: No need for secure key distribution

**How**:
- Public key can be shared anywhere
- No risk from public key exposure
- Only private key must be protected

**Implication**: Solves fundamental key distribution problem

### Advantage 2: Exchange of Private Keys Not Necessary

**Benefit**: Private key never transmitted

**How**:
- Private key stays with owner
- Never sent over network
- Never shared with other parties
- Completely controlled by owner

**Security**: Eliminates private key interception risk

### Advantage 3: Digital Signature and Message Authentication

**Benefit**: Enables digital signatures

**Uses**:
- Prove message authenticity
- Sign documents digitally
- Non-repudiation (can't deny signing)
- Message integrity verification

**Not possible with symmetric encryption**: Symmetric doesn't provide signatures

---

## Use Cases for Symmetric Encryption

### Use Case 1: Banking

**Application**: Encrypting credit card information

**Details**:
- Credit card numbers sensitive
- PII (Personally Identifiable Information) protected
- Transactions encrypted
- Fast processing needed

**Why symmetric**: Speed important for real-time transactions

### Use Case 2: Data Storage

**Application**: Encrypting data on device/disk

**Details**:
- Data at rest protection
- Entire hard drive encryption
- Database encryption
- File system encryption

**Why symmetric**: Fast, doesn't need to be transmitted

### Use Case 3: Digital Signatures

**Application**: Confirming identity in documents

**Details**:
- User signs document
- Signature verifies identity
- Proves signer authorization
- Non-repudiation evidence

### Use Case 4: Blockchain

**Application**: Confirming cryptocurrency transactions

**Details**:
- Authorize transactions
- Prove transaction authenticity
- Prevent counterfeiting
- Create permanent record

**Why asymmetric**: Need to prove identity, enable signatures

### Use Case 5: Public Key Infrastructure (PKI)

**Application**: Governing encryption keys

**Details**:
- Manage certificate lifecycle
- Issue digital certificates
- Bind keys to identities
- Revoke compromised keys

**Why asymmetric**: Need identity binding, certificate signing

---

## Introduction to HTTPS

### Why HTTPS Matters

**Problem**: Regular HTTP unencrypted

**Consequences**:
- Passwords visible on network
- Credit card numbers exposed
- Private information accessible
- Vulnerable to eavesdropping

**Solution**: HTTPS adds encryption layer

---

## What is HTTPS

### Definition

**HTTPS** = **Hypertext Transfer Protocol Secure**

**Full name**: HTTP over TLS (or HTTP over SSL)

**Purpose**: Secure version of HTTP protocol

### Encryption Protocol

**Uses**:
- Transport Layer Security (TLS) - modern
- Secure Socket Layer (SSL) - predecessor (deprecated)

### Terminology

**Often called**:
- HTTP over TLS
- HTTP over SSL
- TLS/SSL

**Modern preference**: HTTPS with TLS (SSL deprecated)

### Purpose

**HTTPS makes possible**:
- Transmit sensitive data securely
- Protect credit card numbers
- Protect banking information
- Protect login credentials
- Protect personal information

---

## HTTPS vs HTTP

### HTTP (Unencrypted)

**Characteristics**:
- Data sent in plain text
- Anyone on network can read
- No authentication of server
- No data integrity protection
- Vulnerable to man-in-the-middle

### HTTPS (Encrypted)

**Characteristics**:
- Data encrypted in transit
- Cannot read without decryption key
- Server authenticated via certificate
- Data integrity protected
- Protected against man-in-the-middle

### Visual Comparison

| Aspect | HTTP | HTTPS |
|--------|------|-------|
| **Encryption** | None | AES-256 (symmetric) |
| **Authentication** | None | SSL/TLS certificate |
| **Vulnerability** | Eavesdropping | Requires certificate compromise |
| **Port** | 80 | 443 |
| **Performance** | Slightly faster | Small overhead |

---

## SSL/TLS Protocols

### SSL (Secure Socket Layer)

**History**:
- Predecessor to TLS
- Developed by Netscape
- Versions 1.0, 2.0, 3.0

**Status**: Deprecated (security vulnerabilities)

### TLS (Transport Layer Security)

**History**:
- Successor to SSL
- IETF standard (RFC 5246)
- Versions: 1.0, 1.1, 1.2, 1.3
- Latest: TLS 1.3 (2018)

**Status**: Current standard

### Security Evolution

```
SSL 1.0 (abandoned)
    ↓
SSL 2.0 (withdrawn due to flaws)
    ↓
SSL 3.0 (deprecated in 2015)
    ↓
TLS 1.0 (deprecated)
    ↓
TLS 1.1 (deprecated)
    ↓
TLS 1.2 (current, widely used)
    ↓
TLS 1.3 (latest, enhanced security)
```

---

## Symmetric and Asymmetric in HTTPS

### Hybrid Approach

**HTTPS uses both types**:
1. **Asymmetric encryption**: For initial key exchange and authentication
2. **Symmetric encryption**: For actual data transmission

### Why Hybrid

**Reasons for combining**:
- Asymmetric: Solves key distribution problem
- Symmetric: Much faster for bulk data
- Best of both worlds

### The Process

```
Step 1-4: Asymmetric encryption (establish secure channel)
          └─ Exchange keys, authenticate server
    ↓
Step 5+:  Symmetric encryption (efficient data transfer)
          └─ Use session key for fast encryption
```

---

## Session Keys

### Definition

**Session key**: Random key generated when session created

**Characteristics**:
- Unique to each connection
- Randomly generated (cryptographically secure)
- Used for that session only
- Different key for each new connection

### Purpose

**Why session keys**:
- Efficiency: Symmetric encryption very fast
- Security: Different key each session (forward secrecy)
- Isolation: Compromise of one doesn't affect others
- Freshness: New key for each connection

### Duration

**Session key valid for**:
- Duration of one connection
- Discarded after connection closes
- Not reused in future sessions
- New key generated for new connection

---

## HTTPS Working: Step-by-Step

### Complete HTTPS Handshake

**Five main steps** with example analogies to banking

---

## Step 1: HTTPS Connection Request

### What Happens

**Browser requests secure connection**:

```
Browser → Server: "Please give me a secure connection"
```

### Technical Details

**When user types**: `https://nameofthesite.com`

**Process**:
1. Browser recognizes HTTPS protocol
2. Initiates secure connection
3. Connects to port 443 (HTTPS port)
4. Regular HTTP uses port 80

### Banking Analogy

**Equivalent to**: Calling bank and saying "I want to withdraw cash but I can't go physically to the bank"

**Meaning**:
- Customer wants service
- Customer wants security
- Customer not physically present
- Need secure remote method

---

## Step 2: Public Key Transmission

### What Happens

**Server sends public key to browser**:

```
Server → Browser: "Here's my public key (and certificate)"
```

### Key Details

**Server transmits**:
- Server's public key
- Digital certificate (signed by CA)
- Server's identity information

### Purpose

**Public key usage**:
- Used to encrypt information
- Only server can decrypt (has private key)
- Browser uses to send session key securely

### Banking Analogy

**Equivalent to**: Bank sends cheque book with validated security

**Meaning**:
- Bank sends cheque leaves (public key)
- Cheque has special chip for validation
- Only bank's special scanner can read (private key)
- Customer uses cheque to communicate securely

---

## Step 3: Session Key Generation

### What Happens

**Browser generates session key**:

```
Browser (locally): Generate random session key
```

### Technical Details

**Process**:
1. Browser generates random number
2. Uses cryptographically secure randomness
3. This becomes the session key
4. Will use for symmetric encryption

### Purpose

**Session key for**:
- Fast encryption of actual data
- Symmetric encryption (much faster than asymmetric)
- Duration of this connection only
- New key for next connection

### Banking Analogy

**Equivalent to**: Customer validates cheque with biometric (retina scan)

**Meaning**:
- Customer activates special chip on cheque
- Creates self-payee cheque
- Sets up standing instruction (SI) for 7 days
- Session key: "for next 7 days, this is our shared secret"

---

## Step 4: Session Key Encryption

### What Happens

**Browser encrypts session key with server's public key**:

```
Browser: "Here's session key, encrypted with your public key"
```

### Technical Details

**Process**:
1. Browser takes session key (random number)
2. Encrypts it using server's public key
3. Only server can decrypt (has private key)
4. Sends encrypted session key to server

### Security Guarantee

**Encryption ensures**:
- Session key transmitted securely
- Cannot be read by eavesdropper
- Only server can decrypt (has private key)
- Session key never revealed

### Banking Analogy

**Equivalent to**: Customer sends validated cheque leaves

**Meaning**:
- Customer sends cheque with activated chip
- Only bank's scanner can read activation
- Bank validates customer's identity
- Cheque can't be forged (chip is unique)

---

## Step 5: Switch to Symmetric Encryption

### What Happens

**Both switch to symmetric encryption**:

```
After Step 4: Both have session key
    ↓
Browser and Server: "We're both using this session key for encryption"
    ↓
All subsequent data: Encrypted with session key (symmetric)
```

### Technical Details

**After key exchange**:
1. Server decrypts session key
2. Both have identical session key
3. Both switch to symmetric encryption
4. All data uses this session key

### Efficiency Gain

**Why this works**:
- Asymmetric encryption slow (RSA operations complex)
- Symmetric encryption very fast (AES operations simple)
- Asymmetric only needed for key exchange
- Rest of communication uses fast symmetric

### Banking Analogy

**Equivalent to**: Bank starts processing withdrawal

**Meaning**:
- Bank scanned and validated customer's chip
- Bank knows customer is legitimate
- Now bank processes automatically for 7 days
- No need for chip validation each time
- Just process the standing instruction

---

## Banking Analogy

### Scenario Setup

**Customer wants to withdraw money**:
- Cannot go to bank physically
- Needs to withdraw through secure remote method
- Bank needs to verify customer identity

### Step-by-Step Analogy

#### Step 1: Initiation

**Customer action**: Calls relationship manager

**What they say**: "I want to withdraw cash but I can't come physically"

**HTTPS equivalent**: Browser requests HTTPS connection

#### Step 2: Bank Sends Cheque Book

**Bank sends**: Cheque book to customer

**Special features**:
- All cheques have security chip
- Chip activated with customer's retina scan
- Only bank's scanner can read activation

**HTTPS equivalent**: Server sends public key and certificate

#### Step 3: Customer Validates Cheque

**Customer action**:
1. Scans cheque with retina
2. Activates security chip
3. Makes cheque self-payee
4. Sets up SI (standing instruction) for 7 days

**HTTPS equivalent**: Browser generates session key

#### Step 4: Customer Sends Validated Cheque

**Customer action**: Sends cheque leaves to bank

**Purpose**: Prove customer has authorized cheques

**HTTPS equivalent**: Browser sends encrypted session key

#### Step 5: Bank Processes Standing Instruction

**Bank action**:
1. Scans activated chip
2. Verifies customer identity
3. Accepts standing instruction
4. Processes automatic withdrawal for 7 days

**HTTPS equivalent**: Both switch to symmetric encryption using session key

#### After Initial Setup

**For next 7 days**: 
- Bank automatically withdraws based on SI
- No need to send validated cheques each time
- Much faster processing
- Already verified customer

**HTTPS equivalent**: 
- All subsequent data encrypted with session key
- No more asymmetric encryption needed
- Much faster (symmetric operations)
- Already authenticated server

---

## How Each Encryption Type is Used

### When Asymmetric is Used

**In HTTPS**:
- Initial handshake phase
- Server authentication
- Session key exchange
- Certificate verification

**Duration**: Only during handshake (few hundred milliseconds)

### When Symmetric is Used

**In HTTPS**:
- Actual data transmission
- After session key established
- All application data
- Duration of entire connection

**Duration**: Entire connection lifetime (could be minutes to hours)

### Why This Hybrid Model

**Reasons**:
- Asymmetric: Solves key distribution, authenticates server
- Symmetric: Much faster for bulk data transmission
- Combined: Best security with good performance

**Result**: Secure and efficient HTTPS connections

---

## Security Considerations

### Certificate Security

**HTTPS relies on**:
- Valid SSL/TLS certificate
- Certificate signed by trusted CA
- Certificate not expired
- Certificate matches domain

**If compromised**: Attacker can impersonate server

### Perfect Forward Secrecy (PFS)

**Modern HTTPS**:
- Uses PFS (TLS 1.3 default)
- Even if private key compromised
- Past sessions remain secure
- Session keys not derivable

### Key Size Recommendations

**Current best practices**:
- RSA: 2048-bit minimum, 4096-bit recommended
- ECC: 256-bit minimum, 384-bit recommended
- Session keys: 128-bit minimum (AES-128), 256-bit recommended

### Regular Updates

**Critical**:
- Keep browsers updated
- Keep OS updated
- Update TLS libraries
- Remove deprecated protocols

---

## Summary

### Key Concepts

1. **Symmetric encryption**: One key for both encryption and decryption
2. **Asymmetric encryption**: Different keys for encryption and decryption
3. **HTTPS**: HTTP over TLS/SSL for secure web communication
4. **Hybrid approach**: Asymmetric for key exchange, symmetric for data
5. **Session keys**: Random symmetric keys for each connection
6. **Five-step handshake**: Establish secure encrypted channel

### Critical Points

- HTTPS uses both symmetric and asymmetric encryption
- Asymmetric solves key distribution problem
- Symmetric provides fast encryption for data
- Session keys are random and unique per connection
- Server authenticated via SSL/TLS certificate
- All data encrypted after handshake

### Modern HTTPS

- TLS 1.3 is current standard (2018)
- Perfect Forward Secrecy (PFS) protects past sessions
- High cipher strength (AES-256, ECDHE-RSA)
- Regular security updates essential
- Widely deployed on modern web

HTTPS represents the practical application of asymmetric and symmetric cryptography together, enabling secure communication on the internet. Understanding both algorithms' roles is essential for cybersecurity knowledge.
