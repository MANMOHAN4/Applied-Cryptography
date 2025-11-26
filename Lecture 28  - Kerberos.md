# Kerberos - Key Distribution Center Protocol
## Lecture 28 - Applied Cryptography (UX231CS641B)

---

## Table of Contents
1. [Introduction to Kerberos](#introduction-to-kerberos)
2. [The Problem: Key Management at Scale](#the-problem-key-management-at-scale)
3. [Symmetric Key Limitations](#symmetric-key-limitations)
4. [The Trusted Third Party Solution](#the-trusted-third-party-solution)
5. [Kerberos Overview](#kerberos-overview)
6. [Kerberos Architecture](#kerberos-architecture)
7. [Kerberos Components](#kerberos-components)
8. [Needham-Schroeder Protocol](#needham-schroeder-protocol)
9. [Kerberos Tickets](#kerberos-tickets)
10. [Kerberos Protocol Notation](#kerberos-protocol-notation)
11. [Kerberos Detailed Protocol](#kerberos-detailed-protocol)
12. [Practical Application: Campus Printing](#practical-application-campus-printing)
13. [Kerberized Login](#kerberized-login)
14. [Alice Requests Ticket to Bob](#alice-requests-ticket-to-bob)
15. [Alice Uses Ticket to Bob](#alice-uses-ticket-to-bob)
16. [Advantages and Disadvantages](#advantages-and-disadvantages)

---

## Introduction to Kerberos

### Name Origin

In **Greek mythology**, Kerberos (Cerberus) is a **3-headed dog** that guards the entrance to Hades

**Security joke**: "Wouldn't it make more sense to guard the exit?"

### What is Kerberos in Security

**Kerberos** is an **authentication protocol based on symmetric key cryptography**

### Key Facts

- **Originated at**: MIT (Massachusetts Institute of Technology)
- **Based on**: Needham-Schroeder protocol
- **Key principle**: Relies on a Trusted Third Party (TTP)
- **Purpose**: Allow users and services to authenticate themselves to each other
- **Goal**: Demonstrate identity to each other

---

## The Problem: Key Management at Scale

### Scenario: Large Organization

**Imagine**:
- Many people, processes, and systems
- Want to communicate in secure fashion
- Dynamic group: constant membership changes
- Entities joining and leaving frequently

### Naive Solution: Pairwise Keys

**Approach**: Each party exchanges encryption key with every other party

**How it works**:
- Any two parties have key for secure communication
- Direct communication without intermediary

**The Problem**: This doesn't scale!

### The Scaling Problem

**For n users**:
- Each user must store and manage **n-1 keys**
- Total system keys: **n × (n-1) / 2** keys
- Grows quadratically with users

**Example: 4 users**:
```
User 1: K12, K13, K14 (3 keys)
User 2: K12, K23, K24 (3 keys)
User 3: K13, K23, K34 (3 keys)
User 4: K14, K24, K34 (3 keys)

Total: 6 unique key pairs
Formula: 4 × 3 / 2 = 6 keys
```

**Example: 100,000 users**:
```
Keys per user: 99,999
Total keys: 100,000 × 99,999 / 2 ≈ 5 billion keys
```

**Example: 1 million users**:
```
Storage per user: ~1 million keys
If each key = 256 bits = 32 bytes
Storage needed: 32 MB per user just for keys
```

### Why This Fails

- **Storage problem**: Each user must store enormous number of keys
- **Key management problem**: Impossible to manage that many keys
- **Update problem**: When new user joins, every existing user needs new key
- **Revocation problem**: When user leaves, must remove from every other user
- **Clearly doesn't scale** for large organizations or networks

---

## Symmetric Key Limitations

### Scalability Comparison

| System | Required Keys |
|--------|----------------|
| **Public key (N users)** | N key pairs |
| **Symmetric key (N users)** | N(N-1)/2 ≈ N² keys |

**The symmetric key case does not scale**

### Why Not Just Use Public Keys?

- Kerberos designed for situations where:
  - Public key infrastructure not available
  - Symmetric key cryptography required
  - Need fast, lightweight authentication
  - Printers, services, embedded systems without PKI support

---

## The Trusted Third Party Solution

### Solution Concept

**Key insight**: Don't store key with every user

**Better approach**: Provide every group member with **single key** for communicating with **Key Distribution Center (KDC)**

### How It Works

**Each user has**:
- Master key for secure communication with KDC
- One key per user (instead of one key per pair)

**When users want to communicate**:
1. User A requests session key from KDC for communicating with User B
2. User B requests session key from KDC for communicating with User A
3. KDC provides session keys
4. Users communicate using session keys

### Storage Requirements

**With TTP approach**:
- **Users with KDC**: N keys (one per user)
- **Services with KDC**: N keys (one per service)
- **Total**: O(n) keys instead of O(n²)

### Visual Representation

```
    User 1 ──K_U1── \
    User 2 ──K_U2── |
    User 3 ──K_U3── |
         .           |    TTP/KDC
         .           |    (holds all keys)
    User n ──K_Un── |
                     |
    Service 1 ──K_S1─ \
    Service 2 ──K_S2─ |
    Service 3 ──K_S3─ |
    Service n ──K_Sn─ /

Every user and service only remembers ONE key!
```

### Advantages

- **Per-user storage**: O(n) instead of O(n²)
- **Scalable**: Works for large organizations
- **Manageable**: Users don't track other users' keys
- **Dynamic**: New users added simply

---

## Kerberos Overview

### What Kerberos Provides

**Core function**: **Authentication service**

**Allows**:
- Users to authenticate themselves
- Services to authenticate themselves
- Mutual authentication between parties

### Why Single Authentication Service?

**Without Kerberos**:
- Multiple services maintain user account records
- Duplicated user databases
- Synchronization problems
- Management nightmare

**With Kerberos**:
- Central authentication authority
- Multiple services don't maintain user records
- Centralized control
- Simplified management

### Trust Model

**Implicit trust**:
- Users implicitly trust Kerberos Authentication Server (AS)
- Services implicitly trust KDC
- AS acts as "introducer"

**Shared secrets**:
- Users have long-term keys with AS
- Services have long-term keys with KDC
- These keys last weeks or months

---

## Kerberos Architecture

### Problem Addressed

**Kerberos solves**:
- How to authenticate user to KDC
- How to get session key for service
- How to authenticate to service
- How to ensure service authenticity

### Real-World Example: Campus Printing

**Scenario at PES University**:

**Problem constraints**:
1. Printer servers must authenticate print requests
2. Students have limited print quota (e.g., 100 pages free, then pay)
3. Not all client hosts authorized to send jobs to all printers
4. Communication must be confidential (not plaintext)
5. Printers have limited computing capability (can't do heavy crypto)
6. Printer server shouldn't be bottleneck (direct client-printer connection)

**Challenge**:
- How to establish direct authenticated confidential link from student laptop to printer?
- Printer is resource-constrained
- Student shouldn't need to maintain password with printer
- Printer shouldn't maintain database of students

**Kerberos solution**: Addresses all these through trusted KDC

---

## Kerberos Components

### Two-Part KDC Architecture

**Kerberos Key Distribution Center has two parts**:

#### 1. Authentication Server (AS)

**Responsibility**: Authentication of clients (users)

**Concerns**:
- Which users are authorized to use system
- User-related security issues
- Client identity verification

**Database**:
- Maintains user database
- User IDs and passwords (or hashes)
- Authentication policies

**Updates**:
- Constantly updated as people join/leave
- User roles change frequently

#### 2. Ticket-Granting Server (TGS)

**Responsibility**: Issuing access tickets to services

**Concerns**:
- Which resources available
- Resource-related security
- Service authorization

**Database**:
- Service information
- Resource load tracking
- Service policies and restrictions

**Updates**:
- Service availability changes
- Resource allocation changes

### Why Separate?

**Separation of concerns**:
- AS focuses exclusively on user authentication
- TGS focuses exclusively on service authorization
- Clear security boundary
- Easier to audit and control

---

## Needham-Schroeder Protocol

### Background

**Needham-Schroeder** is the basic key distribution protocol that Kerberos builds upon

### Four-Step Process

#### Step 1: User Requests Session Key

**Alice wants to talk to Bob**

**Alice sends to KDC**:
```
E(K_A, [ID_A, ID_B, N1])
```

Where:
- K_A = Alice's master key (shared with KDC)
- ID_A = Alice's identifier
- ID_B = Bob's identifier
- N1 = Nonce (number used once)

**Purpose**: Unique identifier to match response with request (Alice may have multiple simultaneous sessions)

#### Step 2: KDC Sends Session Key

**KDC responds to Alice**:
```
E(K_A, [K_S, ID_A, ID_B, N1, E(K_B, [K_S, ID_A])])
```

Where:
- K_S = Session key for Alice-Bob communication
- E(K_A, ...) = Encrypted with Alice's key (only Alice can read)
- E(K_B, ...) = **Ticket for Bob** (encrypted with Bob's key, Alice can't read)
  - Contains session key K_S and Alice's ID
  - Only Bob can decrypt (has K_B)

**Components**:
1. Session key K_S
2. Nonce N1 (to match request)
3. ID_A and ID_B
4. Ticket for Bob (encrypted, opaque to Alice)

#### Step 3: Alice Sends Ticket to Bob

**Alice sends to Bob**:
```
E(K_B, [K_S, ID_A])
```

This is the encrypted ticket from Step 2

**Properties**:
- Alice can't read it (doesn't have K_B)
- Only Bob can decrypt it
- Protected from eavesdropping

#### Step 4: Mutual Authentication

**Bob sends to Alice**:
```
E(K_S, [N2])
```

Where N2 is a nonce generated by Bob

**Alice responds**:
```
E(K_S, [N2 + 1])
```

**Why this works**:
- Confirms session key works
- What Alice encrypts with K_S must differ from what Bob encrypted
- Provides protection against replay attacks
- Proves both parties have same session key

---

## Kerberos Tickets

### What is a Ticket?

**A Kerberos ticket** is information encrypted by KDC

**Contains**:
- Session key
- User's ID
- Expiration time
- Other relevant information

### Ticket Properties

**Every ticket is encrypted with KDC master key (K_KDC)**:
- Can only be read by KDC
- User can't examine contents
- User can't modify contents
- Provides integrity and secrecy

### Ticket-Granting Ticket (TGT)

**TGT** is special ticket for accessing Ticket-Granting Server

**Contains**:
- Session key for client-TGS communication
- Client's ID
- Expiration time

**Use**: Used to obtain tickets for accessing actual services

### Service Tickets

**Tickets for accessing services** (printers, file servers, etc.)

**Contains**:
- Session key for client-service communication
- Client's ID
- Service information
- Expiration time

### Ticket Advantages

**Security advantages**:
- Security transparent to user
- User doesn't manage keys
- Automatic expiration
- Can't be forged (encrypted by KDC)

**Disadvantage**:
- KDC must be secure (it's trusted!)
- Compromise of KDC compromises whole system

---

## Kerberos Protocol Notation

### Key Notation

**Long-term keys** (held by users and services):

| Notation | Meaning |
|----------|---------|
| K_Client | Secret key held by AS for Client |
| K_TGS | Secret key held by AS for TGS |
| K_ServiceProvider | Secret key held by TGS for Service Provider |

### Session Key Notation

**Short-term keys** (generated for specific sessions):

| Notation | Meaning |
|----------|---------|
| K_Client-TGS | Session key from Client to TGS (issued by AS) |
| K_Client-ServiceProvider | Session key from Client to Service (issued by TGS) |

### Understanding

- **K_** prefix = Key
- **Dash** = Connection between parties
- **Example**: K_Client-TGS = Session key for Client communicating with TGS

---

## Kerberos Detailed Protocol

### Complete Protocol Flow

#### Message 1: Client Registration and Request

**Background**:
- Client registers with AS
- Gets user identity and secret password
- Client stores password securely

**Client sends to AS** (plaintext):
- Request for service
- User's identity

#### Message 2-3: AS Sends TGT

**AS sends back to Client** (encrypted with K_Client):

**Message 2 (Part A)**: Session Key
```
E(K_Client, [K_Client-TGS])
```
- Session key for client-TGS communication

**Message 3 (Part B)**: Ticket-Granting Ticket
```
E(K_Client, [K_Client-TGS, E(K_TGS, [ClientID, ClientIP, ValidityPeriod, K_Client-TGS])])
```

**Full Response**:
```
E(K_Client, [K_Client-TGS, E(K_TGS, [ClientID, ClientIP, ValidityPeriod, K_Client-TGS])])
```

**Client Process**:
1. Enter password in dialog
2. Algorithm converts password to K_Client
3. Password immediately destroyed (not kept in memory)
4. Client decrypts message received from AS
5. Client extracts:
   - Session key K_Client-TGS
   - TGT for TGS (opaque - encrypted)

#### Message 4-5: Client Requests Service Ticket

**Client sends two messages to TGS**:

**Message 4**: TGT
- The encrypted ticket meant for TGS
- ID of requested service

**Message 5**: Client Authenticator
```
E(K_Client-TGS, [timestamp, ClientID])
```
- Contains timestamp and client ID
- Encrypted with session key
- Proves client knows session key

**TGS Processing**:
1. Recover TGT from Message 4
2. Decrypt TGT with K_TGS (TGS has this key)
3. Extract K_Client-TGS from TGT
4. Use K_Client-TGS to decrypt Message 5 (authenticator)
5. Authenticate client

#### Message 6-7: TGS Sends Service Ticket

**TGS sends back to Client** (two messages):

**Message 6**: Service Ticket
```
E(K_ServiceProvider, [ClientID, ClientIP, ValidityPeriod, K_Client-ServiceProvider])
```
- Session key for client-service communication
- Encrypted with service's key K_ServiceProvider
- Only service can read it

**Message 7**: Session Key (for client)
```
E(K_Client-TGS, [K_Client-ServiceProvider])
```
- Same session key as in Message 6
- But encrypted with K_Client-TGS (client can decrypt)

**Client Process**:
1. Use K_Client-TGS to decrypt Message 7
2. Extract K_Client-ServiceProvider
3. Keep service ticket (Message 6) opaque

#### Message 8-9: Client Accesses Service

**Client sends to Service Provider** (two messages):

**Message 8**: Service Ticket
- The encrypted ticket received from TGS
- Encrypted with K_ServiceProvider

**Message 9**: Client Authenticator
```
E(K_Client-ServiceProvider, [timestamp, ClientID])
```
- Contains timestamp and client ID
- Encrypted with session key K_Client-ServiceProvider

#### Message 10: Service Confirms

**Service Provider sends to Client**:
```
E(K_Client-ServiceProvider, [timestamp + 1])
```

**Process**:
1. Service decrypts ticket with K_ServiceProvider
2. Extract K_Client-ServiceProvider from ticket
3. Use K_Client-ServiceProvider to decrypt authenticator
4. Send back timestamp + 1 (proves it received and understood)

**Client Verification**:
1. Decrypt message with K_Client-ServiceProvider
2. Verify timestamp matches
3. If correct, client and service authenticated
4. Both now have session key for encrypted communication

---

## Practical Application: Campus Printing

### Scenario at PES University

**Setup**:
- Students' laptops as clients
- Printers as service providers
- Kerberos KDC manages authentication

**Flow**:
1. Student logs in to laptop → Gets TGT from KDC
2. Student sends print job → Laptop requests ticket for Printer
3. TGS issues ticket → Laptop now has session key with Printer
4. Laptop sends print job → Encrypted with session key, includes ticket
5. Printer verifies → Decrypts ticket with K_Printer, gets session key
6. Printer accepts job → Direct encrypted link established

**Advantages**:
- Printer doesn't need to know about individual students
- Direct communication (printer not bottleneck)
- Confidential (encrypted)
- Authenticated (student proved identity to KDC)
- Printer doesn't do heavy crypto

---

## Kerberized Login

### Step 1: Alice Gets TGT

```
Alice enters password
    ↓
K_A = hash(Alice's password)
    ↓
Send request to KDC: (Alice, Kerberos)
    ↓
KDC creates session key SA
    ↓
KDC sends: E(K_A, [SA, TGT])
  where TGT = E(K_KDC, [Alice, SA, ...])
    ↓
Alice's computer decrypts with K_A
    ↓
Extract SA and TGT
    ↓
Forget K_A (password destroyed)
```

**Key points**:
- Password used only to generate decryption key
- Password never transmitted or stored
- Computer forgets password immediately

### What Alice Now Has

**After login**:
- Session key SA (for talking to TGS)
- TGT (opaque, for TGS)
- Can now request services without password

---

## Alice Requests Ticket to Bob

### Request to TGS

**Alice's computer sends**:

**REQUEST** = (TGT, Service: Bob)
- TGT from login
- ID of service requested (Bob)

**AUTHENTICATOR** = E(SA, [timestamp])
- Timestamp encrypted with session key SA
- Proves Alice has SA

### TGS Processing

**TGS receives and**:
1. Recovers SA from TGT
2. Decrypts authenticator using SA
3. Verifies timestamp is current (prevents replay)
4. Creates new session key K_AB
5. Creates ticket for Bob

### TGS Response

**TGS sends**:
```
REPLY = E(SA, [Bob, K_AB, ticket_to_Bob])
  where ticket_to_Bob = E(K_Bob, [Alice, K_AB, K_B])
```

**Alice receives**:
- Service ID (Bob)
- Session key K_AB (for Alice-Bob communication)
- Opaque ticket for Bob

---

## Alice Uses Ticket to Bob

### Alice Sends to Bob

**Message 1**: Service Ticket
```
ticket_to_Bob = E(K_Bob, [Alice, K_AB, K_B])
```

**Message 2**: Authenticator
```
authenticator = E(K_AB, [timestamp])
```

### Bob Processes

**Bob receives and**:
1. Decrypts ticket with his key K_Bob
2. Extracts Alice's ID and session key K_AB
3. Uses K_AB to decrypt authenticator
4. Verifies timestamp
5. Knows Alice is legitimate

### Bob Responds

**Bob sends**:
```
E(K_AB, [timestamp + 1])
```

### Alice Verifies

**Alice decrypts with K_AB**:
1. Verifies timestamp matches what she sent
2. Confirms Bob has same session key
3. Authentication complete

### Secure Communication

**Alice and Bob now have**:
- Shared session key K_AB
- Authenticated each other
- Can use K_AB to encrypt/decrypt all further communication

---

## Advantages and Disadvantages

### Advantages

**1. Centralized Authentication**
- Single point of authentication for all users
- Consistent security policy
- Easier management

**2. Scalability**
- O(n) keys instead of O(n²)
- Works for large organizations
- New users added easily

**3. Transparent to Users**
- Users don't manage keys
- Single password per user
- Automatic ticket management

**4. Reduced Overhead for Services**
- Services don't maintain user databases
- Don't do authentication computation
- Lighter load on services (like printers)

**5. Mutual Authentication**
- Users authenticate to services
- Services authenticate to users
- Both parties assured of identity

**6. Session Keys**
- Short-lived keys for sessions
- Limit damage from compromise
- Automatic expiration

### Disadvantages

**1. KDC as Single Point of Failure**
- If KDC goes down, no authentication possible
- All services affected
- Must be highly available

**2. KDC Security Critical**
- Compromise of KDC = compromise of entire system
- All passwords potentially exposed
- Master keys at risk
- Highest security requirements

**3. Clock Synchronization**
- All parties must have synchronized clocks
- Timestamps used to prevent replay
- Skewed clocks cause authentication failures
- Requires NTP or similar

**4. Ticket Expiration**
- User must renew tickets periodically
- Tickets for services may expire mid-operation
- Requires implementation of renewal

**5. Password-Based Initial Authentication**
- Initial login still password-based
- Passwords transmitted to KDC
- Password attacks still possible at initial step

**6. Replay Attack Vulnerability**
- If timestamp skew allowed too large, more vulnerable
- Requires tight time synchronization

### When to Use Kerberos

**Good for**:
- Large corporate networks
- Multiple services authentication
- Unix/Linux environments
- Microsoft Active Directory (Kerberos-based)
- Enterprise environments where KDC can be protected

**Not suitable for**:
- Small systems (overhead not justified)
- Systems without reliable time synchronization
- Systems that can't protect KDC
- Situations requiring extreme decentralization

---

## Summary

### Key Concepts

1. **Kerberos** is a network authentication protocol based on symmetric cryptography
2. **Solves** the N² key problem through trusted third party
3. **Two components**: Authentication Server (AS) and Ticket-Granting Server (TGS)
4. **Needham-Schroeder** protocol is the basis
5. **Tickets** contain encrypted session keys and user info
6. **TGT** (Ticket-Granting Ticket) obtained at login
7. **Service tickets** obtained from TGS for each service
8. **Session keys** provide encrypted communication

### Critical Points

- One password, multiple services
- No passwords sent to services (only to KDC)
- Tickets automatically expire
- Mutual authentication between all parties
- KDC is critical trust point

### Modern Use

- **Windows Domain**: Uses Kerberos (MIT Kerberos compatible)
- **Unix/Linux**: Kerberos common in enterprise
- **MacOS**: Built-in Kerberos support
- **Kubernetes**: Uses similar token-based auth inspired by Kerberos

Kerberos remains the industry standard for large-scale network authentication, especially in enterprise environments.
