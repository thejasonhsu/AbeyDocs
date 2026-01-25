_Last updated: January 2026_

# Account and Address Format

This document describes how **Abey account addresses** are derived from public keys, including hashing, prefixing, checksumming, and Base58 encoding.

---

## Overview

An Abey address is derived from a **64-byte public key** using the **Keccak-256 (SHA3)** hash function, followed by a prefix and checksum process.

The final address is encoded using **Base58Check**, similar in concept to Bitcoin-style addresses.

---

## Address Derivation Process

### 1. Public Key Hashing

- Input: Public key **P** (64 bytes)
- Hash function: `Keccak256`
- Output: Hash **H**

```text
H = Keccak256(P)
```

---

### 2. Extract Address Payload

- Take the **last 20 bytes** of the hash `H`
- Prepend the **3-byte network prefix**:

```
0x43E552
```

```text
address_payload = 0x43E552 || H[12:32]
```

This produces the raw address payload.

---

## Checksum Calculation (Base58Check)

To protect against input errors, Abey uses a checksum similar to Base58Check encoding.

### 3. First Hash

```text
h1 = Keccak256(address_payload)
```

---

### 4. Second Hash

```text
h2 = Keccak256(h1)
```

---

### 5. Append Checksum

- Take the **first 4 bytes** of `h2`
- Append them to the end of the address payload

```text
checksum = h2[0:4]
address_with_checksum = address_payload || checksum
```

---

## Base58 Encoding

### 6. Encode Final Address

The final byte sequence is encoded using Base58 with the following character set:

```text
ALPHABET = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
```

```text
final_address = Base58Encode(address_with_checksum)
```

---

## Summary

The full address generation pipeline is:

1. `Keccak256(public_key)`
2. Take last 20 bytes
3. Prepend prefix `0x43E552`
4. Double `Keccak256` hash
5. Append first 4 bytes as checksum
6. Encode using Base58

---

## Notes

- Public keys must be **exactly 64 bytes**
- The prefix `0x43E552` identifies the Abey address space
- Base58 encoding avoids ambiguous characters (`0`, `O`, `I`, `l`)
- Any change to the address bytes will invalidate the checksum
