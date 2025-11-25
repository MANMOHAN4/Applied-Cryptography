# Cryptographic Hash Functions and MD5
## Lecture 24 - Applied Cryptography (UE23CS342AA4)

---

## Table of Contents
1. [Introduction to Hash Functions](#introduction-to-hash-functions)
2. [What is a Hash Function](#what-is-a-hash-function)
3. [Role in Public Key Cryptography](#role-in-public-key-cryptography)
4. [Properties of Strong Hash Algorithms](#properties-of-strong-hash-algorithms)
5. [Iterated Cryptographic Hash Functions](#iterated-cryptographic-hash-functions)
6. [Merkle-Damgård Scheme](#merkle-damgård-scheme)
7. [Types of Hash Functions](#types-of-hash-functions)
8. [Hash Function Requirements](#hash-function-requirements)
9. [Attacks on Hash Functions](#attacks-on-hash-functions)
10. [MD5 Algorithm](#md5-algorithm)
11. [MD5 History and Development](#md5-history-and-development)
12. [MD5 Construction](#md5-construction)
13. [MD5 Working Process](#md5-working-process)
14. [MD5 Implementation Steps](#md5-implementation-steps)
15. [MD5 vs MD4](#md5-vs-md4)
16. [MD5 Flaws and Vulnerabilities](#md5-flaws-and-vulnerabilities)
17. [MD5 Collisions](#md5-collisions)
18. [Applications of Hash Functions](#applications-of-hash-functions)
19. [Other Hash Functions](#other-hash-functions)

---

## Introduction to Hash Functions

### Why Hash Functions Matter

**A hash function is a serious mathematical process** that holds a **critical role in public key cryptography**.

### Core Applications

1. **Password Storage**: Securely store passwords in a database
   - Store hash of password instead of actual password
   - Database breach reveals hashes, not passwords

2. **Data Integrity**: Ensure data integrity by detecting alteration
   - Compare hash before and after transmission
   - Mismatch indicates data was modified

3. **Secure Authentication**: Make secure authentication possible
   - Challenge-response systems
   - Time-limited credentials

4. **Efficient Organization**: Organize content and files efficiently
   - Hash tables for fast lookup
   - Organize files by content hash

---

## What is a Hash Function

### Definition

**A hash function** is any function that can be used to **map data of arbitrary size to fixed-size values**.

**Mathematical notation**:
```
H: {0,1}* → {0,1}^n
```

Where:
- **Input**: Data of arbitrary length (strings, files, anything)
- **Output**: Fixed-size bit string (typically 128-512 bits)

### Terminology

The values returned by a hash function are called:

| Term | Meaning |
|------|---------|
| **Hash value** | The output result |
| **Hash code** | Another name for hash value |
| **Digest** | Commonly used in cryptography |
| **Message digest** | Emphasizes it summarizes a message |
| **Fingerprint** | Unique identifier of data |

### Formal Definition

**Hash functions** are **mathematical functions** that transform or **map** a given data set into a **bit string of fixed size**, known as the **hash value**.

### Key Characteristic: Determinism

**Always produces identical output for identical input**:
- Hash(message) always returns same value
- Essential for verification and repeatability

---

## Role in Public Key Cryptography

### Multiple Critical Roles

Hash functions play a crucial role in:

#### 1. Digital Signatures

- Signature schemes sign the **hash of a message**, not the message itself
- Reason: Messages can be very large, hashes are fixed size
- Process:
  1. Compute h = H(message)
  2. Sign h with private key
  3. Receiver computes H(received_message)
  4. Verify signature on hash

#### 2. Password Storage

- Systems store H(password) instead of plaintext password
- Authentication: Compare H(provided_password) with stored hash
- Benefit: Database breach doesn't expose passwords

#### 3. Data Integrity Checks

- Compute hash before transmission
- Transmit data with hash separately
- Receiver recomputes hash and compares
- Mismatch indicates corruption or tampering

#### 4. Digital Signature Applications

- Email authentication
- Document certification
- Software distribution verification
- Certificate generation

---

## Properties of Strong Hash Algorithms

A cryptographic hash function must satisfy several critical properties:

### Property 1: Determinism

**Definition**: A hash algorithm should be **deterministic**.

**Meaning**:
- Always returns an **output of identical size** regardless of input size
- Same input always produces same output
- Different inputs might produce different outputs (ideally)

**Examples**:
- MD5: Always produces 128-bit output
- SHA-256: Always produces 256-bit output
- SHA-512: Always produces 512-bit output

### Property 2: Pre-Image Resistance

**Definition**: It's **infeasible to reverse** a hash value to recover the original input plaintext message.

**Formal statement**:
- Given hash value h = H(m)
- It should be impossible to find m from h

**Why it matters**:
- Hashes are **irreversible, one-way functions**
- Even with a hash, you cannot recover the original message
- Essential for password storage security

**Implication for passwords**:
- Even if H(password) is known, attacker cannot directly get password
- Only way to break: Try candidate passwords and compare hashes (brute force)

### Property 3: Collision Resistance

**Definition**: It should be **impossible for two different inputs** to produce the **same hash value**.

**Formal statement**:
- Should NOT exist different messages m₁ ≠ m₂ such that H(m₁) = H(m₂)

**Why it matters**:
- If collision found, hash function is broken
- Attacker could:
  - Sign legitimate document
  - Create fraudulent document with same hash
  - Use legitimate signature on fraudulent document

**Practical implication**:
- MD5 is considered broken (collisions found)
- SHA-1 is considered weakened (collisions theoretically possible)
- SHA-256 and above are still secure

### Property 4: Avalanche Effect

**Definition**: Any **small change in input** results in a **massive change in output**.

**Mathematical statement**:
- Change even one bit in input
- Output should change approximately 50% of its bits on average

**Example**:
```
MD5("hello") = 5d41402abc4b2a76b9719d911017c592
MD5("hallo") = 5d41e5a542a4c76aa0d70ac4d9cec837
(Every bit position differs significantly)
```

**Why it matters**:
- Prevents predictable patterns
- Prevents incremental hash computation
- Makes brute force attacks computationally expensive
- Prevents statistical analysis of hashes

### Property 5: Hash Speed

**Definition**: Hash algorithms should operate at a **reasonable speed**.

**Requirement**:
- Should compute hash values **quickly**
- Must be fast enough for practical use
- Cannot be so slow that systems become unusable

**Trade-offs**:
- Security vs. speed (cryptography always trades)
- Simple hashes are fast but not secure
- Secure hashes are slower but necessary

**Practical examples**:
- MD5: Very fast (now considered weak)
- SHA-1: Fast but weakened
- SHA-256: Slightly slower but secure
- BLAKE2: Very fast AND secure

---

## Iterated Cryptographic Hash Functions

### The Concept

**Iterated hash functions** use a **compression function** applied **repeatedly**.

### Structure

In an **iterated cryptographic hash function scheme**:

1. **Compression Function**: A function with **fixed-size input**
   - Takes n bits and compresses to m bits (typically n > m)
   - Example: 512 bits → 256 bits

2. **Iteration**: Apply compression function **multiple times**
   - Process large message in blocks
   - Each block processed by same compression function
   - Chain results together

### Why Iteration

**Advantages**:
- **Simplicity**: Design one compression function well
- **Scalability**: Handle messages of any size
- **Efficiency**: Process large data in manageable chunks
- **Security**: Iterative structure can be proven secure

### Example Flow

```
Message: [Block 1] [Block 2] [Block 3] ... [Block N]
             ↓         ↓         ↓              ↓
Compress   f(H₀,M₁)  f(H₁,M₂)  f(H₂,M₃)  f(H_{N-1},M_N)
             ↓         ↓         ↓              ↓
Result:     H₁        H₂        H₃   ...      H_N (final hash)
```

---

## Merkle-Damgård Scheme

### What is Merkle-Damgård?

**The Merkle-Damgård Scheme** is an **iterated hash function** used to create message digests by compressing messages.

### History

- **Discovered by**: Ralph Merkle and Ivan Damgård
- **Purpose**: Build collision-resistant cryptographic hash functions from collision-resistant one-way compression functions
- **Implementation**: Used in SHA-1, SHA-256, and other algorithms

### The Four-Step Process

#### Step 1: Message Padding

**Purpose**: Prepare message for block-wise processing

**Process**:
- **Append message length**: Add the bit-length of original message
- **Add padding**: Append 1 bit followed by 0 bits
- **Create multiple of block size**: Padding such that message length becomes divisible by n bits
  - For SHA-256: n = 512 bits
  - For SHA-512: n = 1024 bits

**Requirement**:
- Padding performed even if message already aligned
- Ensures unique padding for different messages
- Prevents certain attacks (length-extension attacks)

**Example for MD5 (512-bit blocks)**:
- Original message: 100 bits
- Add 1-bit: 101 bits
- Add padding zeros: Pad to 448 bits
- Add 64-bit length: Total 512 bits ✓

#### Step 2: Block Division

**Purpose**: Divide padded message into processing blocks

**Process**:
- **Divide into t blocks**: Each block has size n bits (512 for SHA-256)
- **Name blocks**: M₁, M₂, M₃, ..., M_t
- **Create iteration count**: t iterations (one per block)

**Outputs at each iteration**:
- **Digest values**: H₁, H₂, H₃, ..., H_t
- **H_i** is the intermediate hash after processing block i

**Example**:
```
If padded message = 1536 bits, and block size = 512:
t = 1536 / 512 = 3 blocks
Blocks: M₁ (512 bits), M₂ (512 bits), M₃ (512 bits)
Digests: H₁, H₂, H₃ (with H₃ being final hash)
```

#### Step 3: Iterative Compression

**Purpose**: Process each block through compression function

**Process**:
- **For each block i** (from 1 to t):
  - Input to compression function f:
    - H_{i-1}: Digest from previous iteration
    - M_i: Current block
  - **Compute**: H_i = f(H_{i-1}, M_i)

**Initial condition**:
- **H₀**: Initialization vector (constant value)
- For MD5: H₀ = (01 23 45 67, 89 ab cd ef, fe dc ba 98, 76 54 32 10)
- For SHA: Specific IV defined by standard

**Recurrence relation**:
```
H₁ = f(H₀, M₁)
H₂ = f(H₁, M₂)
H₃ = f(H₂, M₃)
...
H_t = f(H_{t-1}, M_t)
```

#### Step 4: Final Hash

**Purpose**: Output the final message digest

**Process**:
- **Take H_t**: Digest after processing final block
- **This is the hash**: H_t is treated as the digest for original message

**Output**:
- Size: n bits (512 for SHA-256, 256 for MD5)
- Unique to input message (with overwhelming probability)

---

## Types of Hash Functions

### Classification by Construction

| Type | Example | Used For |
|------|---------|----------|
| **Merkle-Damgård** | SHA-1, SHA-256, MD5 | General purpose |
| **Block cipher based** | Hash functions built from AES | Alternative construction |
| **Sponge construction** | SHA-3 | Modern alternative |

### Specific Hash Algorithms

1. **Message Digest Series**:
   - MD2, MD4, MD5: Historical (now broken or weakened)

2. **Secure Hash Algorithm**:
   - SHA-1: 160-bit output (weakened)
   - SHA-224, SHA-256, SHA-384, SHA-512: Modern standard
   - SHA-3: Latest standard (2015)

3. **Modern Alternatives**:
   - BLAKE2: Fast and secure
   - Whirlpool: Strong security
   - RIPEMD: Various versions

### Recommendation by Security Level

| Threat Model | Algorithm | Status |
|--------------|-----------|--------|
| **Non-adversarial** | MD5, SHA-1 | OK (CRC also fine) |
| **Weak adversary** | SHA-256 | Good |
| **Strong adversary** | SHA-512, SHA-3 | Strong |
| **Quantum threats** | Quantum-resistant hash | Future-proof |

---

## Hash Function Requirements

### Critical Requirements for Cryptographic Hash Functions

1. **Determinism**: Same input → Same output
2. **Fixed output size**: Regardless of input length
3. **Pre-image resistance**: Cannot reverse hash to find input
4. **Second pre-image resistance**: Cannot find different message with same hash
5. **Collision resistance**: Cannot find two different messages with same hash
6. **Avalanche effect**: Small input change → Large output change
7. **Efficiency**: Computes quickly
8. **Uniform distribution**: Hash values uniformly distributed

---

## Attacks on Hash Functions

### Attack Types

#### 1. Brute-Force Attacks

**Approach**: Try random inputs until hash matches

**Time complexity**: 
- For n-bit hash: O(2^n) tries expected
- 128-bit hash: ~2^128 operations needed

**Practical impact**:
- 128 bits inadequate by modern standards
- 160 bits suspect
- 256+ bits recommended

#### 2. Preimage Attacks

**Task**: Given hash value h, find input m such that H(m) = h

**Complexity**:
- Brute force: O(2^n) for n-bit hash
- Better attacks: Algorithm-dependent

**Impact**: Breaks one-way property

#### 3. Second Preimage Attacks

**Task**: Given message m₁, find different message m₂ such that H(m₁) = H(m₂)

**Complexity**:
- Brute force: O(2^n) for n-bit hash
- Better attacks: Algorithm-dependent

**Impact**: Allows forgery (sign m₁, claim m₂ was signed)

#### 4. Collision Attacks

**Task**: Find any two different messages m₁ ≠ m₂ with H(m₁) = H(m₂)

**Complexity**:
- Birthday paradox: O(2^(n/2)) for n-bit hash
- Value 2^(n/2) determines security against brute-force
- For 128-bit hash: 2^64 ≈ 10^19 operations (still feasible with resources)

**Impact on hash function security**:
- 128-bits: 2^64 collisions (weak)
- 160-bits: 2^80 collisions (suspect)
- 256-bits: 2^128 collisions (secure)

#### 5. Rainbow Tables

**Approach**: Pre-compute hashes of common passwords

**Process**:
1. Pre-compute H(password) for millions of passwords
2. Store as table
3. When given hash, look up in table
4. If found, you have the password

**Protection**: Use salt (random data added to password before hashing)

---

## MD5 Algorithm

### What is MD5?

**The MD5 message-digest algorithm** is a **widely used hash function** that produces a **128-bit hash value**.

### MD5 Status

**Security Status**: **BROKEN** - Has many vulnerabilities

**Current Use**:
- Still used as checksum for unintentional corruption
- **NOT suitable for cryptographic purposes**
- **NOT recommended for new systems**

### Why MD5 Failed

**Requirement Violation**:
- One basic requirement: Impossible to find two messages with same hash
- **MD5 fails this requirement** - collisions can be found

**Practical impact**:
- Collisions can be generated in hours on standard hardware
- Makes digital signatures forgeable
- Completely breaks security for many applications

---

## MD5 History and Development

### Development

**Created by**: Professor Ronald L. Rivest in **1991**

**Standard**: Defined in **RFC 1321**

**Original purpose** (from RFC 1321):

> "MD5 message-digest algorithm takes as input a message of arbitrary length and produces as output a 128-bit "fingerprint" or "message digest" of the input. The MD5 algorithm is intended for digital signature applications, where a large file must be "compressed" in a secure manner before being encrypted with a private (secret) key under a public-key cryptosystem such as RSA."

### Evolution

**Timeline**:
- **1991**: MD5 released
- **1990s**: Widely adopted (SSL/TLS, checksums, passwords)
- **2004**: Wang et al. break MD5 (collisions found)
- **2005**: Faster collision attacks demonstrated
- **2009**: MD5 deprecated by NIST for digital signatures
- **2012+**: Still used for non-cryptographic checksums

---

## MD5 Construction

### Merkle-Damgård Based

**MD5 uses the Merkle–Damgård construction**.

### Message Processing

#### Input Processing

- **Input**: Arbitrary-length message M
- **Output**: Fixed 128-bit digest

#### Blocking Strategy

- **Block size**: 512 bits
- **Block division**: Message divided into 512-bit blocks (16 32-bit words)

#### Padding

- **Requirement**: Message length must be multiple of 512 bits
- **Process**: 
  1. Append bit "1"
  2. Append "0" bits until length ≡ 448 (mod 512)
  3. Append 64-bit message length
  4. Result: Multiple of 512 bits

**Result**: Each block is 512 bits ready for processing

---

## MD5 Working Process

### State Variables

**MD5 maintains a 128-bit state** divided into four 32-bit words:

- **A**: 32-bit register
- **B**: 32-bit register  
- **C**: 32-bit register
- **D**: 32-bit register

**Initialized to constants**:
```
A = 0x67452301
B = 0xefcdab89
C = 0x98badcfe
D = 0x10325476
```

### Processing Steps

#### 1. Load First Block

Each 512-bit message block is divided into 16 32-bit words (M₀ through M₁₅)

#### 2. Process Through Rounds

**4 Rounds**, each consisting of **16 similar operations**:

**Round 1-4 Structure**:
- Each round uses **non-linear function F, G, H, or I**
- **Modular addition** combines values
- **Left rotation** (bit circular shift) scrambles bits
- **Round constants** added to randomize

**Total operations**: 64 (16 operations × 4 rounds)

#### 3. Update State

After processing all 512-bit block:
- **Update A, B, C, D** with results
- These updated values become input for next block

### Circular Shift (Rotation)

**Purpose**: Ensure bits spread across all positions

**Operation**: <<< (bit rotation)
- Rotate left by fixed amount each step
- Different rotation amounts in each round

---

## MD5 Implementation Steps

### Step 1: Append Padding Bits

**Objective**: Make message length ≡ 448 (mod 512)

**Process**:
1. **Append single "1" bit** to message
2. **Append "0" bits** until length = 448 (mod 512)
3. **Minimum padding**: At least 1 bit
4. **Maximum padding**: At most 512 bits

**Note**: Padding always performed, even if message already 448 mod 512

**Example**:
- Message: 50 bits
- After step 1: 51 bits
- Pad with zeros: 448 - 51 = 397 zeros added
- Total: 50 + 1 + 397 = 448 bits ✓

### Step 2: Append Message Length

**Objective**: Add 64-bit representation of original message length

**Process**:
1. **Original length**: Length of message BEFORE padding (in bits)
2. **Representation**: 64-bit big-endian integer
3. **If > 2^64**: Use only low-order 64 bits

**Result after Step 1 + Step 2**:
- Length ≡ 0 (mod 512) bits
- Message ready for processing

**Example**:
- After padding: 448 bits
- Add 64-bit length: 448 + 64 = 512 bits = 1 block ✓

### Step 3: Initialize MD Buffer

**Four-word buffer** (A, B, C, D) initialized to constants in **little-endian**:

```
A: 01 23 45 67
B: 89 ab cd ef
C: fe dc ba 98
D: 76 54 32 10
```

These constants are chosen somewhat arbitrarily (serve as IVs).

### Step 4: Process Message in 16-Word Blocks

**Four Auxiliary Functions**:

```
F(X, Y, Z) = (X ∧ Y) ∨ (¬X ∧ Z)
G(X, Y, Z) = (X ∧ Z) ∨ (Y ∧ ¬Z)
H(X, Y, Z) = X ⊕ Y ⊕ Z
I(X, Y, Z) = Y ⊕ (X ∨ ¬Z)
```

Where:
- ∧ = AND operation
- ∨ = OR operation
- ⊕ = XOR operation
- ¬ = NOT operation

**For each round**:
- Apply corresponding function (F for round 1, G for round 2, etc.)
- 16 operations per round
- Each operation updates one of A, B, C, D

**After processing all blocks**:
- Final values of A, B, C, D form the 128-bit hash

---

## MD5 vs MD4

### Improvements in MD5

1. **Fourth Round Added**
   - MD4 had 3 rounds
   - MD5 added 4th round for more security

2. **Unique Additive Constants**
   - Each step has its own unique constant
   - MD4 reused constants

3. **Function G Modified**
   - MD4 used: G(X,Y,Z) = (X∧Y) ∨ (X∧Z) ∨ (Y∧Z)
   - MD5 uses: G(X,Y,Z) = (X∧Z) ∨ (Y∧¬Z)
   - More asymmetric, less linearity

4. **Each Step Adds Previous Step**
   - Results feed into next step
   - More nonlinearity

5. **Different Block Access Order**
   - Rounds 2 and 3 access message words in different order
   - Disrupts patterns

6. **Optimized Shift Amounts**
   - Shift amounts chosen for maximum avalanche
   - Different in each round for better mixing

### Why These Improvements

**MD4 was too simple**:
- Proven insecure in mid-1990s
- Fast collision attacks possible
- MD5 attempted to fix weaknesses

**MD5 was supposedly stronger**:
- Resisted cryptanalysis longer
- But still broken (collisions in 2004)
- Illustrates cryptanalysis difficulty

---

## MD5 Flaws and Vulnerabilities

### Major Vulnerabilities

#### 1. Rainbow Tables for Password Cracking

**Attack**: Pre-computed tables of MD5 hashes

**Process**:
1. Attacker pre-computes MD5 hashes of common passwords
2. Stores as huge lookup table (gigabytes or more)
3. When given MD5(password), looks up in table
4. If found, password is recovered

**Example sites**:
- MD5 hash databases publicly available
- Millions of pre-computed hashes online

**Defense**: Salt passwords (add random data before hashing)

#### 2. Collisions

**The Fatal Flaw**:

**MD5 can produce collisions** - two different messages with same hash

**Collision attacks timeline**:
- **2004**: Wang et al. demonstrated collision algorithm
- **2005**: Faster algorithm created (Lenstra et al.)
- **Modern**: Collisions can be generated in hours on laptop

**Practical attack**:
1. Create legitimate document A
2. Create malicious document B
3. Find collision: H(A) = H(B)
4. Have A signed legitimately
5. Sign is also valid for B (forgery!)

**Impact**:
- Digital signature schemes broken
- Certificates can be forged
- Complete security failure for signatures

---

## MD5 Collisions

### Timeline

**2004 - Wang et al.**:
- Delivered algorithm for finding collisions
- Required: Few hours on IBM p690 cluster (powerful parallel computer)

**2005 - Lenstra et al.**:
- Improved the attack
- Required: Only few hours on single laptop
- Made attack practical and accessible

**2007 - Stevens et al.**:
- Further improvements
- Can generate collisions in minutes on modern hardware

**Impact**:
- MD5 completely broken for cryptographic use
- Attacks practical for real-world adversaries

### Why Collisions Are So Bad

**For digital signatures**:
- Signature is computed on message hash
- If two messages have same hash, signature valid for both
- Attacker can substitute forged message

**For password storage**:
- Less critical (rainbow tables more practical)
- But collisions could enable rainbow table expansion

**For certificates**:
- Certificate contains message digest of certificate content
- Collision allows creating different certificate with same signature
- Breaks certificate system

### Current Recommendation

**According to cryptographic community**:

> "**SHA or Whirlpool** should be considered until a replacement for MD5 can be found"

- **SHA-256**: Minimum recommended now
- **MD5**: Only for non-cryptographic checksums
- **Never use**: MD5 for digital signatures or authentication

---

## Applications of Hash Functions

### Application 1: Password Storage

**Traditional (BAD)**:
```
Database: username | password
Problem: Database leak = passwords exposed
```

**Hashing (GOOD)**:
```
Database: username | H(password)
Process: User provides password P
Server computes H(P), compares with stored H(password)
Problem: Rainbow tables can recover passwords
```

**Hashing with Salt (BETTER)**:
```
Database: username | salt | H(salt || password)
Process: User provides password P
Server computes H(salt || P), compares with stored value
Benefit: Rainbow tables useless (each password has unique salt)
```

**Modern (BEST)**:
```
Use bcrypt, scrypt, or Argon2 (memory-hard, slow hash functions)
Prevent brute-force of password hashes
```

### Application 2: File Integrity Verification

**Scenario**: Download large file from internet

**Protection**:
1. Sender computes hash: h = H(file)
2. Sender publishes hash on secure channel
3. Downloader downloads file and hash separately
4. Downloader computes h' = H(downloaded_file)
5. Verify: h = h'?

**Detection**: If corrupted, h ≠ h'

**Tool examples**:
- MD5 checksums (for non-adversarial corruption)
- SHA checksums (for integrity)
- Tripwire (file integrity monitoring)

### Application 3: Digital Signatures

**Process**:
1. Message m sent with signature
2. Receiver computes h = H(m)
3. Receiver verifies signature on h using sender's public key
4. If signature valid, message not tampered

**Why hash instead of signing message directly**:
- Messages can be huge (GB files)
- Hash is fixed size (256 bits for SHA-256)
- Signing 256 bits faster than signing GB
- Hash also provides deterministic input to signature algorithm

### Application 4: Pseudo-Random Functions

**Use case**: Need random values from seed

**Method**: 
```
Generate random values using hash:
r₁ = H(seed || 1)
r₂ = H(seed || 2)
r₃ = H(seed || 3)
...
```

**Applications**:
- Generate session keys
- Derive keys from password
- Create nonces for protocols

### Application 5: Proof-of-Work (Blockchain)

**Bitcoin uses hash for Proof-of-Work**:

1. Miners take pending transactions
2. Add random nonce
3. Compute hash: h = H(transactions || nonce)
4. If h < target (many leading zeros), valid
5. Otherwise, increment nonce, try again

**Why it works**:
- Avalanche effect makes finding valid hash hard
- Requires trying many nonces
- Proves computational work

### Application 6: Hash-Based Data Structures

**Hash tables**:
- Use hash to map keys to array indices
- O(1) lookup time

**Merkle trees**:
- Binary tree where each node is hash of children
- Used in blockchain, distributed systems
- Enables efficient verification

---

## Other Hash Functions

### Why Move Beyond MD5?

**MD5 problems**:
- Collisions found
- 128-bit output too small
- Broken for cryptographic use

### Recommended Alternatives

#### SHA-256 (Secure Hash Algorithm)

- **Output**: 256 bits
- **Security**: Strong (not broken)
- **Status**: NIST approved
- **Usage**: Wide deployment (TLS, Bitcoin, etc.)
- **Speed**: Moderate

#### SHA-3

- **Output**: 256, 384, 512 bits
- **Construction**: Sponge-based (different from SHA-2)
- **Status**: NIST approved (2015)
- **Security**: Very strong
- **Speed**: Moderate to slow

#### BLAKE2

- **Output**: 256 or 512 bits
- **Speed**: Very fast (faster than MD5)
- **Security**: Strong
- **Status**: Recommended by security experts
- **Usage**: Growing in modern systems

#### Whirlpool

- **Output**: 512 bits
- **Security**: Strong
- **Speed**: Moderate
- **Status**: Approved by international standards

### Cryptography Libraries

Hash functions available in cryptographic libraries:

- **Botan**: Modern C++ library
- **Bouncy Castle**: Java/C# library
- **cryptlib**: General-purpose library
- **Crypto++**: C++ library
- **Libgcrypt**: GnuPG library
- **Nettle**: Low-level library
- **OpenSSL**: Most widely used
- **wolfSSL**: Embedded library

### Recommended Choices Today

| Application | Use | Avoid |
|-------------|-----|-------|
| **General purpose** | SHA-256 or BLAKE2 | MD5, SHA-1 |
| **Very high security** | SHA-512 or SHA-3 | MD5, SHA-1 |
| **Password hashing** | bcrypt, scrypt, Argon2 | Plain MD5 or SHA |
| **Legacy systems** | SHA-256 | MD5 |
| **Fast checksum** | BLAKE2 | MD5 |
| **New design** | SHA-256, SHA-3, BLAKE2 | MD5, SHA-1 |

---

## Summary

### Key Concepts

1. **Hash Function Purpose**: Map arbitrary data to fixed-size fingerprints
2. **Critical Properties**: One-way, collision-resistant, avalanche effect
3. **Merkle-Damgård**: Iterative structure for processing large messages
4. **MD5**: Fast but broken (collisions found)
5. **Modern alternatives**: SHA-256, SHA-3, BLAKE2

### Hash Functions in Cryptography

Hash functions are essential for:
- **Digital signatures**: Sign hash, not full message
- **Passwords**: Store hashes, not plaintext
- **Integrity**: Detect tampering
- **Authentication**: Challenge-response systems
- **Key derivation**: Derive keys from passwords

### Practical Recommendations

**Use**:
- SHA-256 or SHA-3 for general cryptography
- bcrypt/Argon2 for passwords
- BLAKE2 for performance-critical applications

**Avoid**:
- MD5: Collisions break security
- SHA-1: Weakened, deprecated for digital signatures
- CRC/checksums: Only for non-adversarial use

**For new systems**:
- Choose SHA-256 as minimum baseline
- Use SHA-3 or BLAKE2 for highest security
- Always use salted hash for passwords
- Never design your own hash function

The lesson from MD5: Even well-designed cryptographic systems can be broken. Hash functions should be standardized and publicly vetted before widespread adoption.
