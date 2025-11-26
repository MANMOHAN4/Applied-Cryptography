# Homomorphic Encryption
## Lecture 36 - Applied Cryptography (UE23CS342AA4)

---

## Table of Contents
1. [Introduction to Homomorphic Encryption](#introduction-to-homomorphic-encryption)
2. [Definition and Core Concept](#definition-and-core-concept)
3. [Formal Definition](#formal-definition)
4. [Intuition Behind Homomorphic Encryption](#intuition-behind-homomorphic-encryption)
5. [Key Properties](#key-properties)
6. [Paillier Encryption](#paillier-encryption)
7. [How Paillier Works](#how-paillier-works)
8. [Why Paillier Works](#why-paillier-works)
9. [Other Homomorphic Schemes](#other-homomorphic-schemes)
10. [Types of Homomorphic Encryption](#types-of-homomorphic-encryption)
11. [Partially Homomorphic Encryption (PHE)](#partially-homomorphic-encryption-phe)
12. [PHE Examples](#phe-examples)
13. [PHE Limitations](#phe-limitations)
14. [Fully Homomorphic Encryption (FHE)](#fully-homomorphic-encryption-fhe)
15. [Craig Gentry's Breakthrough](#craig-gentrys-breakthrough)
16. [FHE Advantages](#fhe-advantages)
17. [FHE Challenges](#fhe-challenges)
18. [PHE vs FHE Comparison](#phe-vs-fhe-comparison)
19. [Applications](#applications)
20. [Future Directions](#future-directions)

---

## Introduction to Homomorphic Encryption

### Definition

**Homomorphic Encryption** is a public-key encryption scheme where **operations on ciphertexts correspond to meaningful operations on plaintexts**.

### Core Idea

**Key concept**: Compute on encrypted data without decrypting it

### Significance

**Why it matters**:
- Revolutionary approach to privacy
- Enables computation without exposing data
- Opens new possibilities for secure computation
- Foundation for privacy-preserving cloud computing

### Historical Context

**Evolution**:
- Sought since early days of public-key cryptography
- Partially homomorphic systems (1980s-1990s)
- Fully homomorphic encryption (Craig Gentry, 2009)
- Continued improvements and practical implementations

---

## Definition and Core Concept

### Conceptual Definition

**Homomorphic encryption** allows you to:
1. Encrypt data
2. Perform computations on encrypted data
3. Get encrypted result
4. Decrypt to get plaintext result

**All without seeing the data in plaintext**

### The Magic Property

**What makes it special**:
- Operations work on ciphertexts
- Results correspond to plaintext operations
- Data never exposed to computation
- Complete privacy during computation

### Real-World Analogy

**Like a sealed safe**:
- Data locked in safe (encrypted)
- Instructions sent to computer (operations)
- Computer performs operations on safe contents
- Safe opened only to reveal result
- Safe contents never exposed to computer

---

## Formal Definition

### Mathematical Formulation

**An encryption scheme (Gen, Enc, Dec) is homomorphic if**:

**Requirements**:
- Exist groups 𝒞 (ciphertexts) and ℳ (plaintexts)
- Plaintexts belong to ℳ
- Ciphertexts belong in 𝒞

### The Homomorphic Property

**For any messages m₁, m₂ ∈ ℳ and corresponding ciphertexts c₁, c₂ ∈ 𝒞**:

```
Dec_sk(c₁ · c₂) = m₁ ★ m₂
```

**Where**:
- · is the group operation in 𝒞 (ciphertext space)
- ★ is the operation in ℳ (plaintext space)
- sk is the secret key

### Example

**Additive homomorphic encryption**:
```
If c₁ = Enc(m₁) and c₂ = Enc(m₂)
Then Dec(c₁ + c₂) = m₁ + m₂
```

**The operation on ciphertexts (addition) corresponds to addition on plaintexts**

### Another Example

**Multiplicative homomorphic encryption**:
```
If c₁ = Enc(m₁) and c₂ = Enc(m₂)
Then Dec(c₁ × c₂) = m₁ × m₂
```

**The operation on ciphertexts (multiplication) corresponds to multiplication on plaintexts**

---

## Intuition Behind Homomorphic Encryption

### The Core Insight

**Why compute on encrypted data?**

**Scenario**: You want a cloud provider to:
- Process your data
- But not see your data
- Return result you can verify

**Solution**: Send encrypted data, have them compute on it, get encrypted result

### How It Works Conceptually

**Step 1: Encryption**
- User has plaintext data: m₁, m₂
- User encrypts: c₁ = Enc(m₁), c₂ = Enc(m₂)
- User sends encrypted data to cloud

**Step 2: Cloud Computation**
- Cloud receives ciphertexts
- Cloud cannot read plaintext (encrypted)
- Cloud performs operation: c₃ = f(c₁, c₂)
- Cloud sends back c₃ (encrypted result)

**Step 3: User Decryption**
- User receives encrypted result c₃
- User decrypts: m₃ = Dec(c₃)
- m₃ = f(m₁, m₂) - the correct result!

**Key point**: Cloud never sees plaintext, but computation is correct

### Privacy Guarantee

**What cloud cannot do**:
- See individual data values
- Learn about data content
- Reverse-engineer original data
- Violate user privacy

**What is guaranteed**:
- Computation is correct
- Result is accurate
- Privacy is maintained

---

## Key Properties

### Property 1: Semantic Security

**Definition**: Ciphertexts reveal no information about plaintexts

**Implication**: From ciphertext alone, cannot deduce anything about plaintext

### Property 2: Correctness

**Definition**: Decryption of computed ciphertext gives correct plaintext result

**Formula**:
```
For all m₁, m₂:
Dec(Enc(m₁) OP Enc(m₂)) = m₁ OP m₂
```

**Example**: Adding encrypted numbers gives encrypted sum

### Property 3: Closure

**Definition**: Result of operation is still a valid ciphertext

**Implication**: Can perform multiple operations sequentially

**Example**: Can add three encrypted numbers: c₁ + c₂ + c₃

---

## Paillier Encryption

### Overview

**Paillier cryptosystem**:
- Invented by Pascal Paillier (1999)
- Public-key encryption scheme
- Additively homomorphic
- Can add encrypted numbers

### Type

**Classification**: Partially homomorphic encryption

**Specific type**: Additive homomorphism

### Key Innovation

**Special property**: Can add ciphertexts to get encryption of sum

### Practical Significance

**Why important**:
- First practical additive homomorphic scheme
- Efficient to compute
- Widely used in applications
- Foundation for many protocols

---

## How Paillier Works

### Key Generation

**Generate keys**:
1. Choose two large prime numbers p, q
2. Compute n = p × q
3. Compute λ = lcm(p-1, q-1)
4. Select random g where g ∈ Z*_{n²}
5. Verify certain properties

**Public key**: (n, g)
**Private key**: λ (or p, q)

### Encryption

**To encrypt message m**:
1. Choose random r (coprime to n)
2. Compute: c = g^m · r^n mod n²

**Result**: c is the ciphertext of m

### Decryption

**To decrypt ciphertext c**:
1. Compute: L(c^λ mod n²)
2. Compute: L(g^λ mod n²)
3. Compute: m = [L(c^λ mod n²)] / [L(g^λ mod n²)] mod n

**Result**: m is the plaintext

---

## Why Paillier Works

### The Additive Property

**Mathematical insight**:
- Paillier structure: c = g^m · r^n
- Two ciphertexts: c₁ = g^{m₁} · r₁^n, c₂ = g^{m₂} · r₂^n

### Multiplication of Ciphertexts

**When you multiply ciphertexts**:
```
c₁ · c₂ = (g^{m₁} · r₁^n) · (g^{m₂} · r₂^n)
        = g^{m₁+m₂} · (r₁ · r₂)^n
        = g^{m₁+m₂} · r₃^n
        = c₃ (ciphertext of m₁ + m₂)
```

**Result**: Multiplying ciphertexts multiplies underlying exponentials

### Exponent Addition

**Key insight**:
- g^a · g^b = g^(a+b)
- This means: Enc(m₁) · Enc(m₂) = Enc(m₁ + m₂)

**Why it works**: Exponent addition becomes addition in plaintext space

### Correctness

**Verification**:
- Operations on ciphertexts preserve additive structure
- Decryption recovers correct sum
- Homomorphic property guaranteed by mathematics

---

## Other Homomorphic Schemes

### ElGamal Encryption

**Type**: Multiplicative homomorphic

**Property**: 
```
Enc(m₁) · Enc(m₂) = Enc(m₁ × m₂)
```

**Basis**: Discrete logarithm problem

**Use**: Can multiply encrypted numbers

### Goldwasser-Micali

**Type**: Partially homomorphic

**Property**: XOR (exclusive or) operation homomorphic

**Use**: Binary operations on encrypted bits

### RSA

**Type**: Multiplicative homomorphic

**Property**:
```
Enc(m₁) · Enc(m₂) = Enc(m₁ × m₂)
```

**Basis**: Factorization problem

**Caveat**: Less secure than originally thought for this property

---

## Types of Homomorphic Encryption

### Classification Dimension 1: Operations Supported

**Type 1: Additive**
- Example: Paillier
- Can add encrypted values
- Cannot multiply

**Type 2: Multiplicative**
- Example: ElGamal, RSA
- Can multiply encrypted values
- Cannot add

**Type 3: Mixed**
- Can both add and multiply
- More complex
- Rarer

### Classification Dimension 2: Extent of Operations

**Limited homomorphic**: Only specific operations

**Somewhat homomorphic**: Limited number of operations before noise makes decryption impossible

**Fully homomorphic**: Arbitrary operations, arbitrary depth

---

## Partially Homomorphic Encryption (PHE)

### Definition

**Partially homomorphic encryption** allows **only one type of mathematical operation** (addition OR multiplication) to be performed on ciphertexts.

### Limitations

**What you cannot do**:
- Cannot combine addition and multiplication
- Cannot perform arbitrary computations
- Limited to one operation type

**What you can do**:
- Perform unlimited operations of that type
- Add (in additive PHE)
- Multiply (in multiplicative PHE)

### Scope

**Limited but practical**:
- Sufficient for many applications
- Fast and efficient
- Widely deployed

### Examples by Category

**Additive PHE**:
- Paillier encryption
- Can only add: Enc(m₁) ⊕ Enc(m₂) = Enc(m₁ + m₂)

**Multiplicative PHE**:
- RSA
- Can only multiply: Enc(m₁) ⊗ Enc(m₂) = Enc(m₁ × m₂)
- ElGamal
- Can only multiply

---

## PHE Examples

### Example 1: Paillier (Additive)

**Operation**: Addition only

**What works**:
```
c₁ = Enc(5)
c₂ = Enc(3)
c₃ = c₁ + c₂ = Enc(8)
```

**What doesn't work**:
```
c₃ = c₁ × c₂ ≠ Enc(15)  [NOT homomorphic]
```

### Example 2: RSA (Multiplicative)

**Operation**: Multiplication only

**What works**:
```
c₁ = Enc(5)
c₂ = Enc(3)
c₃ = c₁ × c₂ = Enc(15)
```

**What doesn't work**:
```
c₃ = c₁ + c₂ ≠ Enc(8)  [NOT homomorphic]
```

### Example 3: Real Application - Voting

**Scenario**: Electronic voting with privacy

**Using Paillier (additive)**:
- Vote 0 (no) or 1 (yes)
- Encrypt each vote
- Add encrypted votes: Enc(v₁) + Enc(v₂) + ... = Enc(total)
- Decrypt only final total
- Individual votes remain private

**Cannot do with multiplicative PHE**: Need addition for vote counting

---

## PHE Limitations

### Limitation 1: Single Operation Type

**Problem**: Cannot combine operations

**Example**: Cannot compute (a + b) × c in encrypted form
- Additive PHE: Can do a + b, but not multiply by c
- Multiplicative PHE: Can do a × c, but not add b first

### Limitation 2: Limited Computation

**Scope**: Restricted to applications needing one operation type

**Example**: Cannot implement complex logic:
- Cannot do conditional operations (requires comparisons)
- Cannot do if-then-else
- Cannot implement general programs

### Limitation 3: Practical Applications

**Where PHE works**:
- Voting (addition)
- Threshold cryptography (multiplication)
- Simple aggregation
- Statistical computations

**Where PHE doesn't work**:
- General computation
- Complex algorithms
- Machine learning on encrypted data
- Arbitrary programs

### Limitation 4: Noise Accumulation

**In some schemes**:
- Operations add noise to ciphertext
- Noise grows with each operation
- Eventually decryption fails
- Limits number of operations

---

## Fully Homomorphic Encryption (FHE)

### Definition

**Fully homomorphic encryption** supports **both addition and multiplication** on ciphertexts, enabling **any arbitrary computation** on encrypted data.

### The Breakthrough

**Invented**: Craig Gentry (2009)

**Significance**: First practical FHE scheme

**Foundation**: Based on lattice-based cryptography

### Key Innovation

**Bootstrapping**:
- Repeatedly refresh ciphertexts
- Reduce accumulated noise
- Enable arbitrary depth of computation
- Allow unlimited operations

### Capability

**What FHE enables**:
- Arbitrary programs on encrypted data
- Complex computations
- Machine learning on encrypted data
- General-purpose computation
- Any boolean circuit implementation

---

## Craig Gentry's Breakthrough

### The Problem Before 2009

**Challenge**: Partially homomorphic encryption existed, but fully homomorphic was theoretical

**Goal**: Support both addition AND multiplication

### Gentry's Solution

**Published**: 2009

**Approach**: Ideal lattices

**Core mechanism**: Bootstrapping

### Bootstrapping Concept

**What it does**:
1. Start with noisy ciphertext
2. Encrypt the decryption algorithm
3. Run encrypted decryption on encrypted ciphertext
4. Result: Fresh ciphertext with less noise
5. Enables continued computation

**Why it works**: Self-referential encryption

### Lattice Foundation

**Why lattices**:
- Problems hard against quantum computers
- Rich structure for cryptography
- Supports homomorphic operations
- Better noise properties

---

## FHE Advantages

### Advantage 1: Arbitrary Computation

**Benefit**: Can compute any function on encrypted data

**Example**:
```
Encrypted inputs: Enc(a), Enc(b), Enc(c)
Arbitrary function: f(a,b,c) = (a+b) × (c-1) / (a+1)
Result: Enc(f(a,b,c))
```

### Advantage 2: Privacy-Preserving Cloud Computation

**Scenario**:
- Send encrypted data to cloud
- Cloud performs computation
- Cloud learns nothing about data
- User gets encrypted result
- User decrypts locally

**Benefit**: Complete outsourcing with privacy

### Advantage 3: Encrypted Machine Learning

**Application**: Train or run ML models on encrypted data

**Benefit**:
- Data provider keeps data private
- Cloud performs computation
- Results kept confidential

### Advantage 4: General-Purpose Computation

**Capability**: Supports any algorithm

**Examples**:
- Database queries on encrypted data
- Encrypted search
- Private computation
- Secure multi-party computation

---

## FHE Challenges

### Challenge 1: Computational Expense

**Problem**: FHE is very slow

**Details**:
- Bootstrapping extremely expensive
- Overhead thousands to millions times larger than plaintext
- May take hours for simple operations

**Example**: Simple addition might take seconds or minutes

### Challenge 2: Memory Requirements

**Problem**: High memory consumption

**Details**:
- Ciphertexts much larger than plaintexts
- Intermediate computations need storage
- Key sizes very large

**Example**: 1 bit plaintext → MBs of ciphertext

### Challenge 3: Key Management

**Complexity**:
- Key generation slow
- Large key sizes
- Storage challenging
- Distribution difficult

**Example**: Key generation may take hours

### Challenge 4: Noise Management

**Issue**: Accumulated noise in ciphertext

**Problem**:
- Each operation adds noise
- Noise grows
- Eventually decryption fails
- Requires careful noise tracking

**Solution**: Bootstrapping (but adds overhead)

### Challenge 5: Practicality Gap

**Current state**:
- Theoretically sound
- Practically challenging
- Improving but not yet widely deployable
- Research ongoing for optimization

---

## PHE vs FHE Comparison

### Comparison Table

| Aspect | PHE | FHE |
|--------|-----|-----|
| **Operations** | One type (+ or ×) | Both + and × |
| **Arbitrary computation** | No | Yes |
| **Practicality** | Good | Poor (improving) |
| **Speed** | Fast | Slow |
| **Key size** | Moderate | Large |
| **Memory** | Reasonable | High |
| **Implementation** | Practical | Experimental |
| **Deployment** | Real-world | Research |
| **Use cases** | Limited | General |

### Which to Use

**PHE when**:
- Need simple computation (addition or multiplication only)
- Performance is critical
- Need practical deployment
- Example: Voting, simple aggregation

**FHE when**:
- Need arbitrary computation
- Performance acceptable
- Want general-purpose solution
- Can tolerate overhead
- Example: Encrypted machine learning, general computation

---

## Applications

### Application 1: Privacy-Preserving Cloud Computing

**Use case**: Data owner uses untrusted cloud

**Process**:
1. Owner encrypts data
2. Sends to cloud
3. Cloud computes on encrypted data
4. Returns encrypted result
5. Owner decrypts
6. Owner learns only result

**Benefit**: Cloud cannot access data

### Application 2: Encrypted Machine Learning

**Use case**: Train ML models on sensitive data

**PHE approach**: Limited to simple models

**FHE approach**: Arbitrary ML algorithms on encrypted data

**Benefit**: Model training with privacy

### Application 3: Secure Medical Analysis

**Use case**: Hospital wants analysis without revealing patient data

**Process**:
1. Hospital encrypts patient records
2. Sends to analysis facility
3. Facility analyzes encrypted records
4. Returns encrypted insights
5. Hospital decrypts
6. Analysis complete, data protected

### Application 4: Private Financial Computation

**Use case**: Bank evaluates encrypted financial data

**Example**: Credit scoring on encrypted information

**Benefit**: Privacy for customers

### Application 5: Encrypted Database Queries

**Use case**: Search database without server seeing queries

**Process**:
1. Database owner encrypts data
2. User sends encrypted query
3. Server searches on encrypted data
4. Returns encrypted results
5. User decrypts

**Benefit**: Server learns nothing

### Application 6: Secure Voting

**Use case**: Electronic voting with privacy

**Using PHE**:
1. Each vote encrypted
2. Votes added (using additive HE)
3. Final tally computed
4. Only total revealed

**Benefit**: Individual votes private

---

## Future Directions

### Research Areas

**Active research**:
1. **Efficiency improvements**: Faster FHE computation
2. **Noise reduction**: Better noise management
3. **Key optimization**: Smaller keys, faster generation
4. **Batch processing**: Process multiple values efficiently
5. **Hybrid approaches**: Combine with other techniques

### Emerging Schemes

**New developments**:
- Over-the-top encryption (OVER)
- Gate bootstrapping optimizations
- Approximate FHE
- Functional encryption variants

### Industry Adoption

**Growing interest**:
- Cloud providers exploring FHE
- Healthcare applications
- Financial sector
- Government security applications

### Quantum-Safe Direction

**Future-proofing**:
- Post-quantum homomorphic schemes
- Lattice-based improvements
- Resistance to quantum attacks

### Integration with Other Technologies

**Combining with**:
- Differential privacy
- Secure multi-party computation
- Trusted hardware
- Blockchain

---

## Summary

### Key Concepts

1. **Homomorphic encryption** enables computation on encrypted data
2. **Operations on ciphertexts** correspond to operations on plaintexts
3. **Partially homomorphic** supports addition OR multiplication
4. **Fully homomorphic** supports both operations
5. **Paillier** is practical additive PHE example
6. **Craig Gentry** introduced FHE in 2009
7. **Bootstrapping** enables FHE but with overhead
8. **Privacy-preserving computation** main application

### Critical Points

- Homomorphic encryption is mathematically elegant
- PHE practical and deployed
- FHE still mainly research (improving)
- Trade-off: Security vs performance
- Different schemes for different needs

### Practical Takeaway

**For now**:
- Use PHE for practical applications needing one operation
- FHE emerging but not yet mainstream
- Expect improvements over coming years
- Future likely to see wider FHE adoption

### References

**For deeper learning**:
- Section 11.3.3: "Introduction to Modern Cryptography"
- Visualization: www.zama.org/introduction-to-homomorphic-encryption
- Original papers on Paillier and Gentry's FHE

Homomorphic encryption represents a fascinating frontier in cryptography, enabling computation without exposure, with implications across privacy, security, and cloud computing.
