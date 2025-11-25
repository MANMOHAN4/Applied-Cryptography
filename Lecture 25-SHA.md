# Secure Hash Algorithm (SHA)
## Lecture 25 - Applied Cryptography (UE23CS342AA4)

---

## Table of Contents
1. [Introduction to SHA](#introduction-to-sha)
2. [History and Development](#history-and-development)
3. [SHA-1 Overview](#sha-1-overview)
4. [SHA Family Comparison](#sha-family-comparison)
5. [SHA-1 Specifications](#sha-1-specifications)
6. [Input Processing](#input-processing)
7. [Chaining Variables](#chaining-variables)
8. [SHA-1 Processing Blocks](#sha-1-processing-blocks)
9. [One Iteration of SHA](#one-iteration-of-sha)
10. [Process P in Each Round](#process-p-in-each-round)
11. [Working Procedure](#working-procedure)
12. [Round Constants](#round-constants)
13. [Rounds 0-79](#rounds-0-79)
14. [Final Round Addition](#final-round-addition)
15. [Complete Algorithm Steps](#complete-algorithm-steps)
16. [Security Status](#security-status)
17. [SHA-1 vs SHA-2 vs SHA-3](#sha-1-vs-sha-2-vs-sha-3)
18. [Applications](#applications)

---

## Introduction to SHA

### What is SHA?

**SHA** stands for **Secure Hash Algorithm**, a family of cryptographic hash functions designed to produce secure message digests.

### Purpose

SHA hash functions are used to:
- Create message digests (fingerprints) of data
- Ensure data integrity
- Support digital signature schemes
- Provide authentication

### Key Characteristic

Like all cryptographic hash functions, SHA algorithms:
- **Map arbitrary-length input** to **fixed-length output**
- Are **deterministic** (same input → same output)
- Are **one-way functions** (infeasible to reverse)
- Have **collision resistance** (hard to find two inputs with same hash)

---

## History and Development

### Timeline

**1993**: 
- **Original SHA** designed by **NIST (National Institute of Standards and Technology)** and **NSA (National Security Agency)**
- Designated as **FIPS 180**

**1995**:
- SHA was **revised** and released as **SHA-1**
- Designated as **FIPS 180-1**
- Also published as **Internet RFC 3174**

### Key Points

**Standard Names**:
- **Algorithm name**: SHA (Secure Hash Algorithm)
- **Standard name**: SHS (Secure Hash Standard)

**Design Basis**:
- SHA-1 is based on the design of **MD5**
- Incorporates **key differences** to improve security
- Addressed weaknesses found in earlier designs

**Original Purpose**:
- **US standard for use with DSA (Digital Signature Algorithm)**
- Designed to work with digital signature schemes

### Security Concerns (2005)

**Recent developments**:
- In **2005**, security results raised **concerns about SHA-1's future use**
- Researchers found **theoretical weaknesses**
- Led to development of stronger alternatives

---

## SHA-1 Overview

### Definition

**SHA-1** (**Secure Hash Algorithm 1**) is a **cryptographic algorithm** that takes an **input** and produces a **160-bit (20-byte) hash value**.

### Output Characteristics

**Hash Value (Message Digest)**:
- **Size**: 160 bits (20 bytes)
- **Hexadecimal representation**: 40 digits long
- **Example**: `da39a3ee5e6b4b0d3255bfef95601890afd80709` (for empty input)

**Format**:
```
Input: Arbitrary length data
↓
SHA-1 Algorithm
↓
Output: 160-bit hash (40 hex digits)
```

### Technical Specifications

**Federal Standard**:
- **FIPS PUB 180-1** (Federal Information Processing Standards)
- **U.S. Federal Information Processing Standard**
- Designed by **United States National Security Agency**

### Security Status

**Declared Insecure**: **2005**
- Security weaknesses discovered
- Researchers found theoretical collision attacks
- Deprecation began in 2005

**Browser Phase-Out**: **2017**
- **Microsoft, Google, Apple, Mozilla** browsers stopped accepting **SHA-1 SSL certificates**
- Marked the end of practical SHA-1 use in many applications

### Current Use

**NOT Recommended For**:
- Digital signatures
- Cryptographic applications
- New system designs
- Security-critical operations

**Still Used For**:
- Git version control (legacy)
- Legacy systems
- Non-security checksums
- Backwards compatibility

---

## SHA Family Comparison

### SHA Variants

| Algorithm | Digest Size | Block Size | Rounds | Strength | Status |
|-----------|------------|-----------|--------|----------|--------|
| **SHA-0** | 160 bits | 512 bits | 80 | Weak | Withdrawn (1993) |
| **SHA-1** | 160 bits | 512 bits | 80 | Weak | Deprecated (2005) |
| **SHA-224** | 224 bits | 512 bits | 64 | Good | Supported |
| **SHA-256** | 256 bits | 512 bits | 64 | Strong | **Recommended** |
| **SHA-384** | 384 bits | 1024 bits | 80 | Strong | **Recommended** |
| **SHA-512** | 512 bits | 1024 bits | 80 | Strong | **Recommended** |
| **SHA-3** | 256, 384, 512 | Variable | Variable | Very Strong | Latest Standard (2015) |

### Timeline of SHA Development

```
1993: SHA-0 released (quickly withdrawn)
   ↓
1995: SHA-1 released (160-bit output)
   ↓
2001: SHA-2 family (SHA-224, SHA-256, SHA-384, SHA-512)
   ↓
2005: SHA-1 weaknesses found
   ↓
2010: SHA-1 formally deprecated
   ↓
2015: SHA-3 standardized (newest)
```

---

## SHA-1 Specifications

### Technical Specifications

| Property | Value |
|----------|-------|
| **Digest Size** | 160 bits |
| **Block Size** | 512 bits |
| **Word Size** | 32 bits |
| **Rounds** | 4 rounds |
| **Steps per Round** | 20 steps |
| **Total Steps** | 80 (4 × 20) |
| **Certification** | FIPS PUB 180-4, RFC 3174 |
| **Structure** | Merkle-Damgård construction |
| **First Published** | 1993 (SHA-0), 1995 (SHA-1) |

### Input and Output

**Input**:
- Message of arbitrary length (0 to 2⁶⁴-1 bits)
- Any data type (text, binary, etc.)

**Output**:
- 160-bit fixed-length hash
- Represented as 40 hexadecimal characters
- Also called "message digest"

### Processing Structure

```
Message Input
    ↓
[Padding to 512-bit multiple]
    ↓
[Divide into 512-bit blocks]
    ↓
[Process each block through:]
  - Expand to 80 words
  - Execute 80 rounds (4 rounds × 20 steps)
    ↓
[Final hash output: 160 bits]
```

---

## Input Processing

### Step 1: Message Padding

**Objective**: Prepare message for 512-bit block processing

**Padding Process**:

1. **Append "1" bit** to the message
2. **Append "0" bits** until message length ≡ 448 (mod 512)
3. **Append 64-bit message length** (in bits)
4. **Result**: Message length ≡ 0 (mod 512)

**Padding Example**:
- Original message: 100 bits
- After appending "1": 101 bits
- Pad with "0" to reach 448 mod 512: 448 bits total
- Add 64-bit length: 448 + 64 = 512 bits ✓
- Now ready for processing as one 512-bit block

**Important Note**:
- Padding always performed (even if already 448 mod 512)
- Ensures each message produces unique padded version
- Prevents certain attacks

### Step 2: Divide into 512-bit Blocks

**Process**:
- **Padded message** divided into 512-bit blocks
- Each block labeled: M₁, M₂, M₃, ..., M_n

**Block Structure**:
```
Message block (512 bits) = 16 words of 32 bits
```

**Example**:
- Padded message: 1024 bits
- Number of blocks: 1024 ÷ 512 = 2 blocks
- Blocks: M₁ (512 bits), M₂ (512 bits)

---

## Chaining Variables

### Initialization

**SHA-1 uses five 32-bit chaining variables**:

- **A**: 32-bit variable
- **B**: 32-bit variable
- **C**: 32-bit variable
- **D**: 32-bit variable
- **E**: 32-bit variable

Together: **A, B, C, D, E** form a 160-bit state

### Initial Values (Hexadecimal)

```
H0 = 67452301 (initial value of A)
H1 = EFCDAB89 (initial value of B)
H2 = 98BADCFE (initial value of C)
H3 = 10325476 (initial value of D)
H4 = C3D2E1F0 (initial value of E)
```

**These constants are**:
- Fixed (same for all SHA-1 operations)
- Derived from fractional parts of square roots of first 8 primes
- Used as initialization vectors

### Purpose of Variables

- **Process** intermediate hash values
- **Updated** through 80 rounds
- **Combined** to produce final 160-bit hash

---

## SHA-1 Processing Blocks

### Block Processing Steps

For each 512-bit block:

#### Step 1: Copy to Working Variables

```
a ← A
b ← B
c ← C
d ← D
e ← E
```

Initialize working variables a, b, c, d, e with current chaining variables.

#### Step 2: Divide into Subblocks

- **512-bit block** divided into **16 subblocks**
- **Each subblock**: 32 bits (one word)
- **Labeled**: W₀, W₁, ..., W₁₅

#### Step 3: Process Through Rounds

**SHA-1 Structure**:
- **4 Rounds** (not to be confused with hash rounds)
- **20 steps per round**
- **Total 80 iterations** (4 × 20)

**Each round uses**:
- **512-bit block** (16 words)
- **Working register**: (a, b, c, d, e)
- **Constant K[t]** where t ∈ [0, 79]

---

## One Iteration of SHA

### Iteration Formula

**Each of the 80 iterations** performs:

**abcde ← (e + Pₜ(a) + S⁵(a) + W[t] + K[t]), a, S³⁰(b), c, d**

Where:
- **abcde** = Updated register (5 variables)
- **e** = Previous E value
- **Pₜ(a)** = Logical operation (depends on round t)
- **S⁵(a)** = Circular left shift of a by 5 bits
- **W[t]** = 32-bit word derived from message block
- **K[t]** = Round constant (differs by round)
- **S³⁰(b)** = Circular left shift of b by 30 bits

### Breakdown of Operations

#### 1. Circular Left Shift (Rotation)

**S⁵(a)**: Rotate 32-bit value a left by 5 bits
- Example: `Sⁿ(x)` = rotate x left by n bits

**S³⁰(b)**: Rotate 32-bit value b left by 30 bits

**Purpose**: Distribute bits across all positions

#### 2. Logical Operation Pₜ(a)

Different function used in each 20-step group:

**Round 0-19 (t=0 to 19)**:
- **P(a,b,c) = (a ∧ b) ∨ (¬a ∧ c)**
- Majority-like operation

**Round 20-39 (t=20 to 39)**:
- **P(a,b,c) = a ⊕ b ⊕ c**
- XOR operation

**Round 40-59 (t=40 to 59)**:
- **P(a,b,c) = (a ∧ b) ∨ (a ∧ c) ∨ (b ∧ c)**
- Majority operation

**Round 60-79 (t=60 to 79)**:
- **P(a,b,c) = a ⊕ b ⊕ c**
- XOR operation (same as 20-39)

#### 3. Round Constants K[t]

Different constant for each 20-step group:

```
K[0-19] = 0x5A827999
K[20-39] = 0x6ED9EBA1
K[40-59] = 0x8F1BBCDC
K[60-79] = 0xCA62C1D6
```

#### 4. Word W[t]

**First 16 words** (t=0 to 15):
- W[t] taken directly from message block

**Remaining 64 words** (t=16 to 79):
- W[t] = S¹(W[t-3] ⊕ W[t-8] ⊕ W[t-14] ⊕ W[t-16])
- Derived from previous words using XOR and rotation

### Output of Iteration

After each iteration, values rotate:

```
New e = Previous d
New d = Previous c
New c = Previous b shifted left 30
New b = Previous a
New a = Result of operation
```

---

## Process P in Each Round

### Round-Specific Logic Functions

**Understanding the four different P functions**:

#### Round 1: (a ∧ b) ∨ (¬a ∧ c)

**Logic**: 
- If a is 1: result is b
- If a is 0: result is c
- "Conditional" operation

**Truth Table**:
```
a | b | c | Result
--|---|---|-------
0 | 0 | 0 |   0
0 | 0 | 1 |   1
0 | 1 | 0 |   0
0 | 1 | 1 |   1
1 | 0 | 0 |   0
1 | 0 | 1 |   0
1 | 1 | 0 |   1
1 | 1 | 1 |   1
```

#### Round 2 & 4: a ⊕ b ⊕ c

**Logic**: 
- XOR of all three inputs
- "Parity" operation

#### Round 3: (a ∧ b) ∨ (a ∧ c) ∨ (b ∧ c)

**Logic**: 
- At least two inputs must be 1
- "Majority" operation

---

## Working Procedure

### Complete SHA-1 Algorithm Flow

#### Phase 1: Message Preparation

**Message (M)**:
- The original input message of arbitrary length

**Message Padding**:
- Pad to ensure length ≡ 448 (mod 512)
- Ensures unique padding for each message

#### Phase 2: Block Expansion

**Round Word Computation (W[t])**:
- Padded message divided into 512-bit blocks
- Each block divided into 16 words of 32 bits
- 16 words expanded to 80 words using XOR and rotation
- Formula: W[t] = S¹(W[t-3] ⊕ W[t-8] ⊕ W[t-14] ⊕ W[t-16])

**Expansion Purpose**:
- Ensures message information spreads throughout computation
- Increases computational complexity
- Improves avalanche effect

#### Phase 3: Initialization

**Round Initialize (A, B, C, D, E)**:
- Five 32-bit variables initialized to fixed constants
- H₀ = 67452301
- H₁ = EFCDAB89
- H₂ = 98BADCFE
- H₃ = 10325476
- H₄ = C3D2E1F0

#### Phase 4: Main Computation

**Rounds (0-79)**:
- Main computation loop with 80 iterations
- Four stages (20 iterations each)
- Each stage uses different logical function and constant

**Operations per iteration**:
1. Compute logical function P(a,b,c)
2. Perform circular left shifts (S⁵ and S³⁰)
3. Add previous values, constants, and message words
4. Update a, b, c, d, e variables
5. Rotate variables for next iteration

#### Phase 5: Finalization

**Final Round Addition**:
- After all 80 iterations on a block
- Add results to chaining variables
- If more blocks exist, process next block

**MPX (Multiplexing)**:
- Combine results from all blocks
- Concatenate A || B || C || D || E
- Final result: 160-bit message digest

---

## Round Constants

### K[t] Values

**Four different constants** used in SHA-1:

```
For iterations 0-19:  K = 0x5A827999 = 1518500249 (decimal)
For iterations 20-39: K = 0x6ED9EBA1 = 1859775393 (decimal)
For iterations 40-59: K = 0x8F1BBCDC = 2400959708 (decimal)
For iterations 60-79: K = 0xCA62C1D6 = 3395469782 (decimal)
```

### Purpose of Different Constants

- **Distinguish rounds**: Different constant for each round
- **Security**: Prevents patterns and differential analysis
- **Avalanche**: Ensures small input changes propagate
- **Non-linearity**: Increases complexity of attack

---

## Rounds 0-79

### Structure

**Total iterations**: 80

**Organization**:
- **4 Rounds of 20 iterations each**
- **Round 1**: Iterations 0-19 (Conditional function, K₁)
- **Round 2**: Iterations 20-39 (Parity function, K₂)
- **Round 3**: Iterations 40-59 (Majority function, K₃)
- **Round 4**: Iterations 60-79 (Parity function, K₄)

### Each Round's Computation

**Input to each round**:
- 512-bit message block
- Current values of a, b, c, d, e
- Specific constant K[t]

**During each iteration**:
1. Compute P function (depends on round)
2. Perform shifts and additions
3. Update working variables

**Output from each round**:
- Updated values of a, b, c, d, e
- Passed to next iteration (or next block)

---

## Final Round Addition

### Post-Processing

**After all 80 iterations** on a message block:

```
A ← A + a
B ← B + b
C ← C + c
D ← D + d
E ← E + e
```

Where:
- A, B, C, D, E = Current chaining variables
- a, b, c, d, e = Updated working variables
- Addition is modulo 2³² (32-bit addition)

### Purpose

- **Cascade feedback**: Mix output back into state
- **Ensure continuity**: Link multiple blocks
- **Security**: Prevent independent round attacks

### Multi-Block Messages

**If more blocks exist**:
1. Use updated A, B, C, D, E as input to next block
2. Process next 512-bit block
3. Repeat Final Round Addition

**If no more blocks**:
1. A, B, C, D, E form final hash
2. Concatenate: A || B || C || D || E
3. Result: 160-bit SHA-1 hash

---

## Complete Algorithm Steps

### Step-by-Step Execution

1. **Pad Message**: Add padding so length ≡ 0 (mod 512)
2. **Divide Blocks**: Split padded message into 512-bit blocks
3. **Initialize**: Set A, B, C, D, E to fixed values
4. **For each 512-bit block**:
   a. Expand 16 words to 80 words
   b. Copy A, B, C, D, E to a, b, c, d, e
   c. For t = 0 to 79:
      - Compute iteration formula
      - Update a, b, c, d, e
   d. Add results: A←A+a, B←B+b, etc.
5. **Output**: Concatenate A || B || C || D || E (160 bits)

### Pseudocode

```
function SHA1(message):
    // Step 1: Padding
    padded_message ← pad(message)
    
    // Step 2: Initialize
    A = 0x67452301
    B = 0xEFCDAB89
    C = 0x98BADCFE
    D = 0x10325476
    E = 0xC3D2E1F0
    
    // Step 3: Process each block
    for each 512-bit block M in padded_message:
        W ← expand_words(M)          // 16 to 80 words
        
        a, b, c, d, e ← A, B, C, D, E
        
        for t = 0 to 79:
            K ← get_constant(t)
            P ← get_function(t)
            temp ← (S5(a) + P + e + W[t] + K) mod 2^32
            e ← d
            d ← c
            c ← S30(b)
            b ← a
            a ← temp
        
        A ← (A + a) mod 2^32
        B ← (B + b) mod 2^32
        C ← (C + c) mod 2^32
        D ← (D + d) mod 2^32
        E ← (E + e) mod 2^32
    
    // Step 4: Output
    return A || B || C || D || E  // 160-bit hash
```

---

## Security Status

### Weaknesses Found

**2004-2005**: Theoretical attacks discovered
- Researchers found methods to produce SHA-1 collisions
- Not practical at the time but showed mathematical weaknesses
- Raised serious concerns about future use

**Timeline of Deprecation**:

```
2005: Weaknesses announced
2009: NIST deprecates SHA-1 for new applications
2010: NIST withdraws approval for digital signatures
2017: Major browsers stop accepting SHA-1 certificates
2020: Organizations phase out SHA-1
```

### Why SHA-1 is Broken

**Collision Vulnerabilities**:
- Can generate two different messages with same SHA-1 hash
- Makes digital signatures forgeable
- Undermines integrity guarantees

**Theoretical Complexity**:
- Best collision attack: ~2⁶³ operations
- For 160-bit hash: Should be ~2⁸⁰ for security
- Gap shows weakness

### Current Recommendations

**NEVER use SHA-1 for**:
- Digital signatures
- Cryptographic authentication
- Security-critical operations
- New system designs

**Use instead**:
- **SHA-256**: Minimum recommended (256-bit output)
- **SHA-384**: Higher security needs
- **SHA-512**: Very high security
- **SHA-3**: Newest standard (2015)

---

## SHA-1 vs SHA-2 vs SHA-3

### Comparison Table

| Feature | SHA-1 | SHA-256 (SHA-2) | SHA-512 (SHA-2) | SHA-3 |
|---------|-------|-----------------|-----------------|-------|
| **Output Size** | 160 bits | 256 bits | 512 bits | 256/384/512 bits |
| **Block Size** | 512 bits | 512 bits | 1024 bits | Variable |
| **Rounds** | 80 | 64 | 80 | Variable |
| **Structure** | Merkle-Damgård | Merkle-Damgård | Merkle-Damgård | Sponge |
| **Released** | 1995 | 2001 | 2001 | 2015 |
| **Status** | Deprecated | Strong | Strong | Latest |
| **Speed** | Fast | Moderate | Moderate | Moderate |
| **Collision Resistance** | Broken | Strong | Strong | Very Strong |

### Key Differences

**SHA-1 vs SHA-2**:
- SHA-2 has larger output (256+ vs 160 bits)
- SHA-2 uses different constants and functions
- SHA-2 resists all known attacks
- SHA-2 is recommended replacement

**SHA-2 vs SHA-3**:
- SHA-3 uses different construction (sponge vs Merkle-Damgård)
- SHA-3 not faster but more secure design
- SHA-3 selected after public competition (2012)
- SHA-3 standardized as FIPS 202 (2015)

---

## Applications

### Current Uses of SHA

**Still Secure** (Use SHA-256/512/3):

1. **Git Version Control**:
   - Uses SHA-1 for commits (legacy)
   - Transitioning to SHA-256

2. **Digital Signatures**:
   - Use SHA-256 or better with RSA/ECDSA
   - Critical for authenticity

3. **TLS/SSL Certificates**:
   - Use SHA-256 for signing
   - SHA-1 certificates deprecated

4. **Password Hashing**:
   - Use bcrypt, scrypt, or Argon2
   - Not plain SHA directly

5. **File Integrity**:
   - Checksums for files
   - Detect corruption or tampering

6. **Blockchain**:
   - Bitcoin: Uses SHA-256
   - Ethereum: Uses Keccak-256 (SHA-3 variant)

7. **HMAC** (Hash-based Message Authentication Code):
   - Provides both integrity and authenticity
   - Uses SHA-256 or better

### Legacy Applications

**Phasing Out** (Avoid new use):

- Git commits (migrating to SHA-256)
- Old SSL/TLS certificates
- Legacy systems (must update)
- Old security protocols

---

## Summary

### Key Concepts

1. **SHA = Secure Hash Algorithm**
2. **SHA-1 produces 160-bit hash** from arbitrary input
3. **Based on Merkle-Damgård construction**
4. **Uses 80 iterations across 4 rounds**
5. **Deprecated since 2005** due to collision weaknesses
6. **Replaced by SHA-256, SHA-512, SHA-3**

### Critical Timeline

```
1993: SHA created (quickly withdrawn)
1995: SHA-1 released
2005: Collisions found, declared weak
2017: Browsers stop accepting SHA-1
2020+: Complete phase-out
```

### Practical Recommendations

**For New Systems**:
- Use SHA-256 as minimum
- SHA-512 for higher security
- SHA-3 for cutting-edge applications

**For Authentication**:
- Never use SHA-1
- Always use SHA-256 or better
- Combine with HMAC for MACs

**Migration Path**:
- Legacy systems: Stop using SHA-1
- Intermediate: Move to SHA-256
- Future-proof: Adopt SHA-3

The story of SHA-1 is a cautionary tale: even well-designed cryptographic functions can be broken as computing power increases and cryptanalysis improves. Always use the latest recommended standards.
