🔐 ANSI X9.19 — Retail MAC Algorithm

«A technical introduction to the ANSI X9.19 Retail Message Authentication Code (MAC) algorithm, its underlying DES operations, and its application in payment systems such as ISO 8583.»

---

📌 1. Introduction

What is a MAC?

MAC (Message Authentication Code) is a cryptographic value used to verify the integrity and authenticity of a message.

For example, consider a payment request sent through the ISO 8583 protocol.

Many ISO 8583 fields contain ordinary data that can be read by anyone who intercepts the message. An attacker could potentially modify these fields or construct a fake request and send it to the server.

How can the server determine whether a received message is authentic and has not been modified?

This is where a MAC comes into play.

A MAC is calculated using two main inputs:

- Message: The data that needs to be authenticated.
- Secret Key: A cryptographic key shared between the communicating parties.

The sender calculates a MAC and attaches it to the message. The receiver independently calculates the expected MAC using the received message and its secret key.

If both MAC values match, the message passes the MAC verification.

🔑 Why not use a simple predefined code?

You might wonder:

«If the server accepts requests containing a specific code, can't an attacker simply observe that code and reuse it?»

Exactly! That is why we don't use fixed or predictable codes.

Instead, the MAC is dynamically calculated using cryptographic operations and a secret key.

Even if an attacker can observe the message and its MAC, knowing the MAC value does not reveal the secret key.

Important: A MAC alone does not prevent replay attacks. Additional mechanisms, such as transaction counters, timestamps, or unique transaction identifiers, may be necessary.

---

⚙️ 2. Quick Overview

ANSI X9.19 Retail MAC uses DES encryption in a CBC-like chaining process, followed by a final decryption and encryption operation.

Algorithm steps

1. Inputs: Obtain the request message, two DES key components ("K1" and "K2"), and an initial chaining value (IV).

2. Divide the message: Split the message into blocks of 8 bytes (64 bits).

3. Padding: If necessary, pad the final block according to the padding method agreed upon by both parties.

4. Initialize chaining: Set the initial chaining value to the provided IV. Its size must be 8 bytes.

5. XOR operation: XOR each message block with the preceding chaining value.

6. DES encryption: Encrypt the XOR result using the first key ("K1").

7. Repeat: Use the encryption output as the next chaining value and repeat the process until all message blocks have been processed.

8. Final decryption: Decrypt the final chaining value using the second key ("K2").

9. Final encryption: Encrypt the decryption result using the first key ("K1") again.

10. MAC output: The resulting 8-byte (64-bit) value is the Retail MAC.

---

🧮 3. Mathematical Representation

Let's define the chaining operation mathematically.

Notation

Symbol| Description
(M_i)| The (i)-th message block (8 bytes)
(H_i)| The chaining value after processing block (i)
(H_0)| Initial chaining value (IV)
(K_1)| First DES key component
(K_2)| Second DES key component
(\oplus)| XOR operation
(E_K(X))| DES encryption of (X) using key (K)
(D_K(X))| DES decryption of (X) using key (K)

Step 1 — CBC-like chaining

Initialize:

[
H_0 = IV
]

For each message block:

[
\boxed{H_i = E_{K_1}(M_i \oplus H_{i-1})}
]

Repeat until the last message block has been processed.

The result is:

[
H_n = E_{K_1}(M_n \oplus H_{n-1})
]

Step 2 — Final transformation

After processing all message blocks, perform the following operations:

1. Decrypt using K2:

[
T = D_{K_2}(H_n)
]

2. Encrypt using K1:

[
\boxed{MAC = E_{K_1}(T)}
]

Therefore, the complete final operation is:

[
\boxed{MAC = E_{K_1}(D_{K_2}(H_n))}
]

The final result is an 8-byte MAC value.

---

🔑 4. Key Structure

ANSI X9.19 uses two DES key components:

Key| Description| Size
(K_1)| First DES key component| 8 bytes
(K_2)| Second DES key component| 8 bytes
Combined Key| Both components together| 16 bytes

The commonly used two-key format is:

[
K = K_1 \parallel K_2
]

Where (\parallel) represents concatenation.

Note: Each DES key component is 64 bits in storage, including 8 parity bits, leaving 56 effective key bits.
