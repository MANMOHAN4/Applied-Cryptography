# Entity Authentication Methods
## Lecture 30 - Applied Cryptography (UE22CS342AA4)

---

## Table of Contents
1. [What is Authentication](#what-is-authentication)
2. [Why Entity Authentication Matters](#why-entity-authentication-matters)
3. [Authentication vs Authorization](#authentication-vs-authorization)
4. [Common Data Breaches](#common-data-breaches)
5. [5 Common Authentication Types](#5-common-authentication-types)
6. [Challenge-Response Authentication](#challenge-response-authentication)
7. [Types of Challenge-Response Mechanisms](#types-of-challenge-response-mechanisms)
8. [Static Authentication](#static-authentication)
9. [Dynamic Authentication](#dynamic-authentication)
10. [Challenge-Response Examples](#challenge-response-examples)
11. [Advanced Challenge-Response Mechanisms](#advanced-challenge-response-mechanisms)
12. [Uses of Challenge-Response](#uses-of-challenge-response)
13. [Best Practices](#best-practices)

---

## What is Authentication

### Definition

**Authentication** is the process of determining if the person or entity accessing a computing system really is who they claim to be.

### Core Function

**Authentication systems make a binary decision**:
- **Allow** access based on credentials/proof provided
- **Deny** access if credentials fail

### Simple Terms

**Purpose**: Verify identity before granting access

**Process**: 
- User claims identity ("I am Alice")
- System verifies claim ("Prove it")
- If verified, grant access
- If not verified, deny access

---

## Why Entity Authentication Matters

### The Cybersecurity Threat

**When authentication is not secure**:
- Cybercriminals can gain system access
- Sensitive information stolen
- Data compromised
- Systems exploited

### Real-World Data Breaches

#### Adobe Breach

**What happened**:
- Massive security breach
- Customer data stolen
- Passwords compromised
- Authentication systems bypassed

**Impact**:
- Millions of users affected
- Personal information exposed
- Customer trust damaged

#### Equifax Breach (2017)

**What happened**:
- Credit card data exposed
- Personal financial information stolen
- Over 147 million consumers affected
- Weak authentication allowed access

**Impact**:
- Massive identity theft risk
- Long-term security consequences
- Regulatory fines

#### Yahoo Breach (2012-2016)

**What happened**:
- Authentication systems compromised
- Hackers accessed accounts
- Stole contacts, calendars, emails
- Years of undetected compromise

**Types of data stolen**:
- Private emails between users
- Contact lists and relationships
- Calendar events (personal info)
- Recovery information

**Timeline**: 2012-2016 (4 years of undetected access)

### The Lesson

**Without secure authentication**:
- Any organization could be at risk
- Attackers gain system access
- Damage can persist years
- Consequences are severe

---

## Authentication vs Authorization

### Distinction

**Authentication**:
- **Question**: "Are you who you claim to be?"
- **Answer**: Verified identity
- **Process**: Checking credentials
- **Prerequisite**: Must happen first

**Authorization**:
- **Question**: "What are you allowed to do?"
- **Answer**: Access levels and permissions
- **Process**: Checking permissions
- **Follows**: Happens after authentication

### Workflow

```
User → Authenticates (proves identity) → Authorization (what can they access?) → Grant/Deny Access
```

### Example

**Scenario**: Alice tries to access financial records

1. **Authentication**: "Alice enters password. System verifies it matches her stored password. Confirmed: This is Alice."

2. **Authorization**: "System checks Alice's role and permissions. Alice is manager, has access to department financial records but not executive summaries. Grant access to department records only."

---

## 5 Common Authentication Types

### Type 1: Password-Based Authentication

**Mechanism**: User provides password, system verifies against stored password

**How it works**:
- User enters password
- System hashes it
- Compares with stored hash
- If match, authenticated

**Advantages**:
- Simple to implement
- User-friendly
- Widely understood

**Disadvantages**:
- Passwords can be weak
- Subject to brute force
- Can be guessed or socially engineered
- Users reuse passwords

### Type 2: Multi-Factor Authentication (MFA)

**Mechanism**: Multiple forms of verification required

**Factors**:
- Something you know (password)
- Something you have (phone, USB key)
- Something you are (biometric)

**How it works**:
- User enters password (factor 1)
- System sends code to phone (factor 2)
- User enters code
- Both verified, access granted

**Advantages**:
- Much more secure
- Even if password stolen, difficult to bypass
- Harder for attackers

**Disadvantages**:
- More complex for users
- Requires additional devices
- Slower login process

### Type 3: Certificate-Based Authentication

**Mechanism**: Digital certificates with public/private key pairs

**How it works**:
- System verifies certificate
- Validates signature on certificate
- Checks CA signature
- Authenticates based on certificate

**Advantages**:
- Cryptographically secure
- Can't be guessed or brute-forced
- Scalable for large systems
- Non-repudiation

**Disadvantages**:
- Complex PKI infrastructure needed
- Certificate management overhead
- User education required

### Type 4: Biometric Authentication

**Subcategories**:

**Facial Recognition**:
- Scans face
- Compares with stored image
- Unlocks if match

**Fingerprint Scanners**:
- Scans fingerprint
- Compares with stored fingerprint
- Authenticates based on match

**Speaker Recognition**:
- Analyzes voice patterns
- Verifies speaker identity
- Works for voice-only systems

**Iris Scanners**:
- Scans iris pattern
- Highly unique and reliable
- Used in high-security areas

**Advantages**:
- Can't be forgotten or lost
- Difficult to forge
- Fast and convenient
- Unique to individual

**Disadvantages**:
- Requires specialized hardware
- Privacy concerns
- False rejection/acceptance rates
- Cannot change if compromised

### Type 5: Token-Based Authentication

**Mechanism**: Physical or software tokens generate one-time codes

**Types**:
- **Hardware tokens**: Generate codes on device
- **Software tokens**: Apps on phone generate codes
- **Time-based**: Codes valid for limited time
- **Event-based**: Codes change after each use

**How it works**:
1. User enters username/password
2. System sends challenge
3. User enters token-generated code
4. System verifies code
5. Access granted

**Advantages**:
- Very secure (code changes frequently)
- Difficult to fake
- Can work offline (some tokens)

**Disadvantages**:
- Tokens can be lost
- Additional device needed
- User burden
- Hardware cost

---

## Challenge-Response Authentication

### Definition

**Challenge-response authentication** refers to a set of protocols that help validate actions and protect digital assets and services from unauthorized access.

### Structure

**Two-component system**:
1. **Challenge**: Question or random data from verifier
2. **Response**: Answer from user/system

**How it works**:
- **Verifier** presents challenge to user
- **User** must provide correct answer
- If correct, authentication succeeds
- If incorrect, authentication fails

### Core Principle

**The idea**: If user is legitimate, they will have valid answer to challenge

### Challenge-Response Variants

**Can be**:
- As simple as password (static challenge)
- As complex as dynamically generated request
- One-time codes
- Cryptographic proofs
- Biometric matching

---

## Types of Challenge-Response Mechanisms

### Type 1: Static Challenge-Response

**Definition**: Challenges don't change over time. User selects challenge for authentication.

**Characteristics**:
- Fixed questions
- Same response needed each time
- User-selected or system-provided
- Never changes

**Common Example**: "Forgot Password" Feature

**Process**:
1. User forgets password
2. System asks security questions
3. User answers (e.g., "What is your mother's maiden name?")
4. If answer correct, can reset password
5. Same security questions used each time

**Advantages**:
- Easy to implement
- User-friendly
- No special hardware needed
- Works anywhere

**Disadvantages**:
- Questions might have guessable answers
- Information can be researched (maiden names, birthplaces)
- If one answer exposed, authentication broken
- Limited entropy

### Type 2: Dynamic Challenge-Response

**Definition**: Challenges change each time. Users respond to newly generated challenges.

**Characteristics**:
- New challenge each authentication attempt
- Different answer required each time
- Based on assumption real users know answer
- Answers vary

**Common Examples**:
- One-Time Password (OTP)
- Randomly generated tokens
- Cryptographic challenge-response

**How it works**:
- System generates new random challenge
- User must respond with correct answer
- Different for every authentication
- Answer specific to that challenge

**Advantages**:
- Much more secure
- Can't reuse old authentication
- Difficult to brute force
- If one answer exposed, others still valid

**Disadvantages**:
- More complex
- Requires device or algorithm
- User must perform calculation
- Synchronization issues possible

---

## Static Authentication

### Example 1: Security Questions

**Process**:
1. During registration, user answers security questions
2. Answers stored securely
3. During authentication, user asked questions
4. Must match stored answers

**Questions typically**:
- Mother's maiden name
- Favorite pet's name
- First school name
- Birthplace
- Favorite color

**Security issues**:
- Answers might be publicly available
- Can be guessed
- Limited randomness
- Social engineering possible

### Example 2: Username/Password

**Process**:
1. User creates username and password
2. Password stored as hash
3. User enters credentials
4. System verifies

**This is static** because:
- Same password used repeatedly
- Same credentials each time
- Doesn't change per authentication

**Security issues**:
- Password might be weak
- Can be guessed, brute-forced
- Can be stolen via phishing
- Subject to keystroke logging

---

## Dynamic Authentication

### Example 1: One-Time Password (OTP)

**How it works**:
1. User requests authentication
2. System generates unique code
3. Code sent to user (email, SMS)
4. User enters code
5. System verifies (code only valid for minutes)
6. Different code next time

**Security**:
- Code only valid once
- Expires quickly
- Can't reuse old codes
- If intercepted, soon invalid

### Example 2: HMAC-Based OTP

**How it works**:
1. User has secret key stored on device
2. System sends counter value
3. Device computes HMAC(secret, counter)
4. Generates 6-digit code
5. User enters code
6. System verifies using same computation

**Advantages**:
- Works offline
- No server-to-user communication
- Cryptographically secure

### Example 3: Time-Based OTP

**How it works**:
1. Device and server share same time-based seed
2. Current time used as input
3. Generate code from time and seed
4. Code valid for 30 seconds
5. Changes automatically with time

**Advantages**:
- Automatic code generation
- No server communication needed
- Simple for user

---

## Challenge-Response Examples

### Example 1: CAPTCHA

**Purpose**: Distinguish humans from bots

**How it works**:
1. User access website or try login
2. System presents challenge (distorted text, puzzles, etc.)
3. User must solve challenge
4. If correct, access granted
5. If incorrect, denied

**Types**:
- **Image text**: User reads distorted text
- **Image selection**: User selects matching images
- **Puzzle**: User solves visual puzzle
- **Math**: User solves math problem

**Security**:
- Difficult for bots to solve
- Relatively easy for humans
- Not strong authentication (just bot detection)

### Example 2: SCRAM (Salted Challenge Response Authentication Mechanism)

**Purpose**: Secure password authentication without exposing password

**How it works**:
1. User enters password
2. Client adds salt (random data)
3. Client computes: hash(salt, password)
4. Sends hash to server (NOT password)
5. Server computes same hash using stored password
6. Compares hashes
7. If match, authenticated

**Advantages**:
- Password never sent over network
- Hash used only once (salted)
- Protected from replay attacks
- Protected from man-in-the-middle

### Example 3: SSH (Secure Shell)

**Purpose**: Secure access to remote systems

**Authentication methods**:
1. **Password-based**: User enters password
2. **Public key**: Uses public/private key pair
3. **Keyboard-interactive**: System asks questions

**Public key process**:
1. Client sends public key
2. Server generates challenge
3. Client signs challenge with private key
4. Server verifies signature with public key
5. If valid, authentication succeeds

**Advantages**:
- Can't brute force (cryptographic)
- Private key never sent
- No password transmitted
- Very secure

### Example 4: Challenge-Handshake Authentication Protocol (CHAP)

**Purpose**: Authenticate in network protocols (PPP, etc.)

**Three-way handshake**:
1. **Phase 1**: Server sends random challenge
2. **Phase 2**: Client computes hash of (challenge, password) and sends it
3. **Phase 3**: Server verifies hash using stored password

**Process**:
```
Server: "Challenge: 12345"
Client: "Hash = MD5(12345 + password123)"
Server: "Verifies MD5(12345 + stored_password) matches received hash"
Result: Access granted or denied
```

**Security**:
- Password never transmitted
- Challenge changes each time
- Replay attacks prevented
- Hash values only valid once

### Example 5: HMAC-SHA1 Authentication

**Purpose**: Secure authentication using cryptographic HMAC

**How it works**:
1. Client and server share secret key
2. Client includes timestamp or counter
3. Client computes: HMAC-SHA1(secret_key, timestamp)
4. Sends timestamp and HMAC
5. Server computes same HMAC
6. If HMAC matches, authenticated

**Advantages**:
- Cryptographically strong
- Can't be forged without key
- Time-based prevents replay
- Proven secure

### Example 6: Yubico OTP

**Purpose**: Hardware token authentication

**Two methods**:
1. **HMAC-SHA1**: String acts as challenge, hashes with stored secret
2. **Yubico OTP**: Creates OTP code encrypted with stored AES key

**How it works**:
1. User inserts YubiKey
2. YubiKey generates OTP
3. User pastes OTP into login field
4. Server verifies OTP
5. Decrypts with AES key, validates

**Security**:
- Private key embedded in YubiKey
- Can't be extracted
- Very difficult to forge

---

## Advanced Challenge-Response Mechanisms

### OATH Challenge-Response Algorithm (OCRA)

**Purpose**: Cryptographically strong challenge-response protocol

**Developed by**: Initiative for Open Authentication (OATH)

**Features**:
- Combines challenge and response
- Uses HMAC for signatures
- Flexible parameter encoding
- Standardized specification

**How it works**:
1. Server generates challenge
2. Client computes: OCRA(key, challenge, counter, timestamp)
3. Generates cryptographic response
4. Server verifies response

### MD5-Based Challenge Response

**Purpose**: Password authentication through hash comparison

**Used in**: RADIUS servers

**How it works**:
1. RADIUS server directs challenge to client
2. Client creates MD5 hash: MD5(challenge + password)
3. Client sends hash back to server
4. Server uses stored password to compute: MD5(challenge + stored_password)
5. Compares received vs computed hash
6. If match, authenticated

**Process**:
```
RADIUS: "Challenge: abc123"
Client: "Hash = MD5(abc123password)"
RADIUS: "Verifies match with computed hash"
```

---

## Uses of Challenge-Response

### Use 1: Verify Passwords

**Purpose**: Confirm user knows password without transmitting it

**How**:
- System sends challenge
- User's password combined with challenge
- Hash computed and verified
- Password never sent

**Advantage**: Password security maintained during authentication

### Use 2: Distinguish Bots from Humans

**Purpose**: Verify human is accessing, not automated bot

**CAPTCHA example**:
- Distorted text difficult for OCR
- Easy for human to read
- Prevents automated access
- Common on web forms

### Use 3: Train Machine Learning Programs

**Purpose**: Teach AI systems to recognize patterns

**How it works**:
1. Challenge-response puzzles presented
2. ML systems attempt to solve
3. Feedback given on correctness
4. System learns from results
5. Performance improves over time

**Examples**:
- Teaching image recognition
- Pattern matching algorithms
- Decision-making systems
- Security model training

**Benefit**: ML systems improve while also protecting against bots

---

## Best Practices

### Implementation Practices

**1. Automated Enrollment System**
- Automatically invite users to set up challenge-response
- Make process easy and clear
- Provide guidance
- Allow multiple challenges per user

**2. Question Design**
- Avoid questions with guessable answers
- Avoid questions with researched answers
- Use questions user will remember
- Avoid questions based on public information

**3. Question Selection**
- Combination of standardized questions (system)
- Allow user-created questions (more unique)
- User questions harder to guess
- More difficult for attackers to research

**4. Multiple Question Tiers**
- Ask standard questions first
- Then ask user-created questions
- Progressive verification
- More difficult attacks require more success

**5. Question Limits**
- Avoid asking more than 6 questions
- User frustration if too many
- Higher abandon rates with many questions
- Still provides security with 3-6 questions

### Security Practices

**1. Protect Question Answers**
- Encrypt all authentication data
- Don't display answers on screen
- Don't echo answers to other users
- Secure storage with strong encryption

**2. Account Lockout**
- Lock account after N failed attempts
- Prevents brute force attacks
- Typical threshold: 3-5 failures
- Automatic unlock after time period or admin action

**3. Replay Attack Prevention**
- Use time-based codes (expire quickly)
- Use counter-based codes (increment each use)
- Track used codes
- Invalidate old codes

**4. Transport Security**
- Use HTTPS for all communication
- Encrypt answers in transit
- Use TLS/SSL
- Prevent eavesdropping

**5. Account Recovery**
- Provide alternate authentication methods
- Backup verification options
- Recovery codes
- Support team procedures

### User Experience Best Practices

**1. Clear Instructions**
- Explain why authentication needed
- Show progress
- Clear error messages
- Help/support available

**2. Reasonable Timeout**
- Don't make codes expire too quickly
- Codes valid for 30-60 seconds typical
- Allow time for user to copy code
- But expire before reuse possible

**3. Remember Devices**
- Option to not require MFA on trusted device
- Balance security with usability
- Allow admin to revoke trust

**4. Support Multiple Methods**
- Backup authentication methods
- Don't force single method
- Allow user choice where possible
- Fallback options for emergencies

---

## Summary

### Key Concepts

1. **Authentication** verifies identity
2. **Challenge-response** uses verified challenge
3. **Static** challenges never change
4. **Dynamic** challenges change each time
5. **Multi-factor** uses multiple verification methods
6. **Cryptographic** methods are strongest

### Critical Points

- Authentication essential for security
- Data breaches often due to weak authentication
- Multi-factor more secure than password alone
- Challenge-response prevents replay attacks
- Proper implementation critical
- User experience matters for adoption

### Deployment Recommendations

**For consumer systems**:
- Multi-factor authentication recommended
- Support multiple methods
- Easy recovery procedures

**For enterprise systems**:
- Certificate-based authentication
- Challenge-response protocols
- Regular security audits
- Compliance with standards

**For financial systems**:
- Multi-factor required
- Hardware tokens recommended
- Regular re-authentication
- Audit trails maintained

Entity authentication is the first line of defense against unauthorized access and must be implemented correctly to protect valuable systems and data.
