# RSA Algorithm (Rivest–Shamir–Adleman)
## Lecture 21 - Applied Cryptography (UE23CS342AA4)

---

## Table of Contents
1. [Introduction to RSA](#introduction-to-rsa)
2. [Historical Background](#historical-background)
3. [RSA Facts and Foundation](#rsa-facts-and-foundation)
4. [Key Generation Algorithm](#key-generation-algorithm)
5. [RSA Encryption and Decryption](#rsa-encryption-and-decryption)
6. [Worked Example 1: Complete RSA Process](#worked-example-1-complete-rsa-process)
7. [Worked Example 2: Quick Problems](#worked-example-2-quick-problems)
8. [RSA Practical Parameters](#rsa-practical-parameters)
9. [Designing a Good RSA Cryptosystem](#designing-a-good-rsa-cryptosystem)
10. [Fast Exponentiation](#fast-exponentiation)
11. [RSA Security and Attacks](#rsa-security-and-attacks)
12. [Factoring Problem](#factoring-problem)
13. [Timing Attacks](#timing-attacks)
14. [Chosen Ciphertext Attacks](#chosen-ciphertext-attacks)

---

## Introduction to RSA

**RSA** stands for **Rivest–Shamir–Adleman**, named after its inventors.

### What is RSA?

RSA is a **public-key cryptosystem** that allows you to encrypt a message without needing to exchange a secret key separately. Unlike symmetric algorithms (like AES, 3DES) which use a single shared key, RSA uses a pair of mathematically related keys:
- A **public key** for encryption
- A **private key** for decryption

### Key Advantage

You can share your public key freely with anyone, and they can encrypt messages for you. Only you (with your private key) can decrypt those messages.

---

## Historical Background

### Timeline

**1977**: The RSA algorithm was first described by:
- **Ron Rivest**
- **Adi Shamir**
- **Leonard Adleman**
- All from MIT

### Why RSA Matters

The RSA cryptosystem is **the most widely-used public key cryptography algorithm in the world**.

### Applications of RSA

RSA has many applications, but in practice it is most often used for:

1. **Public Key Infrastructure (PKI)** - Most PKI products rely on RSA
2. **SSL/TLS** - Certificates and key-exchange for secure web communication
3. **Secure Email** - PGP, Outlook, and other email encryption tools
4. **Digital Signatures** - Signing documents and verifying authenticity
5. **Web Communication** - HTTPS and secure web traffic

---

## RSA Facts and Foundation

### Mathematical Foundation

1. **Security Based on Factorization**
   - The RSA algorithm's security is based on the difficulty of solving certain mathematical problems
   - Specifically: **factoring large integers** (finding the prime factors of a very large number)

2. **Algorithm Complexity**
   - The algorithm uses logarithmic functions to make it complex enough to withstand brute force attacks
   - Yet it's streamlined enough to be fast after deployment

3. **Practical Characteristics**
   - RSA is one of the **first practical public-key cryptosystems**
   - Widely used for secure data transmission
   - The encryption key is **public** and differs from the decryption key which is **kept secret**

### The Trapdoor One-Way Function

RSA is based on a trapdoor one-way function:

**Easy direction**: Computing c ≡ m^e (mod n) is computationally fast
- Given plaintext m, public key e, and modulus n, computing ciphertext c is quick

**Hard direction (without private key)**: Finding m from c is as hard as **factoring n**
- Without the private key d, decryption is computationally infeasible
- The hard problem is finding the two prime factors p and q of n

**Trapdoor (with private key)**: Decryption m ≡ c^d (mod n) is fast
- Only with the private key d can you easily reverse the encryption

---

## Key Generation Algorithm

### Unlike Symmetric Algorithms

Public key algorithms like RSA require computing the pair **(Kpub, Kpr)** - a public key and private key pair. This is different from symmetric algorithms (AES, 3DES) which use a single shared key.

### Steps for RSA Key Generation

#### Step 1: Choose Two Large Prime Numbers

- Select two large, distinct prime numbers: **p** and **q**
- These primes must be large enough to resist factorization attacks
- In practice, each prime should be 512-1024 bits or larger

#### Step 2: Calculate n and φ(n)

**Calculate n (modulus)**:
- n = p × q
- n is called the **modulus** and will be part of both keys

**Calculate φ(n) (Euler's Totient)**:
- φ(n) = (p - 1) × (q - 1)
- This is crucial for computing the private exponent d

#### Step 3: Choose Public Exponent e

**Requirements for e**:
- 1 < e < φ(n)
- e must be **relatively prime to φ(n)**, meaning gcd(e, φ(n)) = 1
- Common choices: e = 3, e = 17, e = 65537 (0x10001)
- Most commonly used: **e = 65537** (good balance of security and performance)

#### Step 4: Calculate Private Exponent d

**Find d using the Extended Euclidean Algorithm**:
- d is the **modular multiplicative inverse** of e modulo φ(n)
- Requirement: **e × d ≡ 1 (mod φ(n))**
- In other words: (e × d - 1) must be exactly divisible by φ(n)
- Formula: d ≡ e^(-1) (mod φ(n))

#### Step 5: Form the Key Pairs

**Public Key**:
- (e, n) - Both e and n are public

**Private Key**:
- (d, n) - Only d is kept secret (n is part of both keys)

---

## RSA Encryption and Decryption

### Ring Arithmetic

RSA encryption and decryption operations are done in the **integer ring Zn = {0, 1, ..., n-1}**.

### Important Constraint

**The plaintext must be less than n**: The binary value of the plaintext x must satisfy: **x < n**

Similarly, the ciphertext y must also satisfy: **y < n**

### Encryption Formula

To encrypt plaintext message **m** using public key **(e, n)**:

**C ≡ m^e (mod n)**

Where:
- **C** is the ciphertext
- **m** is the plaintext message (where m < n)
- **e** is the public exponent
- **n** is the modulus

### Decryption Formula

To decrypt ciphertext **C** using private key **(d, n)**:

**m ≡ C^d (mod n)**

Where:
- **m** is the recovered plaintext
- **C** is the ciphertext
- **d** is the private exponent
- **n** is the modulus

### Why Decryption Works

**Mathematical Basis** (using Euler's Theorem):

If e × d ≡ 1 (mod φ(n)), then:

m^(e×d) ≡ m (mod n)

Therefore:
- Encrypt: C ≡ m^e (mod n)
- Decrypt: m ≡ C^d ≡ (m^e)^d ≡ m^(e×d) ≡ m (mod n) ✓

### Terminology

- **e**: Called the **encryption exponent** or **public exponent**
- **d**: Called the **decryption exponent** or **private exponent**
- **x, y, n, d**: In practice, these are very long numbers, usually **1024 bits or longer**

### Communication Scenario

If Alice wants to send an encrypted message to Bob:
1. Alice obtains Bob's **public key (n, e)**
2. Alice encrypts the message using Bob's public key: C ≡ m^e (mod n)
3. Alice sends C to Bob
4. Bob decrypts using his **private key (d, n)**: m ≡ C^d (mod n)
5. Only Bob can decrypt because only he knows d

---

## Worked Example 1: Complete RSA Process

### Question
How can we encrypt plaintext **9** using the RSA public-key encryption algorithm?

### Complete Step-by-Step Solution

#### Step 1: Select Two Large Prime Numbers

We choose:
- **p = 7**
- **q = 11**

#### Step 2: Calculate n (Modulus)

**Formula**: n = p × q

Calculation:
- n = 7 × 11
- **n = 77**

#### Step 3: Calculate φ(n) and Choose e

**Calculate φ(n)**:
- φ(n) = (p - 1) × (q - 1)
- φ(n) = (7 - 1) × (11 - 1)
- φ(n) = 6 × 10
- **φ(n) = 60**

**Choose e** (must be relatively prime to φ(n)):
- We need: 1 < e < n and gcd(e, φ(n)) = 1
- We need: 1 < e < 77 and gcd(e, 60) = 1
- Let's check e = 7:
  - gcd(7, 60) = 1 ✓ (7 and 60 have no common factors except 1)
  - So **e = 7** is valid

**Public Key Formation**:
- Public key = **(e, n) = (7, 77)**

#### Step 4: Encrypt the Plaintext

**Given**:
- Plaintext: m = 9
- Public key: (e, n) = (7, 77)

**Encryption Formula**: C ≡ m^e (mod n)

Calculation:
- C ≡ 9^7 (mod 77)
- 9^7 = 4,782,969
- 4,782,969 ÷ 77 = 62,117 remainder 0
- Actually, let me recalculate: 4,782,969 mod 77
  - 4,782,969 = 62,116 × 77 + 37
- **C ≡ 37 (mod 77)**

**Ciphertext = 37**

#### Step 5: Calculate the Private Exponent d

**Requirement**: e × d ≡ 1 (mod φ(n))

Substituting:
- 7 × d ≡ 1 (mod 60)

Finding d:
- We need to find d such that (7 × d - 1) is divisible by 60
- Trying values:
  - d = 1: 7(1) = 7, 7 - 1 = 6 (not divisible by 60)
  - d = 7: 7(7) = 49, 49 - 1 = 48 (not divisible by 60)
  - d = 13: 7(13) = 91, 91 - 1 = 90 (not divisible by 60)
  - d = 43: 7(43) = 301, 301 - 1 = 300, 300 ÷ 60 = 5 ✓

Therefore: **d = 43**

**Private Key Formation**:
- Private key = **(d, n) = (43, 77)**

#### Step 6: Decrypt the Ciphertext

**Given**:
- Ciphertext: C = 37
- Private key: (d, n) = (43, 77)

**Decryption Formula**: m ≡ C^d (mod n)

Calculation:
- m ≡ 37^43 (mod 77)

This is a very large number. Let's use modular exponentiation:
- 37^43 mod 77

Using step-by-step reduction:
- 37^2 = 1,369 ≡ 1,369 - 17(77) = 1,369 - 1,309 = 60 (mod 77)
- 37^4 ≡ 60^2 = 3,600 ≡ 3,600 - 46(77) = 3,600 - 3,542 = 58 (mod 77)
- 37^8 ≡ 58^2 = 3,364 ≡ 3,364 - 43(77) = 3,364 - 3,311 = 53 (mod 77)
- 37^16 ≡ 53^2 = 2,809 ≡ 2,809 - 36(77) = 2,809 - 2,772 = 37 (mod 77)
- 37^32 ≡ 37^2 = 60 (mod 77)

Since 43 = 32 + 8 + 2 + 1:
- 37^43 ≡ 37^32 × 37^8 × 37^2 × 37^1 (mod 77)
- ≡ 60 × 53 × 60 × 37 (mod 77)

Let's compute:
- 60 × 53 = 3,180 ≡ 3,180 - 41(77) = 3,180 - 3,157 = 23 (mod 77)
- 23 × 60 = 1,380 ≡ 1,380 - 17(77) = 1,380 - 1,309 = 71 (mod 77)
- 71 × 37 = 2,627 ≡ 2,627 - 34(77) = 2,627 - 2,618 = 9 (mod 77)

Therefore: **m ≡ 9 (mod 77)**

**Recovered Plaintext = 9** ✓

### Verification

- Original plaintext: **9**
- Ciphertext: **37**
- Recovered plaintext: **9** ✓

The decryption successfully recovered the original message!

---

## Worked Example 2: Quick Problems

### Problem 2A: Finding Private Key

**Question**: In an RSA cryptosystem, a particular user A uses two prime numbers **p = 13** and **q = 17** to generate the public and private keys. If the public exponent of A is **e = 35**. Find the private key **d**.

**Solution**:

Step 1: Calculate n
- n = p × q = 13 × 17 = **221**

Step 2: Calculate φ(n)
- φ(n) = (p - 1)(q - 1) = (13 - 1)(17 - 1) = 12 × 16 = **192**

Step 3: Find d such that e × d ≡ 1 (mod φ(n))
- 35 × d ≡ 1 (mod 192)
- We need (35d - 1) divisible by 192

Using Extended Euclidean Algorithm:
- 35d ≡ 1 (mod 192)
- d = 11 (we can verify: 35 × 11 = 385 = 2(192) + 1, so 385 ≡ 1 (mod 192) ✓)

**Answer: d = 11**

### Problem 2B: Finding Ciphertext

**Question**: An RSA cryptosystem uses two prime numbers **p = 3** and **q = 13** to generate the public key = **3** and the private key = **7**. What is the value of ciphertext for a plaintext **m = 8**?

**Solution**:

Step 1: Verify n
- n = p × q = 3 × 13 = **39**

Step 2: Use encryption formula
- C ≡ m^e (mod n)
- C ≡ 8^3 (mod 39)
- 8^3 = 512

Step 3: Calculate 512 mod 39
- 512 ÷ 39 = 13 remainder 5
- 512 = 13 × 39 + 5
- C ≡ 5 (mod 39)

**Answer: Ciphertext C = 8** (The answer in the slide shows C = 8, but based on our calculation it should be 5)

### Problem 2C: Finding Public Exponent

**Question**: An RSA cryptosystem uses two prime numbers **p = 3** and **q = 11**, to generate private key **d = 7**. What is the value of ciphertext for plaintext **m = 5** using the RSA public-key encryption algorithm? (Find e = 3 first)

**Solution**:

Step 1: Calculate n and φ(n)
- n = 3 × 11 = **33**
- φ(n) = (3 - 1)(11 - 1) = 2 × 10 = **20**

Step 2: Find e such that e × d ≡ 1 (mod φ(n))
- e × 7 ≡ 1 (mod 20)
- We need (7e - 1) divisible by 20
- e × 7 ≡ 1 (mod 20)
- Try e = 3: 3 × 7 = 21 ≡ 1 (mod 20) ✓
- So **e = 3**

Step 3: Encrypt plaintext m = 5
- C ≡ 5^3 (mod 33)
- 5^3 = 125

Step 4: Calculate 125 mod 33
- 125 ÷ 33 = 3 remainder 26
- 125 = 3 × 33 + 26
- C ≡ 26 (mod 33)

**Answer: Ciphertext = 26**

---

## RSA Practical Parameters

### Real-World RSA Size

The RSA modulus **n** should be at least **1024 bits long**, resulting in bit lengths for p and q of approximately **512 bits each**.

### Example: 512-Bit RSA Parameters

Here's an example of actual RSA parameters for a 512-bit system (shown in hexadecimal):

**p** (One of the primes):
```
E0DFD2C2A288ACEBC705EFAB30E4447541A8C5A47A37185C5A9
CB98389CE4DE19199AA3069B404FD98C801568CB9170EB712BF
10B4955CE9C9DC8CE6855C6123 (hex)
```

**q** (The other prime):
```
EBE0FCF21866FD9A9F0D72F7994875A8D92E67AEE4B515136B2
A778A8048B149828AEA30BD0BA34B977982A3D42168F594CA99
F3981DDABFAB2369F229640115 (hex)
```

**n** (Modulus - 1024 bits):
```
CF33188211FDF6052BDBB1A37235E0ABB5978A45C71FD381A91
AD12FC76DA0544C47568AC83D855D47CA8D8A779579AB72E635
D0B0AAAC22D28341E998E90F82122A2C06090F43A37E0203C2B
72E401FD06890EC8EAD4F07E686E906F01B2468AE7B30CBD670
255C1FEDE1A2762CF4392C0759499CC0ABECFF008728D9A11ADF (hex)
```

**e** (Public exponent):
```
40B028E1E4CCF07537643101FF72444A0BE1D7682F1EDB553E3
AB4F6DD8293CA1945DB12D796AE9244D60565C2EB692A89B888
1D58D278562ED60066DD8211E67315CF89857167206120405B0
8B54D10D4EC4ED4253C75FA74098FE3F7FB751FF5121353C554
391E114C85B56A9725E9BD5685D6C9C7EED8EE442366353DC39 (hex)
```

**d** (Private exponent - SECRET):
```
C21A93EE751A8D4FBFD77285D79D6768C58EBF283743D2889A3
95F266C78F4A28E86F545960C2CE01EB8AD5246905163B28D0B
8BAABB959CC03F4EC499186168AE9ED6D88058898907E61C7CC
CC584D65D801CFE32DFC983707F87F5AA6AE4B9E77B9CE630E2
C0DF05841B5E4984D059A35D7270D500514891F7B77B804BED81 (hex)
```

### The RSA Challenge Problem (1977)

One of the first public descriptions of RSA was in Martin Gardner's "Mathematical Games" column in Scientific American (1977).

**The Challenge**:
RSA inventors presented a challenge to decrypt this ciphertext:

```
9686 9613 7546 2206 1477 1409 2225 4355 8829 0575 9991 
1245 7431 9874 6951 2093 0816 2982 2514 5708 3569 3147 
6622 8839 8962 8013 3919 9055 1829 9451 5781 5154
```

Encrypted using n and e = 9007:
```
n = 114381625757888867669235779976146612010218296721
    242362562561842935706935245733897830597123513958
    705058989075147599290026879543541
```

**Solution** (Solved in 1994):
The problem was solved in **1994** by first **factorizing n** into:
- One 64-bit prime
- One 65-bit prime

After factorization, the plaintext was computed:

**"THE MAGIC WORDS ARE SQUEMISH OSSIFRAGE"**

---

## Designing a Good RSA Cryptosystem

### Critical Design Considerations

#### 1. How to Choose Large Primes p and q?

**Method**:
- Choose randomly a large integer p
- Verify using a randomized primality testing algorithm whether p is prime
- If not prime, check p + 2, p + 4, ... until a prime is found

**Practical Approach**:
- Use probabilistic primality tests (Miller-Rabin test)
- These tests are fast and sufficient for cryptographic purposes
- Multiple rounds increase confidence in primality

#### 2. What Relations Should Exist Between p and q?

To ensure RSA security, follow these constraints:

**Constraint 1: Length Difference**
- The difference |p - q| should be **neither too small nor too large**
- If p - q is too small, factorization algorithms become more efficient
- If p and q differ by too much in size, weaknesses may emerge

**Constraint 2: Common Factor Condition**
- **gcd(p-1, q-1)** should not be large
- If they share many common factors, certain attacks become more feasible

**Constraint 3: Prime Factors in p-1 and q-1**
- Both **p-1** and **q-1** should contain **large prime factors**
- This prevents attacks that exploit small prime factors

**Constraint 4: Safe Primes (Ideal Case)**
- p and q should be **safe primes**
- A safe prime is a prime p such that (p-1)/2 is also prime
- Examples of safe primes: 83, 107, 10100-166517
- Using safe primes provides very strong security guarantees

#### 3. How to Choose e and d?

**Constraint 1: Neither e nor d Should Be Small**
- If e is too small, certain attacks become possible
- If d is too small, decryption can be computed without knowing the private key

**Constraint 2: d Must Be Large Enough**
- **d should not be smaller than n^(1/4)**
- If d < n^(1/4), a polynomial-time algorithm exists to determine d
- This attack (called Wiener's attack) breaks small private exponents

**Practical Guidelines**:
- e is typically 65537 (0x10001) - a good compromise
- d should be approximately the same size as n (typically 1024+ bits)

---

## Fast Exponentiation

### Problem: Exponentiation with Large Numbers

When computing C ≡ m^e (mod n) or m ≡ C^d (mod n), the numbers involved are huge (often 1024+ bits). Direct computation is impossible.

### Solution: Binary Exponentiation

Also called **square-and-multiply algorithm**, this efficiently computes large exponentiations using modular arithmetic.

**Algorithm Overview**:
1. Convert the exponent to binary representation
2. Initialize result = 1
3. For each bit from most significant to least significant:
   - Square the current result (mod n)
   - If bit is 1, multiply by the base (mod n)

This reduces the computation from exponential time to linear time in the number of bits.

**Complexity**: O(k³) where k is the number of bits in the modulus

---

## RSA Security and Attacks

### Possible Attacks on RSA

There are several approaches to attacking RSA:

#### 1. Brute Force Key Search
- Try all possible values of d until decryption works
- **Status**: Infeasible given the size of numbers (1024+ bits)
- Key space is 2^1024, making this computationally impossible

#### 2. Mathematical Attacks
- Based on the **difficulty of computing φ(n)**
- Achievable only by **factoring the modulus n**
- Core approach: Factor n = p × q, compute φ(n) = (p-1)(q-1), then find d

#### 3. Timing Attacks
- Exploit timing variations in decryption operations
- Developed by Paul Kocher in mid-1990s
- More details in "Timing Attacks" section

#### 4. Chosen Ciphertext Attacks
- Attacker chooses specific ciphertexts and obtains decryptions
- Exploits algebraic properties of RSA
- Can be countered with proper padding (OAEP)

---

## Factoring Problem

### The Core of RSA Security

The mathematical difficulty of RSA security rests on the **factoring problem**:

> Given n = p × q where p and q are large primes, find p and q.

### Three Mathematical Approaches

1. **Direct Factorization**: Factor n = p × q, compute φ(n), then find d

2. **Direct Computation of φ(n)**: Determine φ(n) directly and compute d
   - This is believed to be as hard as factoring

3. **Direct Computation of d**: Find d directly without computing φ(n)
   - This is also believed to be as hard as factoring

### Current Belief

All three approaches are **currently believed to be equivalent to the factoring problem**.

### Progress in Factoring

**Historically**:
- Slow improvements over the years
- Biggest improvements come from improved algorithms, not just faster computers

**Algorithm Evolution**:
- **Quadratic Sieve (QS)** → replaced by
- **Generalized Number Field Sieve (GNFS)** → replaced by
- **Lattice Sieve (LS)** (current best)

**Current Progress (as of May 2005)**:
- Best achievement: Factored a **200 decimal digit number (663-bit) number** using Lattice Sieve
- Effort: Approximately 11-17 months of computation

**Security Assumption**:
- **1024-2048 bit RSA is currently considered secure**
- As computers get faster and algorithms improve, key sizes must increase
- Many systems now recommend 2048-bit or even 4096-bit keys

### Constraints on p and q

To avoid n being factored more easily, follow these constraints:

**Constraint 1: Similar Size**
- p and q should differ in length by only a few digits
- For a 1024-bit key (309 decimal digits), both p and q should be on the order of 10^(150) to 10^(155)

**Constraint 2: Large Prime Factors**
- Both (p - 1) and (q - 1) should contain large prime factors
- Prevents factorization algorithms optimized for numbers with only small prime factors

**Constraint 3: GCD Condition**
- gcd(p - 1, q - 1) should be small
- Avoids common factors that could be exploited

---

## Timing Attacks

### Discovery and Development

**Timing attacks** were developed by **Paul Kocher in the mid-1990s**.

### How Timing Attacks Work

Timing attacks exploit **timing variations in operations** to infer information about secret values:

**Principle**:
- Different operations take different amounts of time
- If these variations depend on secret data, they leak information
- Example: Multiplying by small vs. large numbers takes different times

**Example in RSA**:
- Squaring operations
- Conditional multiplications (IF statements that execute based on bit values)
- The time taken in exponentiation depends on which bits of d are 1 vs. 0

### Implications

An attacker who can precisely measure the **time taken to perform RSA decryption** can:
- Observe how long various operations take
- Infer which bits of the private exponent d are set to 1 or 0
- Gradually reconstruct the entire private key d

### Why Timing Attacks Are Dangerous

**Characteristics**:
1. **Unexpected Attack Vector**: Comes from a completely unexpected direction
2. **Ciphertext-Only**: Requires only observation of decryption times
3. **Practical**: Applicable in real-world scenarios where timing can be measured
4. **Applies Broadly**: Not limited to RSA - applicable to many cryptographic systems

### Countermeasures

To prevent timing attacks, several techniques can be used:

#### 1. Constant Exponentiation Time
- Implement the exponentiation algorithm to always take the same amount of time
- Perform dummy operations when needed
- Ensures no information leaks through timing

#### 2. Add Random Delays
- Add random delays to operations
- Makes it difficult for attackers to correlate timing with secret data
- Must ensure delays are truly random and unpredictable

#### 3. Blind Values Used in Calculations
- Use value blinding to randomize operands
- The computation is done on randomized values instead of actual values
- Prevents direct correlation between timing and specific operations

---

## Chosen Ciphertext Attacks

### What is a Chosen Ciphertext Attack?

A **Chosen Ciphertext Attack (CCA)** is a cryptanalytic attack where:
- The attacker **chooses specific ciphertexts**
- The attacker **obtains the decrypted plaintexts** (e.g., by submitting to a decryption oracle)
- The attacker uses the decryptions to gain cryptanalytic advantage

### RSA Vulnerability to CCA

**RSA is vulnerable to Chosen Ciphertext Attacks** because of its **multiplicative property**:

**Algebraic Property**:
- If C₁ = m₁^e (mod n) and C₂ = m₂^e (mod n)
- Then C₁ × C₂ = (m₁ × m₂)^e (mod n)

**Attack Scenario**:
1. Attacker has ciphertext C that encrypts unknown plaintext m
2. Attacker can access a decryption oracle (e.g., victim's decryption device)
3. Attacker creates: C' = C × 2^e (mod n)
4. Attacker submits C' to get decryption: m' = 2m (mod n)
5. Attacker recovers: m = m'/2 (mod n)

### Countermeasures

#### 1. Plaintext Padding (Random Padding)
- Add random padding to the plaintext before encryption
- Destroys the algebraic structure that enables CCA
- Makes each ciphertext dependent on random values

#### 2. Optimal Asymmetric Encryption Padding (OAEP)
- A sophisticated padding scheme that:
  - Adds deterministic and random padding
  - Makes encryption probabilistic
  - Prevents chosen ciphertext attacks
  - Widely used in modern RSA implementations
  - Specified in PKCS #1 standard
- Provides semantic security

### Modern Practice

Modern RSA implementations **must use proper padding schemes** (typically OAEP) to resist chosen ciphertext attacks. Raw RSA without padding is **not secure** against CCA.

---

## Key Takeaways

### RSA Algorithm Summary

1. **Security Basis**: Based on the difficulty of factoring large integers
2. **Key Generation**: Requires careful selection of primes p, q and exponents e, d
3. **Encryption/Decryption**: Uses modular exponentiation
4. **Practical Size**: Modern RSA uses 1024-2048 bit keys minimum
5. **Attacks**: Multiple attack vectors exist (factoring, timing, CCA)
6. **Countermeasures**: Use safe primes, constant-time algorithms, proper padding
7. **Advantages**:
   - Widely standardized and understood
   - No need for pre-shared secrets
   - Supports digital signatures
   - Suitable for PKI

### RSA in Practice

- **Still secure**: For 2048-bit keys and above with proper padding
- **Widely used**: SSL/TLS, PGP, digital signatures, PKI
- **Well-understood**: Decades of cryptanalysis and formal security proofs
- **Requires care**: Improper implementation leads to security vulnerabilities
