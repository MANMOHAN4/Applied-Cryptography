# Post-Quantum Cryptography
## Lecture 37 - Applied Cryptography (UX231CS642B)

---

## Table of Contents
1. [Introduction: The Y2Q Threat](#introduction-the-y2q-threat)
2. [Blockchain Technology Fundamentals](#blockchain-technology-fundamentals)
3. [What is Blockchain](#what-is-blockchain)
4. [Cryptography in Blockchain](#cryptography-in-blockchain)
5. [Components of Blockchain](#components-of-blockchain)
6. [What is a Block](#what-is-a-block)
7. [Hashing in Blockchain](#hashing-in-blockchain)
8. [Advantages of Hash Functions in Blockchain](#advantages-of-hash-functions-in-blockchain)
9. [Digital Signatures in Blockchain](#digital-signatures-in-blockchain)
10. [Importance of Digital Signatures](#importance-of-digital-signatures)
11. [Future of Cryptography](#future-of-cryptography)
12. [The Y2Q Event](#the-y2q-event)
13. [Quantum Computing Threat](#quantum-computing-threat)
14. [Shor's Algorithm](#shors-algorithm)
15. [Why We Can't Wait](#why-we-cant-wait)
16. [Quantum Computers vs Classical Crypto](#quantum-computers-vs-classical-crypto)
17. [Two Approaches to Quantum Threats](#two-approaches-to-quantum-threats)
18. [Quantum Cryptography](#quantum-cryptography)
19. [Quantum Cryptography Limitations](#quantum-cryptography-limitations)
20. [Post-Quantum Cryptography](#post-quantum-cryptography)
21. [What is PQC](#what-is-pqc)
22. [PQC Characteristics](#pqc-characteristics)
23. [PQC Families](#pqc-families)
24. [Lattice-Based Algorithms](#lattice-based-algorithms)
25. [Code-Based Algorithms](#code-based-algorithms)
26. [Hash-Based Algorithms](#hash-based-algorithms)
27. [Isogeny-Based Algorithms](#isogeny-based-algorithms)
28. [Multivariate Polynomial Algorithms](#multivariate-polynomial-algorithms)
29. [Challenges of Post-Quantum Cryptography](#challenges-of-post-quantum-cryptography)
30. [Current Status of PQC](#current-status-of-pqc)
31. [NIST Standardization](#nist-standardization)
32. [CRYSTALS-Kyber](#crystals-kyber)
33. [CRYSTALS-Dilithium](#crystals-dilithium)
34. [FALCON](#falcon)
35. [SPHINCS+](#sphincs)
36. [Implementation Timeline](#implementation-timeline)

---

## Introduction: The Y2Q Threat

### The Nightmare Scenario

**Imagine**:
- You wake up one day
- All encryption algorithms stop working
- Everything encrypted becomes insecure
- Your digital privacy vanishes

### The Event

**Named**: Y2Q event

**Reference**: Y2K (Year 2000) problem

**Difference**: Y2Q about quantum, not millennium

### Digital Insecurity Consequence

**If Y2Q happens**:
- All encrypted data becomes readable
- Past communications exposed
- Financial transactions vulnerable
- Government secrets compromised
- Personal privacy destroyed

---

## Blockchain Technology Fundamentals

### What is Blockchain

**Definition**: A distributed database of records of all transactions

**Key characteristics**:
- All transactions executed and shared
- Among participating parties
- Every transaction verified by majority
- Contains complete record of every transaction

### Structure

**Visual model**:
```
Block 1 → Block 2 → Block 3 → ... → Block n
```

**Each block contains**:
- Transaction data
- Hash of previous block
- Timestamp
- Merkle tree of transactions

### Distribution

**Decentralized**:
- No central authority
- Copies maintained by all participants
- Transactions verified by consensus
- Immutable record

### Immutability

**Key property**: Data cannot be altered or deleted

**Why**:
- Hash chains link blocks
- Changing one block invalidates all following
- Entire network must agree
- Practically impossible to alter

---

## Cryptography in Blockchain

### Three Main Components

**1. Public-Key Encryption**:
- Basis for blockchain wallets
- Basis for transactions
- Public key as address
- Private key for signing

**2. Cryptographic Hash Functions**:
- Provide immutability
- Link blocks together
- Enable integrity checking
- Create Merkle trees

**3. Digital Signatures**:
- Verify authenticity of transactions
- Prove ownership
- Enable non-repudiation
- Ensure transaction legitimacy

### Process Flow

```
Transaction Creation:
    ↓
Sign with private key (digital signature)
    ↓
Verify with public key
    ↓
Hash for block inclusion
    ↓
Broadcast to network
    ↓
Consensus and verification
    ↓
Add to blockchain (immutable)
```

---

## Components of Blockchain

### Transaction Component

**What is a transaction**:
- Record of asset transfer
- Includes sender, receiver, amount
- Digitally signed
- Timestamped

### Block Component

**What is in a block**:
- Multiple transactions
- Merkle root (hash of all transactions)
- Hash of previous block
- Block header (version, timestamp, difficulty)
- Nonce (for proof of work)

### Chain Component

**How blocks are linked**:
- Each block references previous
- Cryptographic hash link
- Cannot change without detection
- Forms immutable chain

---

## What is a Block

### Definition

**A block is a digital safe** to store data and **lock it forever**

**Key property**: Data added is **immutable** - cannot be altered or deleted

### Block Structure

```
Block Header:
├─ Version
├─ Previous block hash
├─ Merkle root
├─ Timestamp
├─ Difficulty target
└─ Nonce

Block Body:
└─ Transactions (multiple)
```

### Why Immutable

**Reasons**:
1. Hash chain - changing data changes hash
2. All blocks after change become invalid
3. Network nodes detect discrepancy
4. Majority consensus prevents alteration
5. Practically impossible to rewrite

### Security Through Immutability

**Benefit**:
- Historical record cannot be tampered
- Transactions permanently recorded
- Fraud detection easy
- Trust in system maintained

---

## Hashing in Blockchain

### Role of Hashing

**Primary use**: **Hashing enables immutability**

**Process**:
1. Transaction verified
2. Hash added to block
3. New unique hash generated from transaction
4. Block hash links to previous
5. Chain forms via hash links

### Hash Properties in Blockchain

**Critical properties**:
- **Deterministic**: Same input always produces same hash
- **One-way**: Cannot reverse hash to get input
- **Avalanche effect**: Small change produces completely different hash
- **Quick**: Fast to compute
- **Fixed output**: Consistent size

### Merkle Tree

**Structure**:
```
        Root Hash
       /          \
    Hash(AB)    Hash(CD)
    /    \      /    \
Hash(A) Hash(B) Hash(C) Hash(D)
  |       |      |       |
  A       B      C       D
 (Transactions)
```

**Benefit**: Can verify any transaction without downloading all

---

## Advantages of Hash Functions in Blockchain

### Advantage 1: Reduce Transaction Bandwidth

**How**:
- Transactions hashed to fixed size
- Transmit hash instead of full transaction
- Significant size reduction
- Faster transmission

**Example**: 1MB transaction → 32-byte hash

### Advantage 2: Prevent Modifications

**Protection**:
- Any modification changes hash
- Modification detected immediately
- Block becomes invalid
- Network rejects altered block

**Security**: Data integrity guaranteed

### Advantage 3: Easy Transaction Verification

**Process**:
- Compute hash of transaction
- Compare with stored hash
- Match = unmodified
- No match = altered data detected

**Efficiency**: Quick verification without full data

### Advantage 4: Enable Efficient Pruning

**Capability**:
- Nodes can store only hashes
- Verify without storing all data
- Reduces storage requirements
- Enables lightweight clients

---

## Digital Signatures in Blockchain

### Definition

**Digital signatures** function as **cryptographic verification systems**

**Purpose**: **Pivotal for fostering trust within the network**

### How Digital Signatures Work

**Process**:
1. Sender creates transaction
2. Sender signs with private key
3. Signature attached to transaction
4. Receiver verifies with public key
5. Verification proves authenticity

### Ownership Proof

**In blockchain context**:
- Digital signature proves transaction ownership
- Signer must have private key
- Cannot forge without private key
- Proves authorization of transaction

### Non-Repudiation

**Guarantee**:
- Signer cannot deny signing
- Signature proves participation
- Legal validity
- Accountability established

---

## Importance of Digital Signatures in Blockchain

### Importance 1: Enhanced Security

**Security benefits**:
- Only private key holder can sign
- Cannot impersonate someone else
- Tampering detected
- Transaction authenticity verified

**Mechanism**: Cryptographic binding to identity

### Importance 2: Decentralization Support

**How**:
- No central authority needed to verify
- Anyone can verify with public key
- Peer-to-peer verification
- Trust doesn't require middleman

**Benefit**: True decentralization possible

### Importance 3: Traceability

**What's tracked**:
- Who initiated transaction
- When transaction occurred
- What transaction contained
- Complete audit trail

**Forensics**: Can trace history for any transaction

### Additional Benefits

**Further advantages**:
- Prevents double-spending
- Enables smart contracts
- Supports multi-signature wallets
- Enables conditional transactions

---

## Future of Cryptography

### Current State

**Where we are**:
- Cryptography widely deployed
- RSA, ECC, SHA-256 standard
- Quantum computers still theoretical (for now)
- But threat is real and imminent

### Coming Challenges

**Y2Q Event**: When quantum computers break current encryption

**Timeline**:
- Maybe 10-20 years
- Maybe sooner
- Maybe different
- But need to prepare NOW

---

## The Y2Q Event

### Definition

**Y2Q** = **Quantum Year 2** (reference to Y2K)

**When all encryption algorithms stop working** due to quantum computers

### The Horror

**What would happen**:
- All public-key crypto broken
- All symmetric crypto weakened
- Banking systems vulnerable
- Government secrets exposed
- Personal communications readable
- Digital signatures forged

### Historic Parallel

**Y2K problem**:
- Programs used 2-digit year
- Thought would fail at year 2000
- Lots of preparation
- Minor issues when it occurred

**Y2Q would be worse**:
- Not just software issue
- Fundamental cryptography broken
- Affects all digital communication
- Would take years to recover

---

## Quantum Computing Threat

### Shor's Algorithm: Asymmetric Algorithms at Risk

**What it does**:
- Quantum algorithm for factoring
- Efficient solution to discrete log problem
- Can break RSA in polynomial time
- Can break ECC in polynomial time

### Specific Vulnerability

**RSA vulnerability**:
- RSA security depends on factorization difficulty
- Factoring thousands-digit number takes classical computer millions of years
- Quantum computer with Shor's algorithm: seconds or minutes

**Example**:
- RSA-2048 (takes classical computer billions of years)
- Quantum computer: potentially hours or less

### Discrete Logarithm Threat

**ECC vulnerability**:
- ECC security depends on discrete log difficulty
- Shor's algorithm solves efficiently
- ECC-256 similarly vulnerable
- All ECC variants affected

### Timeline

**When would this happen**:
- Depends on quantum computer development
- Not imminent but not far away
- Could be 10-15 years
- Could be sooner
- Could be later

---

## Why We Can't Wait

### Reason 1: Harvesting Attacks

**What it is**:
- Attackers store encrypted data today
- Wait for quantum computers
- Break encryption later
- Read data retroactively

**Called**: "Harvest now, decrypt later"

**Implication**: Data encrypted today is at risk

### Reason 2: Rewrite History

**Specific threat - Blockchains**:
- Forge signatures for old keys
- Rewrite blockchain history
- Steal cryptocurrency retroactively
- Destroy permanent records

**Consequence**: Blockchain immutability broken

### Reason 3: Deployment Takes Time

**Reality of large systems**:
- Changing global cryptography infrastructure: 10+ years
- Need standards
- Need implementations
- Need testing
- Need migration
- Need legacy support

**Bottom line**: Must start now for transition

### Reason 4: Preparation Advantage

**Early adoption**:
- Get familiar with new algorithms
- Build infrastructure gradually
- Test compatibility
- Reduce migration risk

**Procrastination**: Would cause crisis

---

## Quantum Computers vs Classical Crypto

### Impact on Asymmetric Cryptography

**Threatened**:
- RSA: Broken by Shor's algorithm
- ECC: Broken by Shor's algorithm
- Diffie-Hellman: Broken
- ElGamal: Broken
- Digital signatures: Compromised

**Consequence**: All public-key crypto vulnerable

**Mechanism**: Shor's algorithm in polynomial time

### Impact on Symmetric Cryptography

**Quantum threat**:
- Grover's algorithm provides quadratic speedup
- Security level halved
- AES-256 effectively becomes AES-128 strength
- AES-128 becomes weak

**Solution**: Double key size
- AES-256 remains secure
- Must deploy AES-256 or larger

### Unknown Quantum Algorithms

**Caveat**:
- Other quantum algorithms may exist
- Unknown threats possible
- Active research area
- New vulnerabilities discovered

**Approach**: Conservative security assumptions

---

## Two Approaches to Quantum Threats

### Approach 1: Quantum Cryptography

**What physicists say**:
> "Use quantum technologies to fight quantum technology!"

**Technology**: QUANTUM CRYPTOGRAPHY

**Method**: Quantum key distribution (QKD)

### Approach 2: Post-Quantum Cryptography

**What mathematicians say**:
> "Just base your crypto on math that quantum computers can't break."

**Technology**: QUANTUM RESISTANT or POST-QUANTUM CRYPTOGRAPHY

**Method**: Classical algorithms resistant to quantum attacks

---

## Quantum Cryptography

### Overview

**Purpose**: Use quantum mechanics for secure communication

**Basis**: Quantum key distribution (QKD)

### Limitations in Practice

**Limitation 1: Limited scope**:
- Mainly limited to key distribution
- Cannot encrypt all communication
- Only for key exchange

**Limitation 2: No authentication**:
- Provides no authentication (apart from PUF)
- Need additional mechanisms
- Cannot sign documents

**Limitation 3: Infrastructure requirements**:
- Requires direct fiber-optical connection
- Or line of sight
- Cannot use regular internet

**Limitation 4: Distance problems**:
- Has issues with large distances
- Attenuation in optical fibers
- Quantum repeaters not mature
- Limited practical range

**Limitation 5: New infrastructure**:
- Needs new technology
- Special hardware
- Special infrastructure
- Expensive to deploy

### Why Not Enough Alone

**Conclusion**: Quantum cryptography not practical sole solution

**Reality**: Too limited in scope and too infrastructure-intensive

---

## Post-Quantum Cryptography

### Definition

**Post-Quantum Cryptography (PQC)** refers to **cryptographic algorithms that are thought to be secure against cryptanalytic attack by a quantum computer**

### Core Characteristic

**What makes it quantum-resistant**:
- Secure against quantum computer attacks
- Still run efficiently on classical computers
- Based on different math problems
- Not factorization or discrete log

### Key Difference from Quantum Cryptography

**PQC vs Quantum Crypto**:
- PQC: Classical algorithms (run on normal computers)
- QC: Quantum technologies (needs quantum hardware)

**Advantage of PQC**: Can integrate with existing infrastructure

---

## What is PQC

### Definition Recap

**PQC consists of classical algorithms that**:

1. **Run efficiently on classical computers**:
   - Time complexity reasonable
   - Memory requirements manageable
   - Communication overhead acceptable

2. **Are hard to break**:
   - Secure against classical computers
   - Secure against quantum computers
   - No known attacks (yet)

3. **Rely on different mathematics**:
   - NOT integer factorization
   - NOT discrete logarithms
   - NOT elliptic curves
   - Based on harder problems

### Classification

**Types of PQC**:
- Lattice-based
- Code-based
- Hash-based
- Isogeny-based
- Multivariate polynomial-based

---

## PQC Characteristics

### Characteristic 1: Computational Efficiency

**On classical computers**:
- Faster than some traditional methods
- Comparable to RSA in some cases
- Acceptable for most applications
- Can be optimized

### Characteristic 2: Dual Resistance

**Security against**:
- Classical (traditional) computers
- Quantum computers
- Both attack methods covered

### Characteristic 3: Different Mathematical Basis

**Not based on**:
- Integer factorization (RSA)
- Discrete logarithm (DH, ElGamal)
- Elliptic curve (ECC)

**Based on**:
- Lattice problems (LWE)
- Error-correcting codes
- Hash functions
- Isogenies
- Multivariate polynomials

---

## PQC Families

### Family 1: Lattice-Based Algorithms

**Basis**: High-dimensional lattice problems

**Problem type**: Learning with Errors (LWE)

**Security**: Hard even for quantum computers

**Performance**: Fast, practical

**Examples**: NTRU, Ring-LWE

**Advantages**:
- Well-researched
- Efficient
- Small key sizes

**Status**: NIST recommended

### Family 2: Code-Based Algorithms

**Basis**: Error-correcting codes

**Problem type**: Decoding random linear codes

**Security**: Hard even for quantum computers

**Examples**: McEliece, Niederreiter

**Advantages**:
- Long history (1978)
- Well-studied
- Proven resistant

**Disadvantages**:
- Large key sizes
- Slower than lattice

### Family 3: Hash-Based Algorithms

**Basis**: Cryptographic hash functions

**Problem type**: Inverting hash functions

**Security**: Very solid theoretical basis

**Examples**: Merkle signatures, XMSS

**Advantages**:
- Simple security proofs
- Collision resistance assumption only
- Very secure

**Disadvantages**:
- Signatures larger
- Stateful (must track used keys)
- Slower

### Family 4: Isogeny-Based Algorithms

**Basis**: Elliptic curve isogenies

**Problem type**: Computing isogenies between curves

**Security**: New, less studied

**Examples**: SIKE (Supersingular Isogeny Key Encapsulation)

**Advantages**:
- Small key sizes
- Novel approach
- Interesting math

**Disadvantages**:
- Less analysis
- Potentially slower
- Newer algorithm

### Family 5: Multivariate Polynomial Algorithms

**Basis**: Multivariate polynomial equations

**Problem type**: Solving system of multivariate polynomials

**Security**: NP-complete problem

**Examples**: Rainbow, Oil and Vinegar

**Advantages**:
- Fast signing
- Small signatures

**Disadvantages**:
- Large key sizes
- More recent research
- Some attacks known

---

## Challenges of Post-Quantum Cryptography

### Challenge 1: Lack of Standardization

**Problem**: No widely accepted standards

**Consequence**:
- Organizations uncertain what to implement
- Interoperability issues
- Fragmented implementations
- Risk of wrong choice

**Status**: NIST working on standardization

### Challenge 2: Interoperability Issues

**Problem**: PQC not compatible with existing protocols

**Difficulty**:
- Cannot simply drop in replacement
- Need protocol modifications
- Need software updates
- Need system redesign in some cases

**Impact**: Complex integration into existing systems

### Challenge 3: Performance Concerns

**Reality**: PQC algorithms significantly slower than traditional crypto

**Specific issues**:
- Larger key sizes
- Slower computation
- More memory required
- Communication overhead

**For organizations with limited resources**: Challenging implementation

### Challenge 4: Widespread Adoption Requirement

**Need for coordination**:
- Technology companies
- Financial sector
- Government agencies
- International coordination

**Difficulty**: Getting everyone aligned

**Timeline**: Years of negotiation and transition

### Challenge 5: High Implementation Cost

**Expense areas**:
- New software development
- Hardware upgrades
- Research and development
- Testing and validation
- Staff training
- Infrastructure changes

**Budget impact**: Significant for organizations

### Challenge 6: Key Size Concerns

**Problem**: PQC keys larger than RSA/ECC

**Examples**:
- CRYSTALS-Kyber: 2.4 KB keys
- FALCON: 1.8 KB keys
- Hash-based: Even larger

**Impact**: Storage and transmission overhead

### Challenge 7: Maturity and Trust

**Issues**:
- Newer algorithms less analyzed
- Unknown potential vulnerabilities
- Need long-term study
- Trust takes time to build

---

## Current Status of PQC

### NIST Standardization Process

**What NIST did**:
- Called for PQC algorithm submissions (2016)
- Evaluated candidates (2016-2022)
- Announced winners (2022-2023)

**Significance**: Provides guidance for industry

### NIST's First Four Recommendations

**NIST selected four algorithms**:

#### Category 1: General Encryption Recommendation

**CRYSTALS-Kyber**:
- Lattice-based key encapsulation
- Small keys
- Fast computation
- Proven secure

**Use case**: Key exchange and encryption

#### Category 2: Digital Signature Recommendations

**CRYSTALS-Dilithium**:
- Lattice-based signature scheme
- High efficiency
- Primary recommendation
- Fast and practical

**Use case**: Digital signatures

**FALCON**:
- Lattice-based signature scheme
- High efficiency
- For smaller signatures
- Good for applications needing compact signatures

**Use case**: Where signature size critical

**SPHINCS+**:
- Hash-based signature scheme
- Larger and slower than others
- Very solid security foundation
- Conservative choice

**Use case**: Backup when other methods unsuitable

---

## NIST Standardization

### Process Timeline

**2016**: NIST calls for submissions
**2016-2019**: First round evaluation (69 candidates)
**2019**: Second round (26 finalists)
**2022-2023**: Final round and selection

### Current Status

**As of 2023**:
- CRYSTALS-Kyber standardized (FIPS 203)
- CRYSTALS-Dilithium standardized (FIPS 204)
- FALCON standardized (FIPS 205)
- SPHINCS+ standardized (FIPS 206)

**Future work**: More algorithms under evaluation

---

## CRYSTALS-Kyber

### Overview

**Type**: Key Encapsulation Mechanism (KEM)

**Basis**: Lattice-based cryptography

**Purpose**: Exchange encryption keys securely

### Key Characteristics

**Features**:
- Small key sizes (2.4 KB public, 1.6 KB private)
- Fast key generation
- Fast encapsulation/decapsulation
- IND-CCA2 secure

### Use Cases

**Primary uses**:
- Post-quantum key exchange
- Hybrid with traditional methods (during transition)
- Replacing Diffie-Hellman

### Performance

**Metrics**:
- Fast computation (microseconds)
- Small keys (reasonable for most systems)
- Modest computational overhead
- Suitable for high-volume deployment

### Security Level

**Strength**: NIST Level 3 (equivalent to AES-192)

---

## CRYSTALS-Dilithium

### Overview

**Type**: Digital signature algorithm

**Basis**: Lattice-based cryptography

**Purpose**: Sign and verify documents/transactions

### Key Characteristics

**Features**:
- High efficiency
- 2.4 KB public key
- 2.5 KB private key
- 3.3 KB signature

### Why Primary Recommendation

**Reasons**:
- Best performance/security tradeoff
- Fastest among recommendations
- Practical for most applications
- Well-analyzed

### Use Cases

**Applications**:
- Post-quantum digital signatures
- Document signing
- Blockchain transactions
- Certificate signing
- PKI infrastructure

### Performance

**Metrics**:
- Fast signing (microseconds)
- Reasonably sized signatures
- Practical for production
- Efficient verification

### Security Level

**Strength**: NIST Level 2 (equivalent to AES-128)

---

## FALCON

### Overview

**Type**: Digital signature algorithm

**Basis**: Lattice-based (lattice trapdoor with Gaussian sampling)

**Purpose**: Smaller signatures when size critical

### Key Characteristics

**Features**:
- Very compact signatures (0.7 KB)
- Slightly larger keys than Dilithium
- Trade-off: smaller signatures vs speed

### Why Selected

**Reasons**:
- Smallest signatures among lattice options
- When bandwidth critical: preferred
- For applications with size constraints

### Use Cases

**Applications**:
- Embedded systems
- IoT devices
- Bandwidth-constrained environments
- Mobile applications
- Blockchain transactions (when size matters)

### Tradeoff

**Signature size**: Smaller

**Computation**: Slightly slower than Dilithium

**Key size**: Similar

### Security Level

**Strength**: NIST Level 1 (equivalent to AES-128)

---

## SPHINCS+

### Overview

**Type**: Digital signature algorithm

**Basis**: Hash-based signatures

**Purpose**: Conservative approach based on hash functions only

### Key Characteristics

**Features**:
- Based only on hash functions
- Very simple security argument
- Larger signatures (41 KB typical)
- Stateless (unlike some hash-based schemes)

### Why Selected

**Reasons**:
- Backup option to lattice algorithms
- If lattice cryptanalysis breaks Dilithium/Falcon, SPHINCS+ remains
- Proven security based on hash collisions only
- Very conservative choice

### Security Foundation

**Based on**:
- Hash function collision resistance
- Cryptographically reduced problem
- Very well-understood
- Decades of hash research

### Disadvantages

**Tradeoffs**:
- Much larger signatures (~40 KB)
- Slower generation and verification
- Not primary choice due to size
- But secure "insurance policy"

### Use Cases

**When to use**:
- Long-term security critical
- Want absolute certainty
- Can accept large signatures
- As backup system

---

## Implementation Timeline

### Current Phase (2023-2024)

**What's happening**:
- NIST standards finalized
- First implementations released
- Organizations planning migration
- Hybrid systems being deployed

### Near Term (2024-2026)

**Expected activities**:
- Industry adoption beginning
- Hybrid schemes (classical + PQC) deployed
- Testing in production environments
- Standards becoming more stable

### Medium Term (2026-2030)

**Transition phase**:
- Wider deployment across sectors
- Government mandates implemented
- Legacy system retirement planning
- PQC as standard practice

### Long Term (2030+)

**Full migration**:
- PQC widely adopted
- Classical crypto phased out where possible
- New infrastructure in place
- Post-quantum standard

---

## Summary

### Key Concepts

1. **Y2Q threat**: Quantum computers will break current encryption
2. **Quantum advantage**: Shor's algorithm breaks RSA/ECC
3. **Urgent action needed**: Deployment takes 10+ years
4. **Two approaches**: Quantum cryptography (limited) vs PQC (practical)
5. **PQC families**: Lattice, code, hash, isogeny, multivariate
6. **NIST standards**: Kyber (KEM), Dilithium/FALCON/SPHINCS+ (signatures)
7. **Challenges**: Performance, standardization, integration, adoption

### Critical Points

- Quantum computers pose real threat to current cryptography
- Post-quantum cryptography is practical alternative
- NIST has standardized first PQC algorithms
- Migration timeline starts now
- Hybrid approaches during transition
- Organizations must begin planning

### Current Reality

**Status as of 2024**:
- NIST standards released
- First implementations available
- Organizations evaluating options
- Migration path becoming clear
- Timeline: 10-15 years for full transition

### Future Outlook

**Post-quantum era**:
- PQC becomes standard
- Classical crypto phased out
- New security assumptions
- Ongoing research for improvements
- Possible emergence of new threats

Post-quantum cryptography represents the future of cybersecurity, necessary to maintain privacy and security in an age of quantum computers. Preparation and migration must begin now to ensure smooth transition and continued digital security.
