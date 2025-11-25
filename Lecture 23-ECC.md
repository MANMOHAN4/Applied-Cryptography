# Elliptic Curve Cryptography (ECC)
## Lecture 23 - Applied Cryptography (UE23CS342AA4)

---

## Table of Contents
1. [Introduction to ECC](#introduction-to-ecc)
2. [What are Elliptic Curves](#what-are-elliptic-curves)
3. [Elliptic Curves Over Real Numbers](#elliptic-curves-over-real-numbers)
4. [Elliptic Curves Over Finite Fields](#elliptic-curves-over-finite-fields)
5. [Elliptic Curve Discrete Logarithm Problem (ECDLP)](#elliptic-curve-discrete-logarithm-problem-ecdlp)
6. [Point Addition on Elliptic Curves](#point-addition-on-elliptic-curves)
7. [Double-and-Add Algorithm](#double-and-add-algorithm)
8. [The Trapdoor Function in ECC](#the-trapdoor-function-in-ecc)
9. [Why ECC is Hard to Break](#why-ecc-is-hard-to-break)
10. [ECDH - Elliptic Curve Diffie-Hellman](#ecdh---elliptic-curve-diffie-hellman)
11. [ECDSA - Elliptic Curve Digital Signature Algorithm](#ecdsa---elliptic-curve-digital-signature-algorithm)
12. [ECC Security Advantages](#ecc-security-advantages)
13. [Applications of ECC](#applications-of-ecc)
14. [Benefits of ECC](#benefits-of-ecc)
15. [Comparison with RSA](#comparison-with-rsa)

---

## Introduction to ECC

### What is ECC?

**Elliptic Curve Cryptography (ECC)** is a modern family of **public-key cryptosystems** based on:
- The algebraic structures of **elliptic curves over finite fields**
- The difficulty of the **Elliptic Curve Discrete Logarithm Problem (ECDLP)**

### Why ECC Matters

**ECC is the Modern Successor to RSA** because:

1. **Smaller Keys**: Uses significantly smaller key sizes for the same security level
   - Example: 256-bit ECC ≈ 3072-bit RSA in security strength

2. **Fast Operations**: Provides very fast:
   - Key generation
   - Key agreement
   - Signature generation and verification

3. **Efficiency**: Better suited for modern devices with limited:
   - Processing power
   - Memory
   - Battery life
   - Storage space

### History

- **Proposed in mid-1980s**: By Neal Koblitz and Victor Miller independently
- **Adoption timeline**: Slow initially, but now widely deployed
- **Modern standards**: NIST, SEC, and other standards bodies endorse ECC curves

---

## What are Elliptic Curves

### Definition

**An elliptic curve** is a set of points (x, y) that satisfy a specific **cubic equation** with coefficients.

**Standard Form** (over real numbers):

### y² = x³ + ax + b

Where:
- **x, y**: Variables (coordinate of point on curve)
- **a, b**: Coefficients (define the specific curve)
- The curve is **not an ellipse** (misleading name, historical reasons)

### Key Properties of Elliptic Curves

#### Property 1: Symmetry About X-Axis

- The elliptic curve equation is symmetric about the x-axis
- If point (x, y) is on the curve, then (x, -y) is also on the curve
- This symmetry is important for the point addition operation

#### Property 2: Line Intersection

- **Any non-vertical line** will intersect the curve in **at most three places**
- This property is crucial for defining point addition

#### Property 3: Point Addition

If **P₁** and **P₂** are on the elliptic curve E:

**P₃ = P₁ + P₂** (point addition defined geometrically)

### Computational Requirements

For elliptic curves to be useful in cryptography:

1. **Equation in Two Variables**: y² = x³ + ax + b
2. **Restricted to Finite Fields**: Variables and coefficients are elements of finite fields
3. **Discrete Nature**: Working with integers modulo p instead of real numbers
4. **Computational Structure**: Addition operation defined on the curve points

---

## Elliptic Curves Over Real Numbers

### Definition

**Elliptic curves over real numbers** are a set of points **(x, y)** which satisfy:

**y² = x³ + ax + b**

Where:
- **x, y, a, b**: All are **real numbers**

### Geometric Interpretation

**Curves Over Reals**:
- Visual representation possible
- Points form a smooth curve in the real plane
- Line intersections are clear geometrically
- Multiple branches may be present depending on equation

### Example Curves

**Example 1**: y² = x³ - x

- a = -1, b = 0
- Creates a curve with specific shape

**Example 2**: y² = x³ - x + 1

- a = -1, b = 1
- Different curve shape

### Limitations

**Why not used in cryptography**:
- Real numbers have precision issues (floating-point)
- Not suitable for computer implementations (rounding errors)
- Cannot guarantee exact mathematical properties

---

## Elliptic Curves Over Finite Fields

### Definition

**Elliptic curves over finite fields** are defined over **Zₚ** (integers modulo a prime p):

**y² ≡ x³ + ax + b (mod p)**

Where:
- **x, y, a, b**: All are **integers modulo p**
- **p**: A large prime number
- Operations are all modular arithmetic (mod p)

### Why Finite Fields

**Advantages**:
- **Discrete mathematics**: No precision issues
- **Exact arithmetic**: All operations deterministic
- **Suitable for computers**: Integer arithmetic only
- **Well-defined group structure**: Mathematical properties guaranteed

### Practical Implementation

**Cryptographic Curves** (standardized):

1. **secp256k1**: Used in Bitcoin and Ethereum
   - p = 2²⁵⁶ - 2³² - 977
   - 256-bit security level

2. **secp384r1**: High security applications
   - 384-bit security level

3. **Curve25519**: Modern alternative
   - Designed for faster performance
   - 128-bit security level (256-bit key)

4. **Brainpool curves**: European standard
   - Various bit lengths (160 to 512 bits)

---

## Elliptic Curve Discrete Logarithm Problem (ECDLP)

### Definition

**ECDLP** is the problem of finding the scalar k given:

- **Generator point G** on the curve
- **Point Q = kG** (result of scalar multiplication)

**Task**: Find the scalar **k** such that **Q = kG**

### The Security Assumption

**Hardness of ECDLP**:

Given k and P, it's **easy to compute**: **Q = kG**

But given Q and P, it's **hard to find**: **k**

This is the **trapdoor one-way function** in ECC.

### Security Parameters

**For security**:

1. **k must be large**: Typically same size as p (256 bits or more)
2. **k should be random**: Weak randomness breaks security
3. **p should be large prime**: At least 256 bits recommended
4. **Order of G should be large**: Close to p for strongest security

### Current Estimates

- **No polynomial-time algorithm known**: Best known is exponential time
- **Discrete logarithm in multiplicative group**: O(√p) complexity
- **Elliptic curve discrete log**: Same O(√p) complexity
- **But**: ECC curves designed so √p much larger for same key size

---

## Point Addition on Elliptic Curves

### The Addition Operation

**On an elliptic curve**, we can define an **addition operation** (+) that works geometrically.

### Geometric Method

#### Rule 1: Adding Two Different Points P and Q

1. Draw a **line through points P and Q**
2. This line intersects the curve at **three points: P, Q, and R**
3. Reflect R across the **x-axis** to get **-R**
4. Define: **P + Q = -R**

#### Rule 2: Adding a Point to Itself (P + P)

1. Draw the **tangent line** to the curve at point P
2. The tangent intersects the curve at **two points: P and R**
3. Reflect R across the **x-axis** to get **-R**
4. Define: **P + P = 2P = -R**

**This is called "doubling"** and is used in efficient multiplication.

#### Rule 3: Adding a Point to Its Reflection

If **Q = -P** (reflection across x-axis):
- The line through P and -P is **vertical**
- It does NOT intersect the curve at a third point
- Define: **P + (-P) = O** (the point at infinity - identity element)

### Algebraic Formulation

For **P = (x₁, y₁)** and **Q = (x₂, y₂)** on curve **y² = x³ + ax + b**:

**Slope Calculation**:
- If x₁ ≠ x₂: **m = (y₂ - y₁) / (x₂ - x₁) mod p**
- If x₁ = x₂: **m = (3x₁² + a) / (2y₁) mod p** (for doubling)

**Result Point R = (x₃, y₃)**:
- **x₃ = m² - x₁ - x₂ mod p**
- **y₃ = m(x₁ - x₃) - y₁ mod p**

### Important Property: Group Structure

The points on an elliptic curve form an **abelian group** under addition:

1. **Closure**: P + Q is also on the curve
2. **Associativity**: (P + Q) + R = P + (Q + R)
3. **Identity**: Point at infinity O is the identity
4. **Inverse**: For each P, point -P is its inverse

---

## Double-and-Add Algorithm

### Purpose

Efficiently compute **kP** where:
- **k**: Large scalar (could be 256 bits)
- **P**: Point on the elliptic curve

### The Challenge

**Naive approach**: Add P to itself k times
- For k = 2²⁵⁶, this requires 2²⁵⁶ additions
- **Computationally infeasible**

### Solution: Binary Method

**Algorithm**:

1. **Convert k to binary**: k = (kₙ kₙ₋₁ ... k₁ k₀)₂

2. **Initialize**: Result = O (point at infinity)

3. **For each bit from most significant to least significant**:
   - **Always double** the current point
   - **If bit is 1**: Also add P to the result

4. **Return**: Result = kP

### Complexity

- **Time complexity**: O(log k) point additions
- **Space complexity**: O(1) additional storage
- For 256-bit k: Only ~256 doublings and ~128 additions (on average)

### Example: Computing 5P

**k = 5 = (101)₂**

1. Start: result = O, current = P
2. Bit 1: 
   - Double current → current = 2P
   - Bit is 1, so add: result = O + P = P
3. Bit 0:
   - Double current → current = 4P
   - Bit is 0, so don't add
4. Bit 1:
   - Double current → current = 8P
   - Bit is 1, so add: result = P + 4P = 5P

**Final result: 5P** ✓

---

## The Trapdoor Function in ECC

### Visual Analogy

Imagine starting at point **A** on an elliptic curve:

1. Apply point addition operation (the "dot" function) repeatedly
2. "Hop around" the curve through different points
3. Eventually reach final point **E** after multiple hops

### The Trapdoor Property

#### Forward Direction (Easy):

**If you know**:
- Starting point: **A**
- Number of hops: **k**

**Then you can easily compute**:
- Ending point: **E = kA**

This takes only **O(log k)** operations.

#### Reverse Direction (Hard):

**If you know**:
- Starting point: **A**
- Ending point: **E = kA**

**Then finding the number of hops (k) is hard**:
- This is the **Elliptic Curve Discrete Logarithm Problem**
- Best known attacks take **O(√p)** time
- For 256-bit curve: ~2¹²⁸ operations

### Mathematical Form

**Forward** (private key known):
- Public key Q = d·P
- Given: d (private), P (generator)
- Compute: Q in O(log d) time ✓

**Reverse** (private key unknown):
- Public key Q is known
- Find: d such that Q = d·P
- This is ECDLP - computationally hard ✓

### Public Key and Private Key

**Example from slides**:

Starting at point **A**:
- A dot B = -C (add B, get -C, reflect to C)
- A dot C = -D (add C, get -D, reflect to D)  
- A dot D = -E (add D, get -E, reflect to E)

**Public Key**: Starting point A, Ending point E
**Private Key**: Number of "dot" operations (3 in this example)

---

## Why ECC is Hard to Break

### The ECDLP Property

**ECDLP has fundamental hardness properties**:

**Easy**:
- Given k, P → Compute Q = kP (scalarmultiplication)
- Time: O(log k) point operations

**Hard**:
- Given Q, P → Find k such that Q = kP (discrete log)
- Time: O(√p) with best known algorithms
- For p ≈ 2²⁵⁶: ~2¹²⁸ operations (infeasible)

### No Sub-exponential Algorithm

**Current mathematical status**:
- **No sub-exponential algorithm known** for solving ECDLP
- All known algorithms are **exponential in log p**
- This contrasts with **factorization** (has sub-exponential algorithms)

### Attacks on ECDLP

**Known attacks**:

1. **Pollard's rho algorithm**: O(√p) time, O(log p) space
   - Best general-purpose attack
   - Cannot be significantly improved

2. **Pohlig-Hellman algorithm**: Only if order has small factors
   - Why curves use large prime-order subgroups

3. **Index calculus**: Doesn't apply to elliptic curves
   - This is why ECC is stronger than DLP in multiplicative groups

### Security Strength Estimates

| ECC Key Size | Equivalent RSA Key | Security Level |
|--------------|-------------------|-----------------|
| 160 bits | 1024 bits | Weak (deprecated) |
| 192 bits | 1536 bits | Weak |
| 224 bits | 2048 bits | Moderate |
| **256 bits** | **3072 bits** | **Strong** |
| 384 bits | 7680 bits | Very strong |
| 521 bits | 15360 bits | Extremely strong |

---

## ECDH - Elliptic Curve Diffie-Hellman

### What is ECDH?

**ECDH** is the elliptic curve version of the classic **Diffie-Hellman key exchange**.

### Comparison: Classic DH vs ECDH

**Classic Diffie-Hellman**:
- Works in **multiplicative group of integers modulo p**
- Compute: g^x mod p

**ECDH**:
- Works in **additive group of points on elliptic curve**
- Compute: x·G (scalar multiplication of point)

### Protocol

#### Step 1: Public Parameters

Alice and Bob publicly agree on:
- **Elliptic curve equation**: y² ≡ x³ + ax + b (mod p)
- **Generator point**: G on the curve
- **Prime p**: Size of finite field

#### Step 2: Private Keys

**Alice** secretly chooses: **a** (random, large integer)
**Bob** secretly chooses: **b** (random, large integer)

#### Step 3: Public Keys

**Alice computes**: **A = a·G** (scalar multiplication)
**Bob computes**: **B = b·G** (scalar multiplication)

Alice sends A to Bob, Bob sends B to Alice (over public channel)

#### Step 4: Shared Secret

**Alice computes**: **K = a·B = a·(b·G) = (ab)·G**
**Bob computes**: **K = b·A = b·(a·G) = (ab)·G**

Both have the same shared secret **K**!

### Security Basis

**ECDLP ensures security**:
- Attacker knows G, A = a·G, B = b·G
- Attacker must find K = (ab)·G
- Requires solving ECDLP (finding a or b)
- Best known attack: O(√p) time (exponential)

### Advantages Over Classic DH

1. **Smaller key sizes**: 256-bit ECDH ≈ 3072-bit DH
2. **Same security**: With much shorter keys
3. **Faster computation**: Smaller numbers involved
4. **Better scalability**: Lower bandwidth for key exchange

### Real-Life Applications

1. **TLS Handshakes**: ECDH/ECDHE in HTTPS connections
2. **VPN Protocols**: OpenVPN, IKEv2/IPsec, WireGuard
3. **Perfect Forward Secrecy**: Ephemeral ECDH (ECDHE)
4. **Mobile Apps**: WhatsApp, Signal use Curve25519

### Weaknesses

1. **Invalid Curve Attacks**: If implementations don't verify points are on curve
2. **No Authentication**: ECDH alone doesn't authenticate parties
3. **Vulnerable to MITM**: Must combine with digital signatures or certificates

---

## ECDSA - Elliptic Curve Digital Signature Algorithm

### What is ECDSA?

**ECDSA** = **Elliptic Curve** + **Digital Signature Algorithm**

A **digital signature scheme** that provides:
- **Authenticity**: Proves signer's identity
- **Integrity**: Proves message wasn't modified
- **Non-repudiation**: Signer cannot deny signing

### How ECDSA Works

**ECDSA combines**:
- **Elliptic curve math** (for security)
- **Hash functions** (SHA-256, SHA-3, etc.)
- **Digital signature operations** (signing and verification)

### Popular ECC Curves Used

- **secp256k1**: Bitcoin, Ethereum (256-bit security)
- **Curve25519**: Signal, many modern protocols
- **NIST P-256, P-384, P-521**: Government, TLS, official systems

### Signing Process

**Sender (Signer) with private key d**:

1. **Hash the message**: h = H(message)
2. **Generate random nonce**: k (must be unique per signature!)
3. **Compute curve point**: R = k·G
4. **Extract R's x-coordinate**: r = x_R mod n
5. **Compute signature value**: s = k⁻¹(h + d·r) mod n
6. **Signature**: (r, s)

**The signature depends on**:
- Message hash h
- Signer's private key d
- Random nonce k (must be cryptographically strong)

### Verification Process

**Receiver (Verifier) with signer's public key Q**:

1. **Hash the message**: h = H(message)
2. **Extract r, s from signature**
3. **Compute**: u₁ = h·s⁻¹ mod n
4. **Compute**: u₂ = r·s⁻¹ mod n
5. **Compute curve point**: R' = u₁·G + u₂·Q
6. **Extract R' x-coordinate**: r' = x_R' mod n
7. **Verify**: r = r'? (If yes, signature is valid)

### Real-Life Applications

1. **Bitcoin**: Signs all transactions
2. **Ethereum**: Signs smart contract interactions
3. **TLS Certificates**: ECDSA alternative to RSA signatures
4. **Digital IDs**: Smart cards, government credentials
5. **Code Signing**: Software publishers sign executables

### ECDSA Strengths

- **Smaller signatures**: 512 bits for 256-bit security (compared to 3072-bit RSA)
- **Fast verification**: Faster than RSA signatures
- **Widely standardized**: NIST, SEC, ISO standards
- **Future-proof**: Better scalability than RSA

### ECDSA Weaknesses

1. **Nonce Reuse Vulnerability**: 
   - If same k used for two signatures with same key
   - Private key d can be recovered instantly
   - Example: Early Bitcoin implementations, PlayStation 3

2. **Implementation Complexity**:
   - Mathematical operations are intricate
   - Small coding errors cause catastrophic failures
   - Difficult to implement securely

3. **Side-Channel Attacks**:
   - **Timing attacks**: Infer private key from operation timing
   - **Power analysis**: Monitor power consumption during signing
   - **Electromagnetic radiation**: Leak from EM emissions
   - Sophisticated implementations needed for protection

---

## ECC Security Advantages

### Key Size Comparison

**To protect a 128-bit AES key**:

| Algorithm | Key Size Needed | Efficiency |
|-----------|-----------------|------------|
| **RSA** | **3072 bits** | Baseline |
| **ECC** | **256 bits** | **12x smaller** |
| **AES** | 128 bits | Symmetric (direct) |

**Practical implication**:
- ECC uses 1/12th the key size of RSA for same security
- Massive savings in:
  - Storage space
  - Communication bandwidth
  - Computational resources

### Why Strengthen RSA?

**RSA scaling problem**:
- As computers get faster, attacks improve
- Solution: Increase RSA key sizes
- 1024 bits → 2048 bits → 4096 bits needed

**Impractical for some applications**:
- Mobile devices with limited storage
- IoT devices with minimal CPU
- High-volume transactions (bandwidth constraints)

**ECC Scaling**:
- 256-bit → 384-bit → 521-bit needed
- Still much smaller than RSA
- Better fit for resource-constrained devices

---

## Applications of ECC

### 1. Wireless Communication Devices

- **Smartphones, tablets, laptops**: Limited battery, storage
- **Wearables**: Extremely limited computational power
- **IoT sensors**: Minimal processing capability
- ECC's smaller keys reduce power consumption

### 2. Smart Cards

- **SIM cards**: Limited memory and CPU
- **Payment cards**: EMV transactions
- **ID cards**: Government credentials
- ECC enables secure cryptography on tiny devices

### 3. Web Servers

- **Handle many encrypted connections**
- **Need fast key generation and agreement**
- **Reduced bandwidth for key exchange**
- ECC ECDHE makes HTTPS more efficient

### 4. Blockchain and Cryptocurrency

- **Bitcoin**: Uses secp256k1 for signatures
- **Ethereum**: Uses secp256k1 for transactions
- **Other cryptocurrencies**: Many use ECC
- High-volume transactions benefit from speed and size

### 5. Any Application Requiring

**Compact cryptography**:
- Limited storage available
- Bandwidth constraints
- Fast key generation needed
- Efficient signature generation

---

## Benefits of ECC

### Same Benefits As Other Public-Key Cryptosystems

ECC provides all standard security properties:

1. **Confidentiality**: Encrypt data so only recipient can read
2. **Integrity**: Detect if message was modified
3. **Authentication**: Prove message is from claimed sender
4. **Non-repudiation**: Sender cannot deny sending message

**But with significant advantages**:

### Shorter Key Lengths

- **256-bit ECC ≈ 3072-bit RSA**
- **384-bit ECC ≈ 7680-bit RSA**
- **Much more practical** for real-world deployment

### Faster Operations

- **Key generation**: Quick and efficient
- **Encryption/decryption**: Faster computation
- **Signature generation/verification**: Speeds up transactions
- **Overall performance**: Better throughput

### Storage Savings

- **Certificates**: Smaller files
- **Keys**: Reduced storage requirements
- **Signatures**: Compact format
- **Total data**: Significant bandwidth reduction

### Bandwidth Efficiency

- **Network communication**: Fewer bytes to transmit
- **IoT devices**: Lower power for transmission
- **High-frequency trading**: Reduced latency
- **Mobile networks**: Less data usage

---

## Comparison with RSA

| Feature | ECC | RSA |
|---------|-----|-----|
| **Security Basis** | ECDLP | Factorization |
| **Key Size (128-bit security)** | 256 bits | 3072 bits |
| **Key Generation Speed** | Very fast | Moderate |
| **Encryption Speed** | Moderate | Moderate |
| **Signature Speed** | Fast | Moderate |
| **Signature Size** | Smaller | Larger |
| **Security Maturity** | Well-studied since 1985 | Decades of analysis |
| **Standardization** | NIST, SEC, ISO | Universal |
| **Real-World Deployment** | Growing (HTTPS, Bitcoin, TLS) | Dominant (legacy systems) |
| **Quantum Threat** | Yes (like RSA) | Yes (like ECC) |
| **Post-Quantum** | Both superseded by PQC | Both superseded by PQC |

---

## Summary

### Key Concepts

1. **Mathematical Basis**: Elliptic curve arithmetic over finite fields
2. **Hard Problem**: ECDLP (Elliptic Curve Discrete Logarithm Problem)
3. **Trapdoor Function**: Easy to multiply, hard to find scalar
4. **Efficiency**: Smaller keys than RSA for same security
5. **Modern Standard**: ECDH, ECDSA becoming de facto standards

### When to Use ECC

**Use ECC for**:
- Modern cryptographic systems (new deployments)
- Mobile and IoT applications
- Blockchain and cryptocurrencies
- High-performance systems
- Resource-constrained devices
- When storage/bandwidth is limited

**Consider RSA for**:
- Legacy system compatibility
- Universal support requirements
- Specific regulatory requirements
- Well-established infrastructure

### The Future

**ECC Advantages**:
- Better scaling to larger key sizes
- More efficient than RSA for equivalent security
- Natural fit for elliptic curve point arithmetic
- Growing adoption in standards and protocols

**Current Trend**:
- HTTPS moving from RSA to ECC
- TLS 1.3 uses ECDH primarily
- Cryptocurrency built on ECC
- Government standards include ECC
- Gradual migration from RSA to ECC

**The path forward**:
- Both ECC and RSA vulnerable to quantum computers
- Post-quantum cryptography under development
- NIST evaluating quantum-resistant algorithms
- Hybrid approaches combining both for transition period

Elliptic Curve Cryptography represents the practical successor to RSA for most modern applications, offering better efficiency without sacrificing security.
