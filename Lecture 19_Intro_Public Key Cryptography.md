# Introduction to Public Key Cryptography
## Lecture 19 - Applied Cryptography (UE23CS342AA4)

---

## Table of Contents
1. [Symmetric Key Cryptography: Advantages & Disadvantages](#symmetric-key-cryptography-advantages--disadvantages)
2. [Key Distribution Problem](#key-distribution-problem)
3. [Public Key Cryptography: The Solution](#public-key-cryptography-the-solution)
4. [Historical Background](#historical-background)
5. [Core Concepts](#core-concepts)
6. [Security Mechanisms](#security-mechanisms)
7. [Public Key Requirements](#public-key-requirements)
8. [Trapdoor One-Way Functions](#trapdoor-one-way-functions)
9. [One-Way Functions](#one-way-functions)
10. [Examples of One-Way Functions](#examples-of-one-way-functions)
11. [Mathematical Foundations](#mathematical-foundations)
12. [Key Concepts Summary](#key-concepts-summary)

---

## Symmetric Key Cryptography: Advantages & Disadvantages

### Advantages of Symmetric Key Cryptography
- **Extremely Secure**: Provides strong security when properly implemented
- **Relatively Fast**: Computationally efficient compared to asymmetric methods

### Disadvantages of Symmetric Key Cryptography
- **Key Exchange Problem**: Requires secure distribution of the shared secret key
- **Number of Keys Required**: Each pair of users needs a unique key
- **More Damage if Compromised**: If a single key is compromised, all communications using that key are at risk

---

## Key Distribution Problem

### The Fundamental Issue

In **symmetric key cryptography**, Alice and Bob must share a common secret key. This presents a critical challenge:

> _How do you securely distribute and manage these secret keys?_

Some means of distributing a copy of the secret key requires physical distribution or a secure channel.

### Network Example: Scalability Crisis

Imagine a network where:
- **n users** are connected and any two of them may want to communicate
- Each user needs to securely store **n − 1 different symmetric keys** (one for each other user)
- Total number of keys in the system: **n(n − 1)/2**

**Concrete Example**: For a network connecting **2000 university students**:
- Total number of keys needed: **~2 million keys**

#### Mathematical Calculation Example

Let's calculate the number of keys needed for different network sizes:

**Formula**: Total keys = n(n-1)/2

**Example 1: Small University (100 students)**
- n = 100
- Total keys = 100 × 99 / 2 = 9,900 / 2 = **4,950 keys**
- Each student stores: 99 keys

**Example 2: Large University (2000 students)**
- n = 2000
- Total keys = 2000 × 1999 / 2 = 3,998,000 / 2 = **1,999,000 keys** (~2 million)
- Each student stores: 1,999 keys

**Example 3: Small Network (10 users)**
- n = 10
- Total keys = 10 × 9 / 2 = 90 / 2 = **45 keys**
- Each user stores: 9 keys

This creates a **massive key management problem** with critical questions:

1. **How do you add a new user to the system?** ← Every existing user needs a new key with them
2. **What if a user's key is compromised?** ← Must re-establish keys with all other users
3. **How long should a key be considered valid and how should we refresh them?** ← Key lifecycle management

### Traditional Solution: Trusted Third Party (TTP)

A better approach involves introducing a **Trusted Third Party**:

- Each user has a unique secret key shared with the TTP (not with other users)
- When two users want to communicate:
  - They interact with the TTP to establish a shared secret key
  - This temporary key is called a **session key**
  
**Drawbacks of TTP approach**:
- Requires access and trust in a centralized TTP
- Still need to distribute one key (with TTP) for each user
- TTP becomes a single point of failure
- Performance bottleneck if TTP is unavailable
- Can be serious problems in certain situations

---

## Public Key Cryptography: The Solution

### The Elegant Solution

Public key cryptography offers an **extremely elegant solution** to the key distribution problem:

**Core Idea:**
- Use **two different keys** instead of one shared key
  - One key for **encryption** of plaintext messages
  - Another key for **decryption** of ciphertext messages
  
- **Allow the encryption key to be public!**
  - Anyone with access to the public encryption key can send encrypted plaintext to the receiver
  - Only the owner of the corresponding private key can decrypt

### Birth of Asymmetric Encryption

This concept led to the development of **Asymmetric/Public Key Encryption**, which revolutionized cryptography.

---

## Historical Background

### Early Development (1976-1977)

#### Diffie and Hellman (1976)
- Proposed the concept of public-key cryptography
- Documented in landmark paper: **"New Directions in Cryptography" (1976)**
- Introduced four major concepts:
  1. Public-key encryption schemes
  2. Key distribution systems
  3. Diffie-Hellman key agreement protocol
  4. Digital signature

**Important Note**: Diffie and Hellman _introduced the concepts_ but could **not come up with a practical scheme that actually works**. They wanted a public-key encryption algorithm, but couldn't construct one initially. They looked for something with "the flavor" of public-key cryptography, which led to the development of key agreement protocols.

#### James Ellis (1970)
- Public-key encryption was actually proposed in **1970** in a classified paper by **James Ellis**
- This paper remained classified until **1997**, when it was made public by the British Governmental Communications Headquarters (GCHQ)

#### Rivest, Shamir, and Adleman (1977)
- **Ronald L. Rivest**, **Adi Shamir**, and **Leonard M. Adleman** proposed the **first practical public-key cipher in 1977**
- This algorithm is still secure and widely used today
- Known as **RSA** (from their initials)

### Modern Status

**Public key cryptography (or asymmetric cryptography) is one of the most prominent cryptographic techniques used in present times.**

---

## Core Concepts

### Key Pair Generation

**Users generate a related pair of keys:**
- One key is **public** (can be shared with everyone)
- One key is **private** (must be kept secret)

### Mathematical Property

**Critical property**: The private key **cannot be computed from the public key efficiently**
- This is the foundation of public-key security
- Even with the public key, deriving the private key must be computationally infeasible

### Public Key Characteristics

**Public-key is used for:**
- Encryption of messages
- Verification of digital signatures
- Etc.

**Public-key can be:**
- Distributed freely to anyone
- Anyone can encrypt data for the user
- Anyone can verify the user's signatures

### Private Key Characteristics

**Private-key is used for:**
- Decryption (in asymmetric encryption schemes)
- Signing (in digital signature schemes)
- Etc.

**Private-key must be:**
- Kept private and secure
- Never shared with anyone
- Protected from unauthorized access

---

## Security Mechanisms

### Main Mechanisms Realized with Asymmetric Cryptography

#### 1. Key Distribution
- **Examples**: Diffie-Hellman key exchange, RSA
- **Advantage**: No need for pre-shared secret (key)
- Solves the key distribution problem elegantly

#### 2. Nonrepudiation and Digital Signatures
- **Examples**: RSA, DSA, ECDSA
- **Purpose**: Provide message integrity and authentication
- Sender cannot deny having sent a message

#### 3. Identification
- Using challenge-response protocols with digital signatures
- Enables entity authentication

#### 4. Encryption
- **Examples**: RSA, ElGamal
- Direct encryption of messages using public key

### Major Disadvantage

**⚠️ Computationally Very Intensive**
- Public-key algorithms are approximately **1000 times slower** than symmetric algorithms
- Due to complex mathematical operations and large key sizes
- Makes them poorly suited for bulk data encryption
- Often used in hybrid systems: asymmetric for key exchange, symmetric for bulk data

---

## Public Key Requirements

### The Need for Trapdoor One-Way Functions

For public-key cryptography to work, we need a special type of mathematical function called a **trapdoor one-way function**.

### One-Way Function Definition

A **one-way function** has these properties:

| Property | Description |
|----------|-------------|
| **Y = f(X)** | **Easy to compute** - Given X, computing f(X) is straightforward |
| **X = f⁻¹(Y)** | **Infeasible** - Given Y, finding the original X is computationally impossible |

In simple terms:
- Easy to go **forward** (encrypt)
- Hard to go **backward** (break without key)

### Trapdoor One-Way Function Definition

A **trapdoor one-way function** has special additional property:

| Property | Description |
|----------|-------------|
| **Y = fₖ(X)** (k and X known) | **Easy** - Forward computation with the public key |
| **X = fₖ⁻¹(Y)** (k and Y known) | **Easy** - Inverse computation with the private key (trapdoor) |
| **X = fₖ⁻¹(Y)** (Y known, k unknown) | **Infeasible** - Inverse without the trapdoor information |

**The "Trapdoor"**: The private key acts as trapdoor information that makes the otherwise one-way function easy to invert.

### Practical Requirement

> **A practical public-key scheme depends on a suitable trapdoor one-way function.**

This is the mathematical foundation of all public-key cryptography.

---

## Trapdoor One-Way Functions

### Formal Definition

A **trapdoor one-way function** is a special case of one-way function **(F : X → Y)** that contains a **"trapdoor."**

The term **"trapdoor"** refers to specific information, usually obtained during the process of creating a TOWF, that:
- Can provide an easily computable **inverse** of the function
- Is held secret by only the private key owner
- Remains hidden from the public

### How It Works

**Without trapdoor information (public perspective)**:
- The function behaves as a one-way function
- Computing f(X) is easy
- Computing f⁻¹(Y) is infeasible

**With trapdoor information (private perspective)**:
- Both directions become easy
- Owner can both encrypt and decrypt
- Owner can sign and verify their own signatures

### Visual Concept

```
Public View:
X → [Easy] → Y
Y → [Hard] → X (impossible without trapdoor)

Private View (with trapdoor T):
X ⇄ [Easy] ⇄ Y (can go both directions)
```

---

## One-Way Functions

### Definition of One-Way Function

A **one-way function** f(x) is a function from a set **X** to a set **Y** such that:

1. **f(x) is easy to compute** for all **x ∈ X**
   - Computing the function is fast and straightforward
   
2. **For "essentially all" elements y ∈ Y, it is "computationally infeasible"** to find any **x ∈ X** such that **f(x) = y**
   - Reversing the function is practically impossible
   - Even with unlimited computing power, it would take unreasonable time

### Definition of Trapdoor One-Way Function

A **trapdoor one-way function** f(x) is a one-way function **f : X → Y** such that:

1. **If one knows specific information T** (called the **trapdoor information**):
   - Then it becomes computationally **easy to invert** f
   - For any **y ∈ Y**, it is easy to find an **x ∈ X** such that **f(x) = y**

2. **For anyone without knowledge of T**:
   - f(x) remains a one-way function (hard to invert)

**In public-key cryptography**:
- The **trapdoor information T** is the **private key**
- Without it, the function is one-way
- With it, both encryption and decryption are easy

---

## Types of One-Way Functions

### Strong One-Way Function

A **Strong One-Way Function** is a function which:
- Is **easy to compute**
- Can be **inverted only with negligible probability** on a random input
- Is **hard to invert on all but a negligible fraction** of inputs

**Characteristics**:
- Provides strong security guarantees
- Even random attempts have almost zero chance of success
- Suitable for cryptographic applications

**Example**: For a well-designed hash function, finding even one input that produces a specific output is computationally impossible.

### Weak One-Way Function

A **Weak One-Way Function** is a function which:
- Is **easy to compute**
- Is **slightly hard to invert** for random inputs
- Is **easy to invert on some non-negligible fraction** of the inputs

**Characteristics**:
- Less stringent security requirements than strong one-way functions
- May be easier to invert under certain conditions
- Less suitable for direct cryptographic use
- May need additional mechanisms for security

**Note**: Cryptographic applications typically require **strong one-way functions**, not weak ones.

---

## One-Way Functions and Cryptography

### Theoretical Foundation

The relationship between one-way functions and cryptography is fundamental:

#### 1. Secure Systems Require One-Way Functions
- **Secure encryption schemes** imply/require the existence of one-way functions
- **Secure MAC (Message Authentication Code) schemes** require one-way functions
- Without them, no secure cryptographic system is possible

#### 2. One-Way Functions Enable Broader Construction
Given a one-way function, one can construct:
- **PRG** (Pseudo-Random Generator)
- **PRF** (Pseudo-Random Function)
- **PRP** (Pseudo-Random Permutation)

These in turn allow construction of:
- Secure encryption schemes
- Secure MAC schemes
- Other cryptographic primitives

#### 3. Foundation of Modern Cryptography
> **One-way functions are the foundation of modern cryptography theory**

All provably secure cryptographic systems are built on the assumption that one-way functions exist.

---

## Examples of One-Way Functions

### 1. Hash Functions

**Definition**:
A **hash function H** accepts a **variable-size message M** as input and outputs a **fixed-size representation H(M)** of M.

**One-way properties**:
- **Forward**: Computing H(M) for any message M is easy
- **Backward**: Finding an M that produces a specific hash H(M) is infeasible
  - This property is called **preimage resistance** or **one-wayness**

**Common hash functions**:
- SHA-256
- SHA-3
- MD5 (deprecated)

**Applications**:
- Password storage
- Data integrity verification
- Digital signatures
- Blockchain

### 2. RSA Function

**History**:
- Invented in **1977** by **Ron Rivest**, **Adi Shamir**, and **L. Adleman**
- The letters in "RSA" are from their surnames

**One-way properties**:
- **Forward**: Computing c ≡ m^e (mod n) is fast
- **Backward**: Finding m from c without the private key is equivalent to factoring n
  - Factoring large composite numbers (product of two large primes) is computationally infeasible

**Why it's a trapdoor one-way function**:
- With the private key (d), decryption is easy: m ≡ c^d (mod n)
- Without the private key, decryption is as hard as factoring

**Security basis**: The difficulty of factoring large integers
- If you can factor n into p and q, you can compute the private key
- Factoring a 2048-bit number is believed to be computationally infeasible

---

## Mathematical Foundations

### Euler's Totient Function: Review

The **Euler totient function** (also called Euler's phi function, denoted φ(n)):
- Counts the number of positive integers up to n that are relatively prime (coprime) to n
- **Formula for prime p**: φ(p) = p - 1
- **Formula for n = p × q (p, q distinct primes)**: φ(n) = (p-1)(q-1)

#### Worked Examples: Calculating Euler's Totient Function

**Example 1: φ(7) - Prime Number**

Given: n = 7 (which is prime)

Using formula: φ(p) = p - 1
- φ(7) = 7 - 1 = 6

Verification: Integers coprime to 7 from 1 to 6 are: {1, 2, 3, 4, 5, 6}
- All have gcd(x, 7) = 1
- Count = 6 ✓

**Example 2: φ(10) - Product of Two Distinct Primes**

Given: n = 10 = 2 × 5

Using formula: φ(n) = (p-1)(q-1) where p=2, q=5
- φ(10) = (2-1)(5-1) = 1 × 4 = 4

Verification: Integers coprime to 10 from 1 to 10 are: {1, 3, 7, 9}
- gcd(1, 10) = 1 ✓
- gcd(3, 10) = 1 ✓
- gcd(7, 10) = 1 ✓
- gcd(9, 10) = 1 ✓
- Count = 4 ✓

**Example 3: φ(21) - Product of Two Distinct Primes**

Given: n = 21 = 3 × 7

Using formula: φ(n) = (p-1)(q-1) where p=3, q=7
- φ(21) = (3-1)(7-1) = 2 × 6 = 12

Verification: Integers coprime to 21 from 1 to 21 are: {1, 2, 4, 5, 8, 10, 11, 13, 16, 17, 19, 20}
- Must have gcd(x, 21) = 1
- Count = 12 ✓

**Example 4: φ(15) - Product of Two Distinct Primes**

Given: n = 15 = 3 × 5

Using formula: φ(n) = (p-1)(q-1) where p=3, q=5
- φ(15) = (3-1)(5-1) = 2 × 4 = 8

Integers coprime to 15: {1, 2, 4, 7, 8, 11, 13, 14}
- Count = 8 ✓

### Euler's Theorem

#### Theorem Statement

Given integer **n > 1**, such that **gcd(a, n) = 1**, then:

**aᶠ⁽ⁿ⁾ ≡ 1 (mod n)**

Where:
- **a** is any integer coprime to n
- **φ(n)** is Euler's totient function

**Intuition**: Raising a number to the power of φ(n) "wraps around" to 1 in modular arithmetic.

#### Worked Examples: Applying Euler's Theorem

**Example 1: Verifying aᶠ⁽¹⁰⁾ ≡ 1 (mod 10)**

Given: n = 10, a = 3
- φ(10) = 4 (from previous calculation)
- Need to verify: 3^4 ≡ 1 (mod 10)

Step-by-step calculation:
- 3^1 = 3
- 3^2 = 9
- 3^3 = 27 ≡ 7 (mod 10)
- 3^4 = 81 ≡ 1 (mod 10) ✓

Therefore: 3^4 ≡ 1 (mod 10) ✓

**Example 2: Verifying aᶠ⁽¹⁵⁾ ≡ 1 (mod 15)**

Given: n = 15, a = 2
- φ(15) = 8 (from previous calculation)
- Need to verify: 2^8 ≡ 1 (mod 15)

Step-by-step calculation:
- 2^1 = 2
- 2^2 = 4
- 2^3 = 8
- 2^4 = 16 ≡ 1 (mod 15)
- 2^8 = (2^4)^2 ≡ 1^2 ≡ 1 (mod 15) ✓

Therefore: 2^8 ≡ 1 (mod 15) ✓

**Example 3: Verifying aᶠ⁽²¹⁾ ≡ 1 (mod 21)**

Given: n = 21, a = 5
- φ(21) = 12 (from previous calculation)
- Need to verify: 5^12 ≡ 1 (mod 21)

Step-by-step calculation:
- 5^1 = 5
- 5^2 = 25 ≡ 4 (mod 21)
- 5^3 = 5 × 4 = 20 ≡ 20 (mod 21)
- 5^4 = 5 × 20 = 100 ≡ 16 (mod 21)
- 5^6 = (5^2)^3 ≡ 4^3 = 64 ≡ 1 (mod 21)
- 5^12 = (5^6)^2 ≡ 1^2 ≡ 1 (mod 21) ✓

Therefore: 5^12 ≡ 1 (mod 21) ✓

#### Corollary 1: Computing Multiplicative Inverse

Given integer **n > 1**, such that **gcd(a, n) = 1**, then:

**aᶠ⁽ⁿ⁾⁻¹ mod n** is a multiplicative inverse of **a mod n**

**Meaning**: If you raise a to the power of (φ(n) - 1) and take mod n, you get the multiplicative inverse.

**Worked Example: Finding Multiplicative Inverse**

Find the multiplicative inverse of 3 modulo 10.

Given: a = 3, n = 10
- φ(10) = 4
- Multiplicative inverse = 3^(4-1) mod 10 = 3^3 mod 10

Calculation:
- 3^3 = 27 ≡ 7 (mod 10)

Verification: (3 × 7) mod 10 = 21 mod 10 = 1 ✓
- So 7 is the multiplicative inverse of 3 modulo 10

#### Corollary 2: Congruence of Powers

Given integer **n > 1**, positive integers **x, y**, and **a** with **gcd(a, n) = 1**:

If **x ≡ y (mod φ(n))**, then:

**aˣ ≡ aʸ (mod n)**

**Meaning**: If two exponents are congruent modulo φ(n), they produce the same result when used as exponents in modular arithmetic.

**Worked Example: Congruence of Powers**

**Scenario**: Show that a^5 ≡ a^9 (mod 10) for a = 3 (since 5 ≡ 9 (mod 4))

Given:
- n = 10, a = 3
- φ(10) = 4
- Check if: 5 ≡ 9 (mod 4)?
  - 5 = 1×4 + 1, so 5 ≡ 1 (mod 4)
  - 9 = 2×4 + 1, so 9 ≡ 1 (mod 4)
  - Yes, 5 ≡ 9 (mod 4) ✓

According to Corollary 2: 3^5 ≡ 3^9 (mod 10)

Calculation:
- 3^5 = 243 ≡ 3 (mod 10)
- 3^9 = 3^5 × 3^4 = 243 × 81 ≡ 3 × 1 ≡ 3 (mod 10)

Therefore: 3^5 ≡ 3^9 ≡ 3 (mod 10) ✓

**This is crucial for RSA**:
- RSA uses this property to ensure that encryption followed by decryption gives back the original message
- If e × d ≡ 1 (mod φ(n)), then m^(e×d) ≡ m (mod n)

### Fermat's Little Theorem

A special case of Euler's theorem when n is a prime p:

**aᵖ⁻¹ ≡ 1 (mod p)** where gcd(a, p) = 1

**Applications**:
- Primality testing
- RSA algorithm
- Other cryptographic schemes

**Worked Example: Fermat's Little Theorem**

**Example**: Verify Fermat's Little Theorem for p = 7, a = 3

Given: p = 7 (prime), a = 3
- According to Fermat's Little Theorem: 3^(7-1) ≡ 1 (mod 7)
- Simplifies to: 3^6 ≡ 1 (mod 7)

Calculation:
- 3^1 = 3
- 3^2 = 9 ≡ 2 (mod 7)
- 3^3 = 3 × 2 = 6 ≡ 6 (mod 7)
- 3^4 = 3 × 6 = 18 ≡ 4 (mod 7)
- 3^5 = 3 × 4 = 12 ≡ 5 (mod 7)
- 3^6 = 3 × 5 = 15 ≡ 1 (mod 7) ✓

Therefore: 3^6 ≡ 1 (mod 7) ✓

### Primality Testing

The lecture mentions **primality testing** as an important topic for:
- Generating large prime numbers p and q for RSA
- Verifying that selected numbers are indeed prime

**Methods**:
1. **Deterministic algorithms**: Always give correct answer but slower
2. **Probabilistic algorithms**: Fast but may have small error probability (Miller-Rabin test, Fermat test)

For cryptographic purposes, probabilistic tests with multiple rounds are sufficient.

---

## Public Key Security

### Security Against Brute Force

Like private key schemes, **brute force exhaustive search attack is always theoretically possible**:
- Try all possible keys
- Check which one works

### Why Brute Force Doesn't Work in Practice

**Keys used are too large (>512 bits)**:
- A 512-bit number has 2^512 possible values
- This number is astronomically large
- Even checking 1 billion keys per second would take longer than the age of the universe
- Modern RSA typically uses 2048-4096 bit keys

#### Practical Calculation: Breaking a 512-bit Key

**Assumptions**:
- Computer can check: 1 billion keys per second = 10^9 keys/second
- Key space: 2^512 possible keys

**Calculation**:
- Time = (Total keys) / (Keys checked per second)
- Time = 2^512 / 10^9 seconds

**Converting to years**:
- 2^512 ≈ 1.34 × 10^154
- Time = (1.34 × 10^154) / 10^9 = 1.34 × 10^145 seconds
- Seconds per year ≈ 3.15 × 10^7
- Time in years = (1.34 × 10^145) / (3.15 × 10^7) ≈ 4.25 × 10^137 years

**Comparison**:
- Age of universe ≈ 1.38 × 10^10 years (13.8 billion years)
- Time to break 512-bit key ≈ 4.25 × 10^137 years
- **Ratio**: 4.25 × 10^137 / 1.38 × 10^10 ≈ **3 × 10^127 times the age of the universe**

**Conclusion**: Brute force is completely impractical, even for "smaller" 512-bit keys.

### Security Principle

Security relies on:
> **A large enough difference in difficulty between:**
> - **Easy** (encryption/decryption) problems - solvable quickly with the key
> - **Hard** (cryptanalysis/attacking) problems - infeasible without the key

---

## Summary

### Public Key Ciphers Consist Of

1. **Key generation algorithm**
   - Generates the public/private key pair

2. **Encryption algorithm**
   - Uses the public key to encrypt plaintext

3. **Decryption algorithm**
   - Uses the private key to decrypt ciphertext

### Design Principle

Public key schemes are **designed around computationally hard mathematical problems**:
- Problems that are easy in one direction
- Problems that are hard (infeasible) in the reverse direction without special information

### The Trapdoor Concept

Very **hard to solve without key** (called **trapdoor functions**):

1. **Finding prime factors of large integers**
   - Used in RSA
   - Hard: Factor 2048-bit n = p×q
   - Easy with private key: You know p and q

2. **Solving logarithms in modulo arithmetic**
   - Discrete logarithm problem
   - Used in Diffie-Hellman, ElGamal
   - Hard: Find x such that g^x ≡ y (mod p)
   - Easy with private key: x is known

3. **Solving logarithms on elliptic curves**
   - Elliptic Curve Discrete Logarithm Problem
   - Used in ECDSA, ECDH
   - Hard: Find k such that P = k×G on elliptic curve
   - Easy with private key: k is known

### Key Insight

The same operation that is easy in one direction (encryption with public key) becomes the hard problem (factorization, discrete log) when approached from the other direction (without the private key).

---

## Applications of Public Key Cryptography (Beyond This Lecture)

While not covered in detail in this lecture, public-key cryptography enables:

1. **Secure Communication** without pre-shared keys
2. **Digital Signatures** for authentication and non-repudiation
3. **Key Exchange** protocols (Diffie-Hellman)
4. **Certificate Authority (PKI)** infrastructure
5. **Hybrid Encryption** (combine asymmetric for key exchange + symmetric for bulk data)

---

## Study Tips

### Key Concepts to Remember

1. **Symmetric vs Asymmetric Trade-off**:
   - Symmetric: Fast but key distribution problem
   - Asymmetric: Solves key distribution but slower

2. **The Power of Two Keys**:
   - Public key: Distribute freely
   - Private key: Keep secret
   - Mathematical relationship makes this possible

3. **Why Public Key Works**:
   - Based on trapdoor one-way functions
   - Easy with key, hard without key
   - The "trapdoor" is the private key

4. **Mathematical Foundation**:
   - Euler's theorem ensures RSA works correctly
   - Modular arithmetic properties are essential
   - Prime factorization is the hard problem for RSA

5. **Practical Limitations**:
   - Much slower than symmetric (1000x)
   - Used for key exchange and digital signatures
   - Symmetric used for bulk data encryption

### Common Questions to Ask Yourself

- Why can't we derive the private key from the public key?
- What makes a function a "trapdoor one-way function"?
- How does Euler's theorem relate to RSA decryption?
- What's the hard problem underlying RSA security?
- Why is the totient function φ(n) important for RSA?
- How does the network key scaling problem grow with n?
- Why does brute force fail against large cryptographic keys?
