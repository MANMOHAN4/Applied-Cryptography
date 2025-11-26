# Key Distribution and Management
## Lecture 27 - Applied Cryptography (UX22CS641B)

---

## Table of Contents
1. [Security Services Recap](#security-services-recap)
2. [Why Key Management Matters](#why-key-management-matters)
3. [Objectives of Key Management](#objectives-of-key-management)
4. [Kerckhoffs' Principle](#kerckhoffs-principle)
5. [Key Management Fundamentals](#key-management-fundamentals)
6. [The Key Lifecycle](#the-key-lifecycle)
7. [Key Generation](#key-generation)
8. [Key Derivation](#key-derivation)
9. [Key Establishment](#key-establishment)
10. [Key Hierarchy](#key-hierarchy)
11. [Key Storage](#key-storage)
12. [Key Backup](#key-backup)
13. [Key Archival](#key-archival)
14. [Key Recovery](#key-recovery)
15. [Key Usage](#key-usage)
16. [Key Change](#key-change)
17. [Key Destruction](#key-destruction)
18. [Key Length and Lifetime](#key-length-and-lifetime)
19. [Public Key Infrastructure](#public-key-infrastructure)
20. [Key Management Policies and Procedures](#key-management-policies-and-procedures)

---

## Security Services Recap

### Five Core Security Services

**1. Confidentiality**
- Assurance that data cannot be viewed by an unauthorized user
- Keeps information secret from unauthorized parties
- Primary goal of encryption

**2. Data Integrity**
- Assurance that data has not been altered in an unauthorized or accidental manner
- Detects modification or tampering
- Ensured through cryptographic hash functions and MACs

**3. Entity Authentication**
- Assurance that a given entity is involved and currently active in a communication session
- Prevents replay attacks
- Ensures live participation of entities
- Current authentication, not historical

**4. Data Origin Authentication (Message Authentication)**
- Assurance that a given entity was the original source of received data
- Identifies who created or sent the message
- Proves authorship
- Also called message authentication

**5. Non-repudiation**
- Assurance that an entity cannot deny it is the original source
- Prevents sender from denying they sent a message
- Legal accountability
- Essential for digital signatures and contracts

---

## Why Key Management Matters

### The Critical Reality

**Core Principle**: 
- Cryptographic primitives are **rarely compromised** through weaknesses in their design
- **Often compromised** through **poor key management**

### Key Management is Complex

**Challenge**:
- Key management is a **complex and difficult** aspect of any cryptosystem

**Application-Dependent**:
- Different solutions needed for different applications:
  - **Banks**: Highly secure, audited, compliance-heavy
  - **Military organizations**: Maximum security, air-gapped systems
  - **Mobile telephone networks**: Billions of users, scalability critical
  - **Home personal computers**: User-friendly, limited resources
  - **Historical example**: Enigma machine security was compromised through poor key management, not the encryption algorithm itself

### Why It's Critical

Strong cryptography is useless without strong key management. The weakest link in the chain determines overall security.

---

## Objectives of Key Management

### Four Primary Objectives

**Objective 1: Protecting Confidentiality and Authenticity of Secret Keys**
- Secret keys (symmetric keys, private keys) must remain secret
- Only authorized parties should know them
- Protects key value from disclosure
- Essential for cryptographic security

**Objective 2: Protecting Against Unauthorized Use**
- Secret keys must be protected from misuse
- Only authorized parties can use keys
- Prevents key compromise from leading to data compromise
- Implements access control on cryptographic operations

**Objective 3: Protecting Authenticity of Public Keys**
- Unlike secret keys, public keys don't need secrecy
- But they must be authentic
- Must belong to claimed owner
- Must be valid for intended purpose
- Protection through certificates and PKI

**Objective 4: Ensuring Availability**
- Cryptographic keys must be available when needed
- Loss of key may mean loss of data (encrypted data becomes unrecoverable)
- Requires backup and recovery mechanisms
- Balance between security and availability

---

## Kerckhoffs' Principle

### The Principle

**Statement**:
> "The security of a cryptosystem should not rely on the secrecy of any of its workings, except for the value of the secret key."

### Implication for Key Management

**Core Truth**:
- Cryptographic algorithms can be public (and are)
- Security depends entirely on key secrecy
- Therefore, **good key management is essential** to benefit from cryptography

### Consequence

If you cannot keep keys secret, all the mathematical strength of the cryptographic algorithm means nothing.

---

## Key Management Fundamentals

### Four Types of Controls

#### 1. Technical Controls

**Definition**: Specialized hardware and software for key management

**Examples**:
- Special hardware devices for storing keys
- Hardware Security Modules (HSMs)
- Cryptographic protocols for key establishment
- Encrypted storage systems
- Key encryption keys (KEK)

#### 2. Process Controls (Policies, Practices, Procedures)

**Definition**: Operational and organizational guidelines

**Examples**:
- Business continuity processes for key loss
- Key rotation schedules
- Backup procedures
- Recovery procedures
- Documentation and records

#### 3. Environmental Controls

**Definition**: Physical security and location considerations

**Examples**:
- Physical location of key storage devices
- Access to physical facilities
- Environment monitoring
- Protection against theft or sabotage

#### 4. Human Factors

**Definition**: People and their roles

**Observation**: Many key management systems rely, at their highest level, on manual processes

**Examples**:
- Personnel training
- Trust and verification
- Segregation of duties
- Access control policies

### Current Focus

- **Symmetric keys**: Used for encryption
- **Private keys**: Decryption and signature keys
- **Public keys**: Discussed separately

---

## The Key Lifecycle

### Five Main Phases

**Phase 1: Key Generation**
- Creation of new cryptographic keys
- Requires secure random number generation
- Most critical phase for security

**Phase 2: Key Establishment**
- Process of making keys available where needed
- Getting keys to endpoints that will use them
- **Arguably the most difficult phase to implement**
- Requires secure distribution

**Phase 3: Key Storage**
- Storing keys securely
- Keys vulnerable when "sitting around"
- Requires strong storage mechanisms

**Phase 4: Key Usage**
- Using keys for cryptographic operations
- Must ensure keys used only for intended purpose
- Monitor for misuse

**Phase 5: Key Destruction**
- Secure removal of keys when no longer needed
- Can't just delete - requires secure overwriting
- Must also destroy related documentation

### Additional Phases

**Key Backup**: Securing copies for recovery
**Key Archival**: Long-term storage for verification
**Key Change**: Updating keys (planned and unplanned)
**Key Recovery**: Recovering keys from backups

---

## Key Generation

### Requirements for Secure Key Generation

**1. True Randomness**
- Keys must be generated using secure random number generation
- Hardware-based non-deterministic generators preferred for high-value keys
- Software-based generators acceptable for lower-value local keys

**2. Application-Appropriate Methods**

**Examples**:
- **Master keys**: Hardware-based, non-deterministic generator
- **Session keys**: Can use software generator based on mouse movements or other sources
- **Local storage keys**: Software generator acceptable for personal file encryption

**3. Entropy Requirements**
- Must have sufficient entropy
- No predictable patterns
- Uniform distribution across key space

### Key Generation Costs

- Expensive process
- Often multiple keys needed
- Solutions: Key derivation, key hierarchy

---

## Key Derivation

### Concept

**Definition**: Generating new cryptographic keys from other cryptographic keys or a base key

**Properties**:
- One-way process
- Protects base key if derived keys compromised
- Perfect forward secrecy property

### Benefits

**Efficiency**:
- Expensive to generate and establish every key
- Generate one base key, derive many keys
- Significant cost savings

**Practical Use Case**:
- **Scenario**: Application needs both confidentiality and authentication
- **Old approach**: Generate and establish two keys (encryption key + MAC key)
- **New approach**: Generate one key K, derive K1 and K2 from it

**Formula**:
```
K1 = KDF(K, "encryption", domain)
K2 = KDF(K, "authentication", domain)
```

### Security Property

Compromise of derived key K₂ doesn't compromise base key K or sibling key K₁.

---

## Key Establishment

### Definition

**Key establishment** is the process of getting cryptographic keys to the locations where they will be used.

### Scenarios

#### Scenario 1: Keys Don't Need to Be Shared
- Symmetric key for encrypting data on local machine
- No distribution needed
- Easier case

#### Scenario 2: Keys Need to Be Shared
- **Difficult problem**
- Two parties need same key
- How to get it to both safely?
- **Solutions**:
  - **Key agreement**: Both parties contribute to key (Diffie-Hellman)
  - **Key distribution**: Trusted third party distributes key (Kerberos)

#### Scenario 3: Pre-distribution
- Keys pre-loaded onto devices
- Mobile phones, set-top boxes, etc.
- Problems: Key changes difficult, requires device replacement

---

## Key Hierarchy

### Concept

**Key hierarchy** organizes keys by importance level

### Structure

```
Level 1 (Top): Master Keys (most important)
    ↓
Level 2: Key Encryption Keys (KEK)
    ↓
Level 3: Session/Data Keys (working keys)
```

### Key Properties

**Level-dependent lifetime**:
- Keys at one level used to encrypt keys at level beneath
- Master keys: Longest lifetime (years)
- Session keys: Shortest lifetime (minutes to hours)

**Hierarchy property**:
- Longevity increases at higher levels
- Keys at level n must last at least as long as keys at level n+1

### Advantages

**Simplifies key management**:
- Change low-level keys without replacing high-level keys
- Expensive to establish high-level keys infrequently
- Cheap to establish session keys frequently

**Secure distribution and storage**:
- Low-level keys stored encrypted
- Protected by higher-level keys
- Focuses security effort on top-level keys

**Storage location**:
- Master keys stored in Hardware Security Modules (HSMs)
- Session keys can be stored in software
- Clear security boundary

### Implementation Example

```
Master Key (stored in HSM, lifetime: 5 years)
    ↓
Key Encryption Keys (KEK) - lifetime: 1 year
    ├─ Encrypted with Master Key
    ↓
Session Keys - lifetime: 1 hour
    ├─ Encrypted with KEK
    ├─ Used for actual encryption/decryption
```

---

## Key Storage

### Storage Devices

**Three types** based on security capabilities:

#### 1. General Purpose Devices
- General-purpose devices running conventional operating systems
- Default built-in security controls
- **Example**: Laptop running Windows/macOS/Linux
- Lowest security tier
- Easier to attack, more vulnerable

#### 2. Dedicated Devices
- Offer specialist security controls
- Limited tamper resistance
- **Examples**: Point-of-sale terminals, mobile phones
- Mid-tier security
- Specialized for specific applications

#### 3. Specialized Devices
- Designed primarily for security provision
- **Example**: Hardware Security Module (HSM)
- Highest security tier
- Expensive but most secure

### Storage Environments

**Three types** based on physical security:

#### 1. Uncontrolled Environment
- Public locations (shops, restaurants, trains)
- Not possible to implement strict access control
- Least secure
- Keys here are vulnerable

#### 2. Partially Controlled Environment
- General offices, homes, universities
- Basic access control (physical door key)
- Medium security
- Some protection possible

#### 3. Controlled Environment
- High-security offices, military installations, vaults
- Strong access control mechanisms possible
- Biometric swipe cards, multi-factor authentication
- Most secure

### Storage Strategy

**Optimal matching**:
- High-value keys → Specialized devices in controlled environments
- Medium-value keys → Dedicated devices in partially controlled environments
- Low-value keys → General-purpose devices in any environment

---

## Key Backup

### Need for Backup

**The Problem**:
- Data stored in encrypted form is lost if decryption key is lost
- Nobody can recover encrypted data without key
- Loss of critical keys = Loss of critical data

### Backup Requirements

**Security must match key security**:
- Backup security >= Key security
- Don't use weak encryption for backup
- **Example**: Don't back up AES-256 key using DES encryption

### Storage Requirements

**Backup media**:
- Must be stored on media with same security level as key itself
- Must have same device and environmental controls
- Cold storage, encrypted, physically secure

### Important Limitations

**Private signature keys**:
- **Should NOT be backed up** (when possible)
- Reason: Backing up compromises non-repudiation property
- If someone has backup, could claim they didn't sign something
- Exception: CAs may back up their signing keys

**Other private keys**:
- May warrant backup in some cases
- When backed up, must be under owner's control
- Must maintain security

---

## Key Archival

### Definition

**Key archival** is the long-term storage of cryptographic keys required between their expiry and destruction.

### Purpose

**Why archive?**
- Verification of old digital signatures beyond key expiry
- Decryption of old encrypted data
- Legal/regulatory requirements
- Audit trail maintenance

### Characteristics

**Archived keys**:
- No longer "live"
- Cannot be used for new cryptographic operations
- Only for verification of past operations

### Regulatory Requirements

**Time periods**:
- Financial data often requires keys archived for **7 years**
- Different industries have different requirements
- Some data never expires (historical records)

### Types That Cannot Be Archived

**Private signature keys**:
- Generally should NOT be archived
- Could compromise non-repudiation
- Someone with archived key could claim they signed something

**Private authentication keys**:
- Generally should NOT be archived
- Compromises authentication guarantees

**Public verification keys**:
- **CAN and SHOULD be archived**
- Needed to verify old signatures
- No security risk (already public)

### Management

**Storage management**:
- Same care as key backup
- Must maintain access control
- Encrypted, physically secure

**Destruction**:
- Once no longer needed, must be destroyed securely
- Follow destruction procedures

---

## Key Recovery

### Definition

**Key recovery** is the process of retrieving a key from backup or archive.

### Technical Difficulty

**Technically simple**:
- Same as retrieving any data from storage
- No harder than backup/restore operations

### Management Challenges

**All complexity in management processes**:
- Who can authorize recovery?
- What records must be kept?
- How to prevent unauthorized recovery?
- Audit trail requirements

### Authorization Requirements

**Critical safeguard**:
- Must not be possible to recover key unless suitably authorized
- Requires approval processes
- Need to record who, when, why
- Segregation of duties

---

## Key Usage

### Fundamental Principle

**Keys for specific purposes**:
- Encryption keys can only encrypt
- Authentication keys can only authenticate
- Signature keys can only sign
- Don't mix purposes

### Different Key Requirements

**Different cryptographic operations have different requirements**:
- **Encryption**: Key size based on encryption security needs
- **Entity authentication**: Key size based on authentication security needs
- **Data origin authentication**: May have different lifetime than encryption keys

**Example**:
- Email encryption key might have lifetime of 1 year
- Email authentication key might have lifetime of 3 months
- Different threat models, different aging requirements

### Implementation

**Key purpose field**:
- Certificates and key metadata should specify allowed uses
- Enforcement through policy
- Systems check before using key

---

## Key Change

### Planned Key Changes

**Regular scheduled changes**:
- End of key lifetime
- Preventive measure against cumulative attacks
- Regular "fire drills" to test change procedures
- Most common for long-lifetime keys

**Benefits**:
- Limits damage from compromised key
- Tests change procedures when not emergency
- Distributes key management load
- Regular practice improves efficiency

### Unplanned Key Changes

**Triggered by events**:

**1. Key compromise**:
- Must change immediately
- Investigate how compromised
- Implement preventive measures

**2. Security vulnerabilities**:
- Operating system vulnerabilities
- Cryptanalysis breakthroughs
- HSM tamper mechanism failures
- Force changes to use different algorithms/key sizes

**3. Employee departure**:
- **Good terms**: Just withdraw personal keys (symmetric keys specific to them, personal key pairs)
- **Concern**: Employee had group keys shared with others
- **Action**: Change all group keys immediately

### Coordinating Key Changes

**Challenges**:
- Multiple users/systems
- Must maintain consistency
- Cannot have gaps where no valid key exists
- Cannot have overlapping periods too long

**Process**:
- Distribute new key
- Maintain old key for period
- Switch all users
- Destroy old key

---

## Key Destruction

### When to Destroy

**Three scenarios**:

**1. Key expires**
- Reaches natural end of lifetime
- No longer used

**2. Key is withdrawn**
- Before expiry, due to unplanned events
- Compromise, vulnerability, or policy change

**3. End of archival period**
- Was archived, but now no longer needed
- Archival period complete

### Destruction Method

**Secure destruction critical**:
- Simply deleting is insufficient
- Key remains in memory/storage
- Can be recovered by forensic techniques

**Proper destruction**:
- Repeatedly overwrite memory with random data
- Multiple passes (DOD standard: 7 passes)
- Use certified destruction tools
- Different for different media types

**Documentation destruction**:
- Paper copies
- Backup media
- Any related documents
- All must be securely destroyed

### Hardware Destruction

- Shredding devices
- Degaussing (magnetic erasure)
- Incineration (for highly sensitive)
- Certified destruction services

---

## Key Length and Lifetime

### Relationship

**Key length closely linked to intended lifetime**:
- **Longer keys** = Can be used longer
- **Shorter keys** = Must be changed more frequently

### Why Finite Lifetimes

**Reasons for expiration**:

**1. Limit key compromise impact**:
- If key compromised, damage limited to key lifetime
- Attacker can't decrypt all past and future messages
- Forces regular changes

**2. Mitigate management failures**:
- Annual key change forces out old employees
- If employee retains key through management failure, they can't use it next year
- Automatic rotation provides safety net

**3. Algorithm aging**:
- Longer lifetime = More opportunity for attack
- As computing power increases, key size needs increase
- Shorter-lived keys avoid obsolescence

### Key Strength Over Time

**Example**: AES-256 key strength changes
- 2010: Excellent security
- 2020: Still excellent
- 2030: Possibly weakened by quantum computing or attacks
- Shorter-lived keys avoid this risk

---

## Public Key Infrastructure

### Definition

**PKI** is often misunderstood as just "public-key cryptography"

**Correct definition**: PKI is the **supporting key management system** for public-key cryptography

### Scope

**Important clarification**:
- PKI strongly associated with certificates
- But **all cryptosystems need key management**
- Symmetric key systems need KM too
- PKI is just the name for public-key KM systems

### Components

**PKI typically includes**:
- Certificate Authorities (CAs)
- Certification Revocation Lists (CRLs)
- Online Certificate Status Protocol (OCSP)
- Repositories for certificates
- Trust policies and procedures

---

## Key Management Policies and Procedures

### Three Levels

#### 1. Key Management Policies

**Definition**: Define overall requirements and strategy

**Example**:
- "All cryptographic keys are stored only in hardware"
- "Master keys must be stored in HSMs"
- "Keys must be rotated annually"
- "All keys must use AES or better"

**Characteristics**:
- High-level strategic guidance
- Organization-wide
- Set by management/governance
- Define "what we want to achieve"

#### 2. Key Management Practices

**Definition**: Define tactics to achieve policy goals

**Example**:
- "All devices using cryptography will have in-built HSM"
- "All PKIX certificates issued by internal CA"
- "All session keys derived from master key"

**Characteristics**:
- Mid-level tactical guidance
- How policies are implemented
- Provide concrete mechanisms
- Define "how we'll do it"

#### 3. Key Management Procedures

**Definition**: Document step-by-step operational tasks

**Example**:
- Specification of key establishment protocol
- Step-by-step procedure for key backup
- Detailed process for key archival
- Certificate enrollment process

**Characteristics**:
- Low-level operational guidance
- Step-by-step instructions
- Detailed and specific
- Define "the exact steps"

### Hierarchy

```
Policy (What we want)
    ↓
Practices (How we'll achieve it)
    ↓
Procedures (Exact steps)
```

---

## Considerations for Key Management

### Network Topology
- Network complexity affects key management
- More complex network = More keys needed
- More endpoints = More distribution challenges
- Scalability critical

### Cryptographic Mechanisms
- Symmetric key management different from public-key
- Different key lifecycle phases have different importance
- Technology choices affect key management approach

### Compliance Restrictions
- Legal requirements for key recovery
- Archival mandates
- Interoperability requirements
- Industry-specific regulations

### Standards
- NIST SP 800-57: Comprehensive recommendations
- PKI standards
- Industry standards
- Full compliance extremely hard to achieve
- Most organizations comply with "spirit" not "letter"

### Real-World Challenges

**Key management is hard**:
- Different implementations
- Concerns about assurance
- No one-size-fits-all solution
- Requires continuous review and update

---

## Summary

### Key Concepts

1. **Key management is critical** - Cryptography only as strong as key management
2. **Four objectives**: Confidentiality/authenticity, prevent misuse, public key authenticity, availability
3. **Five phases**: Generation, establishment, storage, usage, destruction
4. **Key hierarchy** essential for scalability and security
5. **Policies, practices, procedures** needed at all levels
6. **No perfect solution** - Must balance multiple requirements

### Critical Reminders

- Kerckhoffs' Principle: Security depends only on key
- Master keys most valuable, require most protection
- Key backup needed but must maintain security
- Key destruction must be secure
- PKI is not just certificates - it's entire KM system

### Best Practices

- Use HSMs for master keys
- Implement key hierarchy
- Regular key rotation
- Secure destruction
- Strong policies and procedures
- Regular audit and assessment
- Keep systems updated
- Train personnel
- Document everything

Key management is the foundation upon which all cryptographic security rests. Neglecting it is the most common way cryptographic systems fail.
