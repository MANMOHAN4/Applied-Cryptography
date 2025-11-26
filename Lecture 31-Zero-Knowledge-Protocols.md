# Zero-Knowledge Protocols
## Lecture 31 - Applied Cryptography (UE22CS342AA4)

---

## Table of Contents
1. [Introduction to Zero-Knowledge Proof](#introduction-to-zero-knowledge-proof)
2. [Entity Authentication Overview](#entity-authentication-overview)
3. [Definition and Core Concept](#definition-and-core-concept)
4. [Historical Background](#historical-background)
5. [Properties of Zero-Knowledge Proofs](#properties-of-zero-knowledge-proofs)
6. [Types of Zero-Knowledge Proofs](#types-of-zero-knowledge-proofs)
7. [The Safe Analogy](#the-safe-analogy)
8. [Interactive Protocol Structure](#interactive-protocol-structure)
9. [Fiat-Shamir Protocol](#fiat-shamir-protocol)
10. [Fiat-Shamir Setup Phase](#fiat-shamir-setup-phase)
11. [Fiat-Shamir Protocol Steps](#fiat-shamir-protocol-steps)
12. [Bank-to-Client Verification Example](#bank-to-client-verification-example)
13. [Anonymity Considerations](#anonymity-considerations)
14. [Advantages and Disadvantages](#advantages-and-disadvantages)
15. [Applications](#applications)

---

## Introduction to Zero-Knowledge Proof

### Core Concept

**One party can prove to another party that they know a value without revealing the value itself.**

### Simple Explanation

- Person A wants to prove they know a secret
- Person B is the verifier
- The proof reveals ONLY that the knowledge exists
- Nothing else about the secret is revealed

### Cryptographic Goal

Create a system where:
- Prover can convince verifier of knowledge
- Verifier learns nothing beyond the fact stated
- No information about the secret is leaked

---

## Entity Authentication Overview

### What is Entity Authentication?

**Entity authentication** is a technique designed to let one party prove the identity of another party.

### Key Terms

**Entity**: Can be:
- A person
- A process
- A client
- A server

**Claimant**: The entity whose identity needs to be proved

**Verifier**: The party that tries to prove the identity of the claimant

### Methods of Entity Authentication

**Three main approaches**:

1. **Using a password**
   - Simple but not cryptographically strong
   - Subject to eavesdropping

2. **Challenge-response protocols**
   - More secure than passwords
   - Requires interaction

3. **Zero-knowledge protocols**
   - Most secure
   - Cryptographically proven secure

---

## Definition and Core Concept

### Formal Definition

**A zero-knowledge proof (ZKP)** is a method of proving the validity of a statement **without revealing anything other than the validity of the statement itself**.

### Components

**Three essential elements**:

1. **Prover**: Party that demonstrates knowledge
2. **Verifier**: Party that validates the proof
3. **Challenge**: Mechanism to ensure validity

### In Simpler Terms

> "I know X, which I'm not going to tell you, but I can prove to you that this statement involving X is true."

### Key Insight

**Proof system with special property**:
- Gives users ability to publicly share proof of knowledge
- Without revealing details of the knowledge
- Verifiable by anyone
- Secret remains completely protected

---

## Historical Background

### Original Paper

**Title**: "The knowledge complexity of interactive proof systems [GMR85]"

**Published**: 1985

**Authors**:
- Shafi Goldwasser
- Silvio Micali
- Charles Rackoff

### Significance

- Introduced zero-knowledge formally to cryptography
- Pioneering work in proof systems
- Laid foundation for modern applications

### Evolution

**Timeline**:
- 1985: Theoretical foundation established
- 1988: Practical protocols developed (Fiat-Shamir)
- 1990s: Further theoretical work
- 2000s: Practical implementations
- 2010s: Non-interactive zero-knowledge (SNARK)
- 2020s: Blockchain and privacy applications

---

## Properties of Zero-Knowledge Proofs

### Property 1: Completeness

**Definition**: If the statement is true, an honest verifier will be convinced by an honest prover.

**Implication**:
- Valid proofs always succeed
- No false negatives for correct claims
- 100% acceptance rate for true statements

**Example**: If Alice truly knows the secret, Bob will always accept her proof.

### Property 2: Soundness

**Definition**: If the statement is false, no dishonest prover can convince the honest verifier.

**Characteristics**:
- Proof systems are truthful
- Do not allow cheating
- Malicious provers cannot succeed
- Very small probability of false positives

**Example**: If Alice doesn't know the secret, she cannot convince Bob (except with negligible probability).

### Property 3: Zero-Knowledge

**Definition**: If the statement is true, no verifier learns anything other than the fact that the statement is true.

**Meaning**:
- Verifier gains no information about secret
- Only learns that claim is valid
- Cannot use proof to recreate secret
- Cannot impersonate prover

**Example**: Bob learns Alice knows the secret but cannot extract the secret from the proof.

### Combination

**All three properties together guarantee**:
- Honest provers always succeed
- Dishonest provers always fail
- Verifier learns nothing beyond validity

---

## Types of Zero-Knowledge Proofs

### Type 1: Interactive Zero-Knowledge Proofs

**Characteristic**: Prover and verifier engage in **back-and-forth dialogue**

**Process**:
- Multiple rounds of communication
- Prover sends message
- Verifier sends challenge
- Prover sends response
- Cycle repeats

**Communication Pattern**:
```
Prover → Message 1 → Verifier
Prover ← Challenge ← Verifier
Prover → Response 1 → Verifier
...repeat many times...
```

**Advantages**:
- More intuitive
- Easier to understand
- Probability of cheating reduced per round

**Disadvantages**:
- Requires multiple exchanges
- Communication overhead
- Real-time interaction needed

### Type 2: Non-Interactive Zero-Knowledge Proofs

**Characteristic**: Prover sends **single message** to verifier

**Process**:
1. Prover computes complete proof
2. Sends entire proof in one message
3. Verifier checks validity
4. No further communication needed

**Communication Pattern**:
```
Prover → Complete Proof → Verifier
Verifier can verify anytime, anywhere
```

**Advantages**:
- Single message exchange
- Can be sent through insecure channels
- Verifier can verify offline
- Better for public verification

**Disadvantages**:
- More complex to construct
- Larger proof size
- Harder to prove soundness

### Modern Non-Interactive Systems

**zk-SNARK**: Zero-Knowledge Succinct Non-Interactive Argument of Knowledge
- Succinct: Small proof size
- Non-Interactive: Single message
- ARK: Argument of Knowledge

**zk-STARK**: Zero-Knowledge Scalable Transparent Argument of Knowledge
- Scalable: Proof time grows faster than input size
- Transparent: No trusted setup

---

## The Safe Analogy

### Scenario

**You meet someone you don't know** who claims to be a member of your exclusive group.

**Group's exclusive feature**: Only members know the secret combination to the group's locked safe.

**How to verify membership**: You write a secret message and place it in the locked safe.

### The Process

#### Step 1: Verifier Creates Challenge

**Verifier's action**:
- Writes a secret message
- Puts it in a locked safe
- Locks it with combination only members know

**Analogy**: You ask the person to prove they're a member by opening the safe.

#### Step 2: Prover Demonstrates Knowledge

**Prover's action**:
- Has knowledge of the combination (being a member)
- Opens the locked safe
- Shows knowledge without revealing combination

**Analogy**: The person opens the safe, proving they know the combination.

#### Step 3: Proof of Knowledge

**Result**:
- Prover returns the secret message to verifier
- Prover never revealed the combination
- But opening the safe proved knowledge

#### Step 4: Verification Complete

**Verifier's conclusion**:
- The person really knows the combination
- Therefore, they must be a member
- Can now be trusted

### Why This Is Zero-Knowledge

**What verifier learns**:
- Person knows the combination
- Nothing more

**What verifier doesn't learn**:
- The actual combination
- How to open the safe
- Any information to replicate the proof

---

## Interactive Protocol Structure

### Three-Pass Identification Protocol

**Zero-knowledge protocol is fundamentally a 3-pass identification protocol**

#### Pass 1: Commitment (Witness)

**Sent from Alice to Bob**

**Contains**:
- Random value generated by Alice
- Commitment to knowledge
- Cannot reveal the secret

**Purpose**: Bind Alice to her claim

#### Pass 2: Challenge

**Sent from Bob to Alice**

**Contains**:
- Random bit or value
- Tests Alice's knowledge
- Makes each proof unique

**Purpose**: Ensure freshness and prevent replay

#### Pass 3: Response

**Sent from Alice to Bob**

**Contains**:
- Computed response based on challenge
- Depends on Alice's secret
- Only valid if Alice knows secret

**Purpose**: Prove knowledge without revealing it

### Complete Flow

```
Alice ──(Commitment/Witness)──→ Bob
Alice ←──(Challenge)────────── Bob
Alice ──(Response)───────────→ Bob
Bob verifies: Response = f(Secret, Challenge, Commitment)
```

---

## Fiat-Shamir Protocol

### Overview

**A simple protocol that effectively demonstrates Zero Knowledge proofs**

**Developed by**:
- Uriel Feige
- Amos Fiat
- Adi Shamir
- Year: 1988

### Purpose

**Allows one party (Prover) to prove to another party (Verifier) that they possess secret information without revealing that secret information.**

### Key Characteristics

**Probabilistic Protocol**:
- Probability of adversary fooling verifier: 2^(-n)
- Executed n times
- Usually n = 20 to 40 for practical security

**Security**:
- With 20 iterations: 1 in 1,048,576 chance of cheating
- With 40 iterations: Extremely high security

---

## Fiat-Shamir Setup Phase

### Initial Setup

**Before any message exchange begins**

### Components

#### 1. The Secret

**Prover's side**:
- Possesses secret 's'
- Aims to prove knowledge to Verifier
- Secret is never revealed

**Constraint**: 1 ≤ s ≤ n-1

#### 2. The Modulus

**Parameter 'n'**:
- Composite number
- Product of two prime numbers: n = p × q
- Both Prover and Verifier know 'n'
- Typically large: 1024-2048 bits (RSA strength)

**Example**: p = 101, q = 103, so n = 10403

#### 3. The Verification Key

**Parameter 'v' (public verification key)**:
- Calculated by Prover
- Formula: v = (s × s) mod n = s² mod n
- Shared with Verifier (published publicly)
- Used to verify responses

**Example**: If s = 23, then v = 23² mod 10403 = 529

### Setup Summary

```
Setup Phase:
├─ Prover chooses secret: s (1 ≤ s ≤ n-1)
├─ Calculate public key: v = s² mod n
├─ Publish n and v
└─ Keep s secret
```

---

## Fiat-Shamir Protocol Steps

### Phase 1: Commitment Phase

**Prover initiates**

**Prover's actions**:
1. Chooses random number: r (where 1 ≤ r ≤ n-1)
2. Computes commitment: x = r² mod n
3. Sends x to Verifier

**Security**: This is called a "witness" - binds Alice to the interaction

### Phase 2: Challenge Phase

**Verifier challenges**

**Verifier's action**:
- Sends random challenge bit: b ∈ {0, 1}
- Either 0 or 1

**Purpose**: Creates two different paths, only one correct if secret known

### Phase 3: Response Phase

**Prover responds**

**Based on challenge value**:

**If b = 0**:
- Prover sends: y = r
- Simply returns the random value

**If b = 1**:
- Prover sends: y = (r × s) mod n
- Multiplies random with secret

### Phase 4: Verification Phase

**Verifier checks validity**

**Verification equation**:

**If b was 0**:
- Check: y² mod n = x
- Because: r² mod n = x

**If b was 1**:
- Check: y² mod n = (x × v) mod n
- Because: (r×s)² mod n = r² × s² mod n = x × v mod n

### Iteration

**Protocol repeated t times**:
- Each round has 1/2 probability of guessing correctly without secret
- After t rounds: probability of cheating = 2^(-t)
- Typically t = 20-40

---

## Bank-to-Client Verification Example

### Setup

**Scenario**: Client wants to authenticate to bank

#### Setup Parameters

**Trusted source generates**:
```
p = 101, q = 103
n = p × q = 10403
```

**Publicly known**: n = 10403 (bank, client, and everyone)

### Client Registration

**Client setup**:

1. **Choose secret**:
   - Select coprime s: s = 23 (must be 1 ≤ s ≤ n-1)

2. **Calculate public key**:
   - v = s² mod n
   - v = 23² mod 10403
   - v = 529 mod 10403
   - v = 529 (public verification key)

3. **Register with bank**:
   - Submit v = 529
   - Keep s = 23 secret
   - Bank stores v publicly

### Authentication Attempt 1

**Client wants to access account**

#### Commitment Phase

1. **Client chooses random**:
   - r = 21

2. **Compute witness**:
   - x = r² mod n
   - x = 21² mod 10403
   - x = 441

3. **Send to bank**: x = 441

#### Challenge Phase

**Bank chooses**: e = 0

#### Response Phase

**Client responds**:
- Since e = 0: y = r = 21

#### Verification

**Bank checks**:
- Equation: y² ≡ x × v^e (mod n)
- 21² ≡ 441 × 529⁰ (mod 10403)
- 441 ≡ 441 × 1 (mod 10403)
- 441 ≡ 441 ✓ (verified!)

**Result**: Authentication successful for this round

### Authentication Attempt 2

**Second round with same client, same parameters**

#### Challenge Phase

**Bank chooses**: e = 1 (different this time!)

#### Response Phase

**Client must compute**:
- Since e = 1: y = (r × s) mod n
- y = (21 × 23) mod 10403
- y = 483 mod 10403
- y = 483

#### Verification

**Bank checks**:
- Equation: y² ≡ x × v^e (mod n)
- 483² ≡ 441 × 529¹ (mod 10403)
- 233289 mod 10403 ≡ 233289 mod 10403
- Check: 233289 ≡ 441 × 529 ≡ 233289 ✓ (verified!)

**Result**: Authentication successful for second round

### Multiple Executions

**Complete authentication**:

**Protocol executed 2^t times** where t is large:
- Each round: only 50% probability of guessing correctly without knowing s
- With t = 20: (1/2)^20 = 1 in 1,048,576 chance of fooling bank
- With t = 40: virtually impossible to cheat

**Database Entry**: (s=23, v=529, n=10403, r varies each time)

---

## Anonymity Considerations

### Problem with Certificates

**Scenario**: Bob and Alice want to communicate

**Issue 1: Bob doesn't know Alice's public key**:
- Bob needs to verify Alice's identity
- Bob asks for Alice's certificate
- Alice sends certificate

**Issue 2: Certificate reveals identity**:
- Certificate contains identity information
- Identifies Alice to Bob
- Trudy (eavesdropper) can determine Bob is communicating with Alice
- Communication pattern visible to everyone

### Anonymity Challenge

**Problem**:
- ZKP proves knowledge of secret
- But doesn't hide who's proving it
- When public keys are used, identity becomes known
- Difficult to maintain anonymity

### Partial Solutions

**Approaches to improve anonymity**:
1. Anonymous credentials
2. Multiple proofs with different keys
3. Group signatures
4. Ring signatures
5. Blockchain-based privacy

**Limitation**: Complete anonymity hard to achieve with public key systems

---

## Advantages and Disadvantages

### Advantages of Zero-Knowledge Protocols

#### Advantage 1: Security

**Benefit**: Not requiring revelation of one's secret

**Security properties**:
- Secret remains secret
- Cannot be extracted from proof
- Cryptographically proven secure
- No information leakage

#### Advantage 2: Simplicity

**Benefit**: Does not involve complex encryption methods

**Characteristics**:
- Elegant mathematical basis
- Relatively simple to implement (Fiat-Shamir)
- Easy to understand conceptually
- No need for heavy cryptographic machinery

#### Advantage 3: Practical Authentication

**Benefit**: Proves knowledge without key exchange

**Applications**:
- Authentication without password exposure
- Identification systems
- Access control
- Biometric-style authentication

### Disadvantages of Zero-Knowledge Protocols

#### Disadvantage 1: Limited Domain

**Limitation**: Secret must be numerical, otherwise translation needed

**Problem**:
- Designed for mathematical proofs
- Non-numerical secrets require translation
- Overhead in converting to numerical form
- Not natural for all domains

**Example**: Proving you know a picture requires mathematical encoding

#### Disadvantage 2: Computational Overhead

**Limitation**: Lengthy process with many computations

**Issue**:
- 2^k computations where k is security parameter
- Each computation requires time
- Multiple iterations (20-40 rounds)
- Total time = computations × iterations

**Impact**: Performance sensitive applications may be affected

#### Disadvantage 3: Imperfect Security

**Limitation**: Malice can intercept and modify transmission

**Specific vulnerabilities**:
- Messages to verifier might be modified
- Messages to prover might be intercepted or destroyed
- Man-in-the-middle attacks possible
- Requires secure channel

**Example**: Attacker could change challenge bit or response value

#### Disadvantage 4: Implementation Complexity

**Issues**:
- Requires random number generation (must be cryptographically secure)
- Multiple message exchanges needed
- Synchronization required
- Timing attacks possible
- State management complex

#### Disadvantage 5: Trust Requirements

**Concerns**:
- Verifier must trust randomness source
- Both parties must follow protocol correctly
- No recovery if protocol violated
- Difficult to debug security issues

---

## Applications

### Application 1: Authentication Systems

**Use**: Proving identity without password exposure

**Example**: Access to secure facilities, computer systems

### Application 2: Biometric Security

**Use**: Proving identity via biometric without storing biometric

**Benefit**: Store hash of biometric, prove knowledge without revealing data

### Application 3: Blockchain and Cryptocurrency

**Use**: Privacy-preserving transactions

**Example**: zk-SNARKs in Zcash for anonymous transactions

### Application 4: Intellectual Property Protection

**Use**: Proving knowledge of solution without revealing solution

**Example**: Proving correct algorithm without disclosing code

### Application 5: Voting Systems

**Use**: Proving vote cast correctly without revealing vote

**Benefit**: Voter privacy + auditability

### Application 6: Access Control

**Use**: Fine-grained access control

**Example**: Proving attributes (age, employment) without revealing details

---

## Summary

### Key Concepts

1. **Zero-knowledge proof** proves knowledge without revealing the knowledge
2. **Three properties**: Completeness, soundness, zero-knowledge
3. **Interactive** protocols use multiple message exchanges
4. **Non-interactive** protocols use single message
5. **Fiat-Shamir** is practical interactive protocol
6. **Three-pass protocol**: Commitment, Challenge, Response

### Critical Insights

- Mathematically proven secure
- Protects secrets completely
- Verifier learns only validity
- Multiple iterations reduce cheating probability
- Can be applied to various domains

### Modern Relevance

- Cryptocurrency privacy (Zcash, etc.)
- Identity systems
- Blockchain applications
- Privacy-preserving authentication
- Emerging quantum-resistant variants

Zero-knowledge proofs represent elegant solution to fundamental problem: proving knowledge without revealing the knowledge. Their applications continue to expand in privacy-conscious systems and blockchain technology.
