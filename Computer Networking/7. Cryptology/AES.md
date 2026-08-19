### **AES (Advanced Encryption Standard)**

AES is a **symmetric-key block cipher** standardized by NIST in 2001. It replaced DES and is now one of the most widely used encryption algorithms.

AES operates on a fixed **128-bit (16-byte) block**. It supports three different key sizes:

|**Variant**|**Key Size**|**Block Size**|**Rounds**|
|---|---|---|---|
|AES-128|128 bits|128 bits|10|
|AES-192|192 bits|128 bits|12|
|AES-256|256 bits|128 bits|14|

Notice that the block size is **always 128 bits**. `AES-256` refers to a 256-bit key rather than a 256-bit block.

For a plaintext block $P$ and secret key $K$:

$$  
C = E_K(P)  
$$

and decryption performs the inverse transformation:

$$  
P = D_K(C)  
$$

AES itself only defines the transformation of **one 128-bit block**. It does not define how a long message should be divided and encrypted, how an IV/nonce should be generated, or how ciphertext integrity should be verified. These problems are handled by a **mode of operation**.

#### **AES Structure**

AES is a **Substitution-Permutation Network (SPN)**.

A 128-bit input block is represented internally as a $4 \times 4$ matrix of bytes called the **state**.

Each AES round applies several transformations:

1. **SubBytes** — substitutes every byte using a nonlinear S-box.
2. **ShiftRows** — cyclically shifts the rows of the state.
3. **MixColumns** — mixes bytes within each column.
4. **AddRoundKey** — XORs the state with a round key.

For AES-128, the basic process is:

```text
Plaintext
    |
AddRoundKey
    |
    |-- SubBytes
    |-- ShiftRows
    |-- MixColumns      x 9 rounds
    |-- AddRoundKey
    |
    |-- SubBytes
    |-- ShiftRows       final round
    |-- AddRoundKey
    |
Ciphertext
```

The final round does not perform `MixColumns`.

`SubBytes` provides **non-linearity**, while `ShiftRows` and `MixColumns` provide **diffusion**. After several rounds, changing a small part of the plaintext or key causes changes throughout the ciphertext.

`AddRoundKey` introduces the secret key into the transformation:

$$  
S’ = S \oplus K_r  
$$

where $K_r$ is a round key generated from the original AES key through the **key schedule**.

#### **Modes of Operation**

AES only accepts a single 16-byte block. A **mode of operation** describes how the AES primitive is used to encrypt arbitrary-length data.

The distinction is:

```text
AES
 |
 +-- block cipher primitive
       |
       +-- mode of operation
              |
              +-- ECB
              +-- CBC
              +-- CTR
              +-- GCM
```

Therefore, saying that a system “uses AES” does not completely describe its encryption scheme.

##### **ECB**

**Electronic Codebook (ECB)** simply encrypts every block independently:

$$  
C_i = E_K(P_i)  
$$

```text
P1 ---- AES(K) ----> C1
P2 ---- AES(K) ----> C2
P3 ---- AES(K) ----> C3
```

The problem is that:

$$  
P_i = P_j \Rightarrow C_i = C_j  
$$

Identical plaintext blocks produce identical ciphertext blocks.

As a result, patterns in the original data can remain visible in the ciphertext. ECB is therefore generally unsuitable for normal application encryption.

The important point is:

A secure block cipher does not automatically produce a secure encryption scheme.

##### **CBC**

**Cipher Block Chaining (CBC)** makes each block depend on the previous ciphertext block:

$$  
C_i = E_K(P_i \oplus C_{i-1})  
$$

The first block uses an **Initialization Vector (IV)**:

$$  
C_1 = E_K(P_1 \oplus IV)  
$$

Conceptually:

```text
        IV
         |
P1 ---> XOR ---> AES ---> C1
                         |
                         v
P2 ---> XOR ---> AES ---> C2
```

Therefore, even if two messages begin with the same plaintext, different IVs can produce different ciphertext.

Because CBC operates on complete 16-byte blocks, plaintext normally requires **padding**.

A common scheme is PKCS#7 padding. If five bytes of padding are needed:

```text
... 05 05 05 05 05
```

CBC provides encryption but does **not provide authentication by itself**. An attacker may be able to modify ciphertext without the receiver reliably detecting the modification.

This distinction between encryption and authentication becomes important when considering GCM.

##### **CTR**

**Counter Mode (CTR)** changes how AES is used.

Instead of encrypting the plaintext directly, AES encrypts a combination of a nonce and counter:

$$  
S_i = E_K(Nonce \parallel Counter_i)  
$$

This produces a keystream block $S_i$, which is XORed with plaintext:

$$  
C_i = P_i \oplus S_i  
$$

Decryption performs exactly the same XOR:

$$  
P_i = C_i \oplus S_i  
$$

Conceptually:

```text
Nonce + Counter 0 ---> AES(K) ---> S0 ---> XOR P0 ---> C0
Nonce + Counter 1 ---> AES(K) ---> S1 ---> XOR P1 ---> C1
Nonce + Counter 2 ---> AES(K) ---> S2 ---> XOR P2 ---> C2
```

CTR therefore makes a block cipher behave somewhat like a **stream cipher**.

It has several useful properties:

- no padding is required;
- blocks can be processed independently;
- encryption can be parallelized;
- ciphertext length is approximately plaintext length.

However, the nonce/counter sequence must not be reused with the same key.

Suppose two messages reuse the same keystream $S$:

$$  
C_1 = P_1 \oplus S  
$$

$$  
C_2 = P_2 \oplus S  
$$

Then:

$$  
C_1 \oplus C_2  
= P_1 \oplus S \oplus P_2 \oplus S  
= P_1 \oplus P_2  
$$

because:

$$  
S \oplus S = 0  
$$

Therefore, keystream reuse leaks a relationship between the two plaintexts.

#### **AES-GCM**

**AES-GCM (Galois/Counter Mode)** is one of the most commonly used modern AES constructions.

GCM is an **AEAD** mode:

**Authenticated Encryption with Associated Data**

It provides both:

- **Confidentiality** — an attacker should not learn the plaintext.
- **Integrity/authenticity** — an attacker should not be able to modify the message without detection.

Conceptually, GCM combines:

```text
AES-CTR
   |
   +---- confidentiality

GHASH
   |
   +---- authentication
```

Its abstract interface can be represented as:

$$  
(C,T) = Encrypt(K,N,P,A)  
$$

where:

|**Symbol**|**Meaning**|
|---|---|
|$K$|AES key|
|$N$|Nonce|
|$P$|Plaintext|
|$A$|Associated Authenticated Data|
|$C$|Ciphertext|
|$T$|Authentication tag|

Therefore, AES-GCM produces not only ciphertext but also an **authentication tag**.

A stored encrypted value may conceptually look like:

```text
nonce | ciphertext | authentication tag
```

The nonce normally does **not** need to be secret.

A common GCM nonce length is:

```text
12 bytes = 96 bits
```

What matters is that a nonce should not be reused with the same key.

Nonce reuse is particularly dangerous with GCM because GCM’s encryption is based on counter mode, while its authentication construction also depends on correct nonce usage.

#### **Authentication Tag**

The authentication tag allows the receiver to determine whether the encrypted message has been modified.

Conceptually:

```text
                  AES-GCM
                     |
Key ---------------->|
Nonce -------------->|
Plaintext ---------->|----> Ciphertext
AAD ---------------->|----> Authentication Tag
```

During decryption, the receiver verifies the tag.

If an attacker changes:

```text
ciphertext
nonce
AAD
authentication tag
```

the verification should fail.

Therefore, with authenticated encryption, successfully producing some plaintext is not sufficient. The application should only trust the plaintext **after authentication succeeds**.

#### **AAD**

GCM also supports **Associated Authenticated Data (AAD)**.

AAD is:

authenticated but not encrypted.

For example:

```text
AAD:
    version   = 1
    messageId = 10001
    type      = PAYMENT

Encrypted:
    {
        "account": "123456",
        "amount": 100
    }
```

The receiver can read:

```text
version
messageId
type
```

without decrypting them.

However, changing those values causes authentication to fail.

AAD is therefore useful for protocol metadata that should remain visible but must be protected against modification.

#### **Key, IV, Nonce, Salt and Tag**

Several cryptographic values are often confused:

|**Value**|**Purpose**|**Secret?**|
|---|---|---|
|Key|Controls encryption/decryption|Yes|
|IV|Initializes a cipher mode|Usually no|
|Nonce|Prevents reuse of encryption state|No|
|Salt|Randomizes key derivation|No|
|Authentication Tag|Detects modification/forgery|No|

An important distinction is that **nonce does not mean random secret value**.

For many schemes, the important property is uniqueness:

```text
same key + different nonce   -> expected

same key + same nonce        -> potentially dangerous
```

A **salt** serves a different purpose. It is commonly involved when deriving an encryption key from a password:

```text
password + salt
       |
       v
      KDF
       |
       v
    AES key
```

For example:

```text
PBKDF2
scrypt
Argon2
```

A password should generally not be directly converted into bytes and treated as an AES key.

#### **AES-GCM in Java**

Java provides AES through JCA/JCE.

AES-GCM is normally requested using:

```java
Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
```

`NoPadding` is appropriate because GCM does not require block padding in the way CBC does.

A basic encryption example is:

```java
byte[] nonce = new byte[12];

SecureRandom random = new SecureRandom();
random.nextBytes(nonce);

SecretKey key = ...;

GCMParameterSpec spec =
        new GCMParameterSpec(128, nonce);

Cipher cipher =
        Cipher.getInstance("AES/GCM/NoPadding");

cipher.init(
        Cipher.ENCRYPT_MODE,
        key,
        spec
);

cipher.updateAAD(
        "message-v1".getBytes(StandardCharsets.UTF_8)
);

byte[] ciphertextAndTag =
        cipher.doFinal(plaintext);
```

Decryption requires the same:

```text
key
nonce
AAD
```

For example:

```java
GCMParameterSpec spec =
        new GCMParameterSpec(128, nonce);

Cipher cipher =
        Cipher.getInstance("AES/GCM/NoPadding");

cipher.init(
        Cipher.DECRYPT_MODE,
        key,
        spec
);

cipher.updateAAD(
        "message-v1".getBytes(StandardCharsets.UTF_8)
);

byte[] plaintext =
        cipher.doFinal(ciphertextAndTag);
```

If authentication fails, `doFinal()` throws an exception rather than returning trusted plaintext.

#### **Storage and Key Rotation**

The AES key should generally not be stored together with ciphertext.

For example:

```text
Database
------------------------------------------------
id
key_version
nonce
ciphertext_and_tag
------------------------------------------------
```

while the actual key may come from:

```text
Secret Manager
KMS
HSM
protected application configuration
```

A `key_version` or `keyId` becomes useful when rotating keys.

For example:

```text
2025 data ---> key-v1
2026 data ---> key-v2
```

New data can use `key-v2`, while existing ciphertext still records that it needs `key-v1` for decryption.

Hard-coding a key directly in application source code should generally be avoided:

```java
// Bad
String key =
    "0123456789abcdef0123456789abcdef";
```

The difficult part of application encryption is often not the AES algorithm itself, but **key management and correct nonce handling**.

#### **Summary**

AES should be understood at several layers:

```text
AES
 |
 +-- symmetric block cipher
 |     block = 128 bits
 |     key   = 128 / 192 / 256 bits
 |
 +-- Mode of Operation
       |
       +-- ECB
       |     independent blocks
       |     leaks plaintext patterns
       |
       +-- CBC
       |     block chaining
       |     IV + padding
       |     no built-in authentication
       |
       +-- CTR
       |     nonce + counter
       |     AES generates a keystream
       |     nonce reuse is dangerous
       |
       +-- GCM
             CTR-style encryption
             + authentication
             + AAD
             = AEAD
```

Therefore:

```text
"AES-256"
```

alone does not tell us whether an encryption design is secure.

We also need to know:

```text
AES-256
   +
mode
   +
key management
   +
IV / nonce management
   +
authentication
```

For modern application encryption, **AES-GCM** is often preferred because it combines confidentiality and integrity into a standardized authenticated-encryption construction.