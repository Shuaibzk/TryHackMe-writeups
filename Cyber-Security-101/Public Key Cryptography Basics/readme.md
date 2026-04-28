# Public Key Cryptography Basics (TryHackMe)

## 🧠 What is Public Key Cryptography?

Public key cryptography, also called asymmetric cryptography, uses two mathematically related keys:

- **Public key** → can be shared openly
- **Private key** → must be kept secret

The public key is commonly used for encryption or verification, while the private key is used for decryption or signing.

---

## 🔐 Why Public Key Cryptography Matters

Public key cryptography helps provide:

- Secure communication over insecure channels
- Authentication of servers and users
- Digital signatures
- Secure key exchange
- SSH key-based login

---

## 🔢 RSA Overview

RSA is a public-key cryptography algorithm based on the difficulty of factoring very large numbers.

### Key Idea

Multiplying two large prime numbers is easy, but factoring their product back into the original primes is extremely difficult when the numbers are large enough.

---

## 🧮 Main RSA Variables

| Variable | Meaning |
|---|---|
| `p` | First large prime number |
| `q` | Second large prime number |
| `n` | Product of `p × q` |
| `e` | Public exponent |
| `d` | Private exponent |
| `m` | Plaintext message |
| `c` | Ciphertext message |

---

## 🔑 RSA Keys

### Public Key

(n, e)

Used for encryption or verification.

### Private Key

(n, d)

Used for decryption or signing.

---

## 🧩 RSA Process in Simple Terms

### Key Generation

1. Choose two large prime numbers: `p` and `q`
2. Calculate `n = p × q`
3. Calculate Euler’s totient value
4. Choose a public exponent `e`
5. Calculate the private exponent `d`

---

### Encryption

c = m^e mod n

The sender encrypts the plaintext message using the receiver’s public key.

---

### Decryption

m = c^d mod n

The receiver decrypts the ciphertext using their private key.

---

## 💡 RSA Security Insight

RSA is secure because an attacker may know `n` and `e`, but calculating `d` requires factoring `n` into `p` and `q`.

With real-world key sizes, this factoring process is computationally infeasible using normal classical computers.

---

## 🏁 RSA in CTFs

In CTF-style cryptography challenges, RSA problems often provide some RSA variables and ask the learner to recover the plaintext.

### Common CTF Variables

| Variable | Meaning |
|---|---|
| `p`, `q` | Prime factors |
| `n` | Modulus |
| `e` | Public exponent |
| `d` | Private exponent |
| `m` | Plaintext |
| `c` | Ciphertext |

### Key Insight

If weak values, small primes, reused parameters, or exposed variables are given, RSA may become breakable in a challenge environment.

---

## 🔄 Diffie-Hellman Key Exchange

Diffie-Hellman is a key exchange method that allows two parties to create a shared secret over an insecure channel.

The shared secret can later be used for symmetric encryption.

---

## 🧮 Diffie-Hellman Variables

| Variable | Meaning |
|---|---|
| `p` | Public large prime number |
| `g` | Public generator |
| `a` | Alice’s private value |
| `b` | Bob’s private value |
| `A` | Alice’s public value |
| `B` | Bob’s public value |
| Shared Secret | Final secret calculated by both parties |

---

## 🔁 Diffie-Hellman Process

1. Alice and Bob agree on public values `p` and `g`
2. Alice chooses private value `a`
3. Bob chooses private value `b`
4. Alice calculates her public value
5. Bob calculates his public value
6. They exchange public values
7. Each side combines the received public value with their own private value
8. Both calculate the same shared secret

---

## 💡 Diffie-Hellman Key Insight

Diffie-Hellman allows secure key agreement, but it does not automatically prove identity.

For real-world security, it is usually combined with authentication methods such as digital signatures or certificates.

---

## 🖥️ SSH and Public Key Cryptography

SSH uses public key cryptography for:

- Authenticating the server
- Authenticating the client
- Establishing secure communication

---

## 🛡️ Authenticating the Server

When connecting to a new SSH server, the client may show a fingerprint warning.

This means the SSH client has not seen that server key before.

### Key Insight

The user should verify the server fingerprint before trusting it.

Once accepted, the fingerprint is stored in the known hosts file, and future connections are checked against it.

---

## 👤 Authenticating the Client

SSH can authenticate users using:

- Username and password
- Public/private key pair

Key-based authentication is often more secure than password-only login when keys are protected properly.

---

## 🔧 Generating SSH Keys

### Example Command

ssh-keygen -t ed25519

This generates:

- A private key
- A public key

---

## 📁 Common SSH Key Files

| File | Purpose |
|---|---|
| `id_ed25519` | Private key |
| `id_ed25519.pub` | Public key |
| `known_hosts` | Stores trusted server fingerprints |
| `authorized_keys` | Stores public keys allowed to log in |

---

## 🔒 SSH Private Key Safety

A private SSH key should be treated like a password.

### Good Practices

- Never share the private key
- Use a strong passphrase when possible
- Keep correct file permissions
- Store the key only on trusted devices
- Share only the public key

---

## 🧾 Private Key Permissions

Private keys should be readable only by the owner.

### Common Permission

chmod 600 private_key_file

### Using a Specific Private Key

ssh -i private_key_file user@host

---

## 🧠 Important Clarification

The private key passphrase does not authenticate the user to the server directly.

It only decrypts the private key locally on the user’s machine.

The passphrase is not sent to the server.

---

## 🛠️ Tools and Commands Mentioned

| Tool / Command | Purpose |
|---|---|
| `ssh` | Connect to remote systems securely |
| `ssh-keygen` | Generate SSH key pairs |
| `ssh-copy-id` | Copy public key to a server |
| `chmod` | Set file permissions |
| `known_hosts` | Stores trusted server identities |
| `authorized_keys` | Stores allowed public keys |

---

## 💡 What I Learned

- RSA depends on the difficulty of factoring large numbers
- Public keys can be shared, but private keys must remain secret
- Diffie-Hellman helps two parties agree on a shared secret
- SSH uses public key cryptography for authentication
- Server fingerprints help detect possible impersonation
- Private SSH keys must be protected carefully
- Key-based authentication is safer than password-only authentication when configured correctly

---

## ⚠️ Note

This writeup focuses on cryptography concepts, SSH authentication, and defensive understanding only.

Specific challenge answers, flags, private keys, and target-specific solutions are intentionally excluded.
