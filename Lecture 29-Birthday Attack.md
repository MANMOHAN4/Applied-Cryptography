# Birthday Attack - Hash Collision Exploitation
## Lecture 29 - Applied Cryptography (UE22CS342AA4)

---

## Table of Contents
1. [Introduction to Birthday Attack](#introduction-to-birthday-attack)
2. [Random Oracle Model (ROM)](#random-oracle-model-rom)
3. [Message Digest](#message-digest)
4. [Pigeonhole Principle](#pigeonhole-principle)
5. [Birthday Paradox](#birthday-paradox)
6. [The Birthday Paradox Problem](#the-birthday-paradox-problem)
7. [Mathematical Analysis](#mathematical-analysis)
8. [Probability Calculations](#probability-calculations)
9. [Birthday Paradox Formulas](#birthday-paradox-formulas)
10. [Hash Functions Review](#hash-functions-review)
11. [Birthday Attack Mechanics](#birthday-attack-mechanics)
12. [Why Hash in Birthday Paradox](#why-hash-in-birthday-paradox)
13. [How Birthday Attack Works](#how-birthday-attack-works)
14. [Why Birthday Attack Works](#why-birthday-attack-works)
15. [Implications on Different Systems](#implications-on-different-systems)
16. [Real-World Examples](#real-world-examples)
17. [Best Practices to Prevent](#best-practices-to-prevent)
18. [Security Implications](#security-implications)

---

## Introduction to Birthday Attack

### Definition

**Birthday attack** is a type of cryptographic attack that belongs to a class of **brute force attacks**

### Core Concept

**Exploits**: The mathematics behind the **birthday problem** in probability theory

### Key Insight

**The success largely depends** on the **higher likelihood of collisions** found between random attack attempts and a fixed degree of permutations

### What It's Used For

- Abuse communication between parties
- Compromise cryptographic systems
- Find hash collisions
- Forge digital signatures
- Break authentication systems

---

## Random Oracle Model (ROM)

### Definition

**The Random Oracle Model (ROM)** was introduced in **1993 by Bellare and Rogaway**

**Purpose**: An ideal mathematical model for a hash function

### The Metaphor: The Gnome with a Book

**How ROM works** (metaphorically):

**There exists a black box containing a gnome** with:
- A big book
- Some dice

**Process**:

1. **Input data** into the box (arbitrary bit sequence)

2. **Gnome's action** if input is new:
   - Uses dice to generate output uniformly and randomly
   - Writes down the input-output pair in book
   - Returns the output

3. **If input already seen**:
   - Looks up in book
   - Returns same output as before
   - Ensures consistency

**Result**: Perfectly random but deterministic (same input → same output)

### Why ROM?

#### 1. Security Proofs and Simplification

**In ROM**:
- Hash functions assumed to behave like true random functions
- Simplifies mathematical modeling of security
- Makes proving security easier

**Alternative**:
- Real-world hash functions have specific structures
- Structures might have subtle vulnerabilities
- Harder to account for in theoretical proofs

#### 2. Modeling Ideal Behavior

**Real-world hash functions** like SHA-256 or SHA-3:
- Have specific deterministic internal structures
- Might have subtle weaknesses
- Difficult to account for in security proofs

**ROM abstraction**:
- Ignores internal structure
- Models as ideal oracle
- Ensures outputs appear random and independent
- Essential for cryptographic constructions

### ROM Properties

**Ideal assumptions in ROM**:
- **Uniformly random output**:  Every output equally likely
- **Independent outputs**: Different inputs produce uncorrelated outputs
- **Deterministic**: Same input always produces same output
- **Efficiently computable**: Oracle responds in reasonable time

---

## Message Digest

### Definition

**A message digest** is a **cryptographic hash value** containing a string of digits created by a **one-way hashing formula**

### Purpose

**Message digests** are designed to:
- Protect integrity of data or media
- Detect changes and alterations
- Identify modifications to any part of a message

### Key Characteristics

- **Fixed output size** (regardless of input size)
- **One-way function** (cannot reverse)
- **Deterministic** (same input → same output)
- **Unique to content** (different content → different hash)
- **Unforgeable** (cannot create message with specific hash)

---

## Pigeonhole Principle

### Definition

**The pigeonhole principle states**:

> If you have **n+1 objects (pigeons)** and **n groups (pigeonholes)**, then **one group must contain more than one object**.

### General Form

More formally:
- If m objects > n groups
- Then at least one group must contain multiple objects

### Implication for Hashing

**For hash functions**:
- If input space larger than output space
- Must exist collisions (by pigeonhole principle)
- Unavoidable mathematically

### Example

**Hash function**:
- 256-bit output space: 2^256 possible values
- Infinite input space: unlimited messages
- By pigeonhole principle: Must be collisions

**The question**: How hard are they to find?

---

## Birthday Paradox

### The Problem

**Classroom scenario**:
- 30 students and a teacher
- Teacher wants to find pairs with same birthday
- Asks everyone for their birthday
- Seeks matching pairs

**Intuitive expectation**: You'd need many more people for likely matches

**Actual result**: Much fewer people than you'd think!

### The Question

**How many people in a room such that the chance of two people sharing a birthday is 50%?**

**Naive guess**: ~183 people (half of 365)

**Actual answer**: ~**23 people**

**This seems counterintuitive** - hence the name "**Birthday Paradox**"

### Assumptions

For traditional calculation:
1. Non-leap year (365 days)
2. Equal probability of birth on any day
3. Birth dates independent

**Reality**: People more likely born in Aug/Sep, not uniform. But principle still applies.

---

## The Birthday Paradox Problem

### Two Different Questions

#### Question 1: Fixed Date

**"What's the probability at least one person shares a specific date (October 10)?"**

**Answer**: 1 - (364/365)^30 ≈ **7.9%**

**Formula**:
```
P(at least one match to Oct 10) = 1 - P(nobody matches Oct 10)
                                 = 1 - (364/365)^30
```

#### Question 2: Any Date Match

**"What's the probability at least two people share ANY birthday?"**

**Answer**: ≈ **70%** with 30 people

**This is much higher** than Question 1!

**Formula**:
```
P(any two match) = 1 - P(all have different birthdays)
```

### Why the Difference?

**Question 1 constraints**:
- One fixed reference date
- Only comparing others to that date
- Lower probability

**Question 2 freedom**:
- Can match on ANY date
- Comparing all pairs to each other
- Many more possible matches

---

## Mathematical Analysis

### Two-Person Calculation

**For n = 2 (two people)**:

```
P(two have same birthday) = 1 - P(different birthdays)
                          = 1 - (365/365 × 364/365)
                          = 1 - (1 × 364/365)
                          = 1 - 364/365
                          = 1/365
                          ≈ 0.27%
```

### N-Person Calculation

**For n people**:

```
P(at least two share birthday) = 1 - P(all different)

P(all different) = (365/365) × (364/365) × (363/365) × ... × (365-n+1)/365
                = (365! ) / ((365-n)! × 365^n)
```

**Simplified approximation**:
```
P(at least one match) ≈ 1 - e^(-n(n-1)/730)
```

### Key People Counts

| People | Probability |
|--------|-------------|
| 10 | 11.7% |
| 15 | 25.3% |
| 20 | 41.1% |
| 23 | **50.7%** |
| 30 | 70.6% |
| 50 | 97.0% |
| 60 | 99.4% |
| 70 | 99.9% |
| 100 | ~99.9999% |
| 366+ | 100% (by pigeonhole) |

### Pigeonhole Limit

**Important note**:
- For N ≥ 366, probability is **100%**
- By pigeonhole principle, must have match
- With 366 people and 365 days, guaranteed collision

---

## Birthday Paradox Formulas

### General Formula

**For N objects and M slots**:

```
P(collision) = 1 - P(all different)

P(all different) = M/M × (M-1)/M × (M-2)/M × ... × (M-N+1)/M

                 = M! / ((M-N)! × M^N)
```

### Approximation Formula

**For large M and small N/M**:

```
P(collision) ≈ 1 - e^(-N²/(2M))
```

### Square Root Rule

**For 50% probability of collision**:

```
N ≈ √(2M × ln(2)) ≈ 1.177√M
```

**Examples**:
- 365 days: N ≈ √(2 × 365) ≈ 27 (actual: 23, very close!)
- 2^64 hash values: N ≈ √(2^64) = 2^32
- 2^128 hash values: N ≈ √(2^128) = 2^64

---

## Hash Functions Review

### Hash Function Definition

**A hash function H** is a transformation that takes:
- **Input**: Variable-sized data (m)
- **Output**: Fixed-size string called hash value (h = H(m))

### Cryptographic Hash Requirements

1. **Variable-length input**
   - Accept any size input
   - From 1 byte to gigabytes

2. **Fixed-length output**
   - Always same output size
   - 128 bits (MD5), 256 bits (SHA-256), 512 bits (SHA-512)

3. **Easy computation**
   - H(x) relatively fast to compute
   - Reasonable time for practical use

4. **One-way property**
   - H(x) is one-way
   - Cannot reverse: given hash, cannot find input
   - Preimage resistant

5. **Collision-free property**
   - H(x) should be collision-resistant
   - Difficult to find x₁ ≠ x₂ where H(x₁) = H(x₂)
   - **This is where birthday attacks matter**

---

## Birthday Attack Mechanics

### Two Stages

#### Stage 1: Precomputation

**The attacker**:

1. **Generates massive table** of hash values
2. **Pairs each hash** with its input
3. **Creates birthday table** - lookup database
4. **Purpose**: Efficiently find collisions

**Storage**:
- For 2^64 entries
- 64-bit hash + variable input
- Terabytes of storage
- Expensive to create but reusable

#### Stage 2: Collision Search

**The attacker**:

1. **Generates new inputs**
2. **Computes hash values** with same hash function
3. **Looks up** in precomputed birthday table
4. **Searches for match** (collision)

**If match found**:
- Two different inputs: one from table, one newly generated
- Both produce same hash
- Collision discovered
- Attack succeeds

---

## Why Hash in Birthday Paradox

### How It Works

**In birthday attack, attacker**:

1. **Generates variations** of two inputs
   - Input A: Legitimate message
   - Input B: Malicious message

2. **Computes hash values**
   - h_A = H(A)
   - h_B = H(B)

3. **Searches for collision**
   - Generates variations of both
   - Computes hashes
   - Looks for h_A = h_B

4. **Goal**: Find two different inputs with same hash

### Why It Matters

**Once collision found**:
- Two completely different messages
- Produce identical hash values
- Can be exploited to compromise security

---

## How Birthday Attack Works

### Attack Process

**Step 1: Understand vulnerability**
- Recognize that hash output space has limited size
- Understand birthday paradox mathematics
- Know number of attempts needed for collision

**Step 2: Generate candidates**
- Create variations of legitimate message (A)
- Create variations of fraudulent message (B)
- Modify formatting, whitespace, comments, etc.
- Keep cryptographic properties intact

**Step 3: Hash generation**
- Compute H(A₁), H(A₂), H(A₃), ...
- Compute H(B₁), H(B₂), H(B₃), ...
- Store all hash values

**Step 4: Collision search**
- Look for match: H(A_i) = H(B_j)
- Can use precomputed tables
- Use sorting/lookup techniques for efficiency

**Step 5: Exploitation**
- Now have A_i and B_j with same hash
- Can trick signature system or authentication

---

## Why Birthday Attack Works

### The Mathematics

**Probability of collision grows faster** than expected:

**Naive expectation**:
- If 2^n possible hashes
- Need ~2^n attempts to find one
- Extremely secure

**Birthday paradox reality**:
- Only need ~2^(n/2) attempts
- For 128-bit hash: 2^64 attempts instead of 2^128
- Billion times easier!

### The Fundamental Reason

**Comparing pairs**:
- Not comparing against fixed target
- Comparing all pairs against each other
- Number of pairs grows quadratically
- N(N-1)/2 possible pairs
- Makes collisions much more likely

---

## Implications on Different Systems

### Digital Signatures

**Problem**: Signature systems sign hash, not full message

**Attack scenario**:
1. Attacker wants to forge signature
2. Creates legitimate document A
3. Creates fraudulent document B
4. Finds collision: H(A) = H(B)
5. Gets A signed legitimately
6. Forged B has same hash
7. Legitimate signature now validates fraudulent B

**Impact**:
- Completely breaks digital signature security
- Can forge contracts, software, certificates
- Trust system compromised

### Password Storage

**Problem**: Systems store H(password)

**Attack scenario**:
1. Attacker finds collision: H(P₁) = H(P₂)
2. P₁ is legitimate password
3. P₂ is attacker's chosen password
4. Both hash to same value
5. Can login with P₂ even if system validates against H(P₁)

**Impact**:
- Can bypass password authentication
- Less critical than signatures (rainbow tables more practical)
- Still represents security breach

### SSL/TLS Certificates

**Problem**: Certificates use hash of certificate content

**Attack scenario**:
1. Attacker creates certificate for legitimate domain
2. Creates certificate for malicious domain
3. Finds collision in certificate hashes
4. Gets legitimate certificate signed by CA
5. Certificate hash now validates malicious certificate too
6. Can spoof legitimate sites

**Impact**:
- Man-in-the-middle attacks
- Impersonation of legitimate websites
- Users trust spoofed certificates

### Authentication Systems

**Problem**: Challenge-response or HMAC-based auth use hashes

**Attack scenario**:
1. Attacker finds collision in authentication tokens
2. Legitimate token generated
3. Fraudulent token with same hash
4. Both pass authentication
5. Unauthorized access

**Impact**:
- Bypass authentication
- Escalate privileges
- Unauthorized access to systems

---

## Real-World Examples

### 1. SSL/TLS Certificate Attacks (2008)

**What happened**:
- Researchers demonstrated practical MD5 collision attack
- Generated two different SSL certificates
- Both certificates had same MD5 hash
- One for legitimate website, one for attacker site

**Attack details**:
- Used differential cryptanalysis
- Found collisions in MD5 compression function
- Created X.509 certificates with crafted prefixes
- Same serial number, subject, issuer in both

**Impact**:
- Fraudulent certificates could be issued
- Man-in-the-middle attacks possible
- Browsers couldn't distinguish legitimate from fake

**Lesson**: MD5 completely broken for cryptographic purposes

### 2. MD5 Hash Collisions (2004)

**What happened**:
- Researchers discovered practical MD5 collision attack (Wang et al.)
- Could create two files with same MD5 hash
- Previously thought collision-resistant

**Attack details**:
- Chinese cryptanalysis team
- Reduced computation from 2^123 to 2^2
- Made collisions practical

**Impact**:
- MD5 considered broken
- Deprecated for digital signatures
- Files could be forged to have same MD5
- Download verification mechanisms compromised

**Timeline**:
- 2004: Collision algorithm published
- 2005: Faster algorithms developed
- 2006: Collisions generated in seconds
- 2012: Fast practical attacks available

### 3. Bitcoin Private Key Hash (2013)

**What happened**:
- Researchers generated two Bitcoin private keys with same hash
- Used birthday attack on ECDSA
- Could steal Bitcoins

**Attack details**:
- Created malicious transaction
- Used one key to sign
- Replaced with other key (same hash)
- Transaction still validated
- Bitcoins stolen

**Impact**:
- Bitcoin security vulnerability
- Highlighted importance of collision resistance
- Modern cryptocurrencies require secure hashing

### 4. Git Repository Collision (2017)

**What happened**:
- Researchers found SHA-1 collision (first practical attack)
- Git version control relies on SHA-1
- Two different files same hash

**Attack details**:
- Google's team (Shattered project)
- Found two PDF files with same SHA-1
- 2^63 operations (still expensive)
- But proven practical

**Impact**:
- Git infrastructure vulnerable
- Repository integrity compromised
- Prompted migration to SHA-256
- Modern systems now use SHA-256

---

## Best Practices to Prevent

### Practice 1: Adequate Hash Length

**Guideline**: Longer hashes reduce collision probability

**Recommendations by year**:
```
2020: 256 bits minimum for digital signatures
2020: 512 bits for long-term data
2025+: 256+ bits standard
```

**Why**:
- For 128-bit hash: 2^64 collisions needed
- For 256-bit hash: 2^128 collisions needed  
- For 512-bit hash: 2^256 collisions needed
- Exponentially harder

**Current standards**:
- SHA-256: 256 bits (good)
- SHA-512: 512 bits (excellent)
- SHA-3: Variable (256-512 bits common)
- MD5: 128 bits (broken, don't use)
- SHA-1: 160 bits (weakened, deprecated)

### Practice 2: Robust Mathematical Foundation

**Hash functions must**:
- Be built on hard mathematical problems
- Resist all known cryptographic attacks
- Have proven security properties
- Be publicly vetted

**Characteristics of strong algorithms**:
- Avalanche effect (small input change → large output change)
- Non-linearity in operations
- Sufficient complexity
- No known shortcuts

### Practice 3: Frequent Evaluation and Updates

**Ongoing requirements**:
- Continuously evaluate hash functions
- Monitor for vulnerabilities
- Research new attack techniques
- Update as computing power increases

**Actions to take**:
- Migrate to stronger algorithms periodically
- Deprecate weakened algorithms
- Monitor cryptanalysis research
- Plan migration paths
- Test alternatives early

### Practice 4: Use Multiple Hash Functions

**For critical applications**:
- Don't rely on single hash function
- Use hash chaining or parallel hashing
- If one compromised, backup exists
- Provides defense in depth

### Practice 5: Salting (For Password Context)

**For password hashing**:
- Add unique random salt to each password
- Salt stored with hash
- Makes precomputed tables (rainbow tables) ineffective
- Doesn't prevent birthday attacks but helps

### Practice 6: Key Stretching

**For password-derived keys**:
- Use expensive hash functions
- bcrypt, scrypt, Argon2
- Make brute force expensive
- Slows down collision search

---

## Security Implications

### Hash Algorithm Timeline

```
1991: MD4 deployed
1992: MD5 as improvement
1993: SHA (Secure Hash Algorithm) - 160 bits
1998: Weaknesses in SHA found
2000: SHA-1 revised
2004: MD5 collision attack feasible
2005: SHA-1 further weakened
2008: SHA-3 competition started
2011: SHA-3 selected (Keccak)
2017: SHA-1 collision practically demonstrated
2020+: Migration to SHA-256 and SHA-3
```

### Current Situation

**Weak algorithms**:
- **MD5**: Completely broken (don't use)
- **SHA-1**: Broken for collision resistance (deprecated for signatures)
- **Whirlpool**: Still acceptable but slower

**Current standards**:
- **SHA-256**: Recommended for most applications
- **SHA-512**: Strong alternative
- **SHA-3**: Latest standard, excellent security

**Quantum threats**:
- Grover's algorithm halves security
- Hash output must be doubled to maintain security
- 512 bits becomes 256 bits equivalent quantum security

### Security Levels

| Hash | Bits | Security Level | Quantum-Safe |
|------|------|----------------|--------------|
| MD5 | 128 | Broken | No |
| SHA-1 | 160 | Weakened | No |
| SHA-256 | 256 | Strong | 128-bit |
| SHA-512 | 512 | Very Strong | 256-bit |
| SHA-3 | 256-512 | Strong | 128-256-bit |

---

## Summary

### Key Concepts

1. **Birthday paradox** shows collisions more likely than expected
2. **2^(n/2) rule**: For 2^n hash space, ~2^(n/2) attempts needed for collision
3. **Birthday attack** exploits this to find hash collisions
4. **Practical threat** to digital signatures, certificates, authentication
5. **Historical attacks** show real-world exploitation: MD5, SHA-1, Bitcoin

### Critical Takeaways

- Hash function security depends on output size
- Birthday attack is fundamental, not novel cryptanalysis
- 128-bit hashes insufficient by modern standards
- Must use 256+ bit hashes
- Monitor research and update algorithms
- Hash collision is different from preimage attack
- Collision resistance is essential property

### Defense Strategy

1. **Use strong algorithms**: SHA-256, SHA-3
2. **Adequate length**: 256 bits minimum
3. **Regular updates**: Stay current with research
4. **Defense in depth**: Multiple mechanisms
5. **Monitor threats**: Track cryptanalysis progress
6. **Plan migrations**: Pre-emptive algorithm changes

The birthday attack remains one of the most fundamental challenges to cryptographic hash functions and demonstrates why adequate hash size is critical for security.
