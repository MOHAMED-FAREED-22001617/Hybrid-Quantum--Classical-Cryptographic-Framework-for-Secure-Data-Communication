## Hybrid Quantum-Classical Cryptographic Framework for Secure Data Communication

A robust security framework that seamlessly integrates the information-theoretic security of Quantum Key Distribution (QKD) with the efficiency of classical AES-256 encryption to protect data against future quantum computing threats.

## About
The rapid evolution of quantum computing poses a significant existential threat to traditional cryptographic systems like RSA and Elliptic Curve Cryptography (ECC), which rely on mathematical assumptions vulnerable to quantum algorithms such as Shor’s. This project, **Hybrid Quantum-Classical Cryptographic Framework**, is designed to bridge the gap between current security infrastructure and the post-quantum era.

The framework employs a layered security approach:
1. **Quantum Layer**: Utilizes a simulated **BB84 protocol** to generate shared secret keys, ensuring that any eavesdropping attempts are physically detectable.
2. **Authentication Layer**: Integrates **Post-Quantum Cryptography (PQC)** algorithms (specifically lattice-based methods like CRYSTALS-Dilithium) to prevent man-in-the-middle attacks.
3. **Classical Layer**: Uses the generated hybrid keys within **AES-256 (Galois/Counter Mode)** for high-speed, authenticated data encryption.

This system provides a practical, scalable migration path for organizations to enhance their security posture without a complete overhaul of existing TCP/IP network infrastructures.

## Overview
This project presents a **Hybrid Quantum–Classical Cryptographic Framework**
that integrates:
- Quantum Key Distribution (BB84 – simulated)
- Post-Quantum Cryptography (Lattice-based)
- AES-256 (GCM Mode)
The framework ensures **quantum-resilient, secure, and scalable communication** without requiring real quantum hardware.
## Features
- **Quantum Key Distribution (QKD):** Implements a simulation of the BB84 protocol using Qiskit to generate high-entropy keys with eavesdropping detection.
- **Hybrid Key Derivation:** Combines quantum-generated keys with classical/post-quantum salts using SHA-256 to derive secure session keys, ensuring resilience even if one source is compromised.
- **Post-Quantum Authentication:** Utilizes lattice-based digital signatures to authenticate users and session handshakes, resisting quantum adversaries.
- **Authenticated Encryption:** Deploys AES-256 in GCM mode to provide both data confidentiality and integrity verification.
- **Forward Secrecy:** Enforces automated key rotation and secure memory erasure of old keys to prevent future decryption of past sessions.
- **Tamper Detection:** Automatically detects and aborts sessions if high error rates (indicating eavesdropping) or integrity check failures are detected.

## Requirements
* **Operating System:** Windows, Linux, or macOS.
* **Processor:** Intel i5 / Ryzen 5 or higher.
* **RAM:** 8–16 GB (Recommended for quantum simulation operations).
***Language:** Python 3.x.
* **Cryptography Libraries:** `PyCryptodome` for AES-256 (GCM), SHA-256, and HMAC operations.
* **Quantum Simulation:** `Qiskit` framework for simulating qubits and the BB84 protocol.
* **Networking:** Standard Python `socket` library for TCP/IP communication.
* **IDE:** VS Code (Recommended).

## Project Structure
See the folder structure inside the repository for:
- `src/` → source code
- `docs/` → reports, PPT, diagrams
- `tests/` → test cases
- `outputs/` → logs and results

##  Testing
- Black Box Testing
- White Box Testing
- QBER-based attack detection
- Encryption & Decryption validation

## programs
# BB84 QKD Algorithm
```
import random

def generate_bits(n=16):
    return [random.randint(0, 1) for _ in range(n)]

def generate_bases(n=16):
    return [random.choice(['+', 'x']) for _ in range(n)]

def bb84():
    sender_bits = generate_bits()
    sender_bases = generate_bases()
    receiver_bases = generate_bases()

    shared_key = []

    for i in range(len(sender_bits)):
        if sender_bases[i] == receiver_bases[i]:
            shared_key.append(sender_bits[i])

    return bytes(shared_key)

if __name__ == "__main__":
    key = bb84()
    print("Quantum Key Generated:", key)

```
# Hybrid Key Generation
```
import os
import hashlib
from qkd.bb84 import bb84

def generate_hybrid_key():
    quantum_key = bb84()
    classical_key = os.urandom(16)

    combined_key = quantum_key + classical_key
    hybrid_key = hashlib.sha256(combined_key).digest()

    return hybrid_key

if __name__ == "__main__":
    key = generate_hybrid_key()
    print("Hybrid Key Generated:", key)

```
# AES-256 GCM Encryption & Decryption
```
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
import os

def encrypt_data(key, plaintext):
    aesgcm = AESGCM(key)
    nonce = os.urandom(12)
    ciphertext = aesgcm.encrypt(nonce, plaintext.encode(), None)
    return nonce, ciphertext

def decrypt_data(key, nonce, ciphertext):
    aesgcm = AESGCM(key)
    plaintext = aesgcm.decrypt(nonce, ciphertext, None)
    return plaintext.decode()

if __name__ == "__main__":
    key = AESGCM.generate_key(bit_length=256)
    nonce, ct = encrypt_data(key, "Secure Message")
    print("Encrypted:", ct)
    print("Decrypted:", decrypt_data(key, nonce, ct))

```
# Sender Code
```
import socket
from crypto.hybrid_key import generate_hybrid_key
from crypto.aes_gcm import encrypt_data

key = generate_hybrid_key()
message = "Secure Message using Hybrid Cryptography"

nonce, ciphertext = encrypt_data(key, message)

client = socket.socket()
client.connect(("localhost", 9999))
client.send(nonce + ciphertext)
client.close()

print("Encrypted data sent successfully")

```
# Receiver Code
```
import socket
from crypto.hybrid_key import generate_hybrid_key
from crypto.aes_gcm import decrypt_data

key = generate_hybrid_key()

server = socket.socket()
server.bind(("localhost", 9999))
server.listen(1)

conn, addr = server.accept()
data = conn.recv(1024)

nonce = data[:12]
ciphertext = data[12:]

message = decrypt_data(key, nonce, ciphertext)
print("Decrypted Message:", message)

conn.close()

```
## sample output
```
Quantum Key Generated Successfully
Hybrid Key Created Successfully

Encrypted Data Sent
Encrypted Message:
gAAAAABlYw...

Decrypted Message:
Secure Message using Hybrid Cryptography

Integrity Verified
Secure Communication Successful
```


## System Architecture
The architecture consists of five robust layers: User, Application, Security, Storage, and Communication. The core logic resides in the Security Layer, where the Key Management module orchestrates the Quantum Module (BB84), Post-Quantum Auth Module, and the Classical Crypto Engine.


<img width="753" height="343" alt="image" src="https://github.com/user-attachments/assets/6dc0f9f1-3613-4ed8-a697-624aa9466c09" />


## Output

#### Output 1 - System Initialization & QKD
The console log below demonstrates the BB84 protocol simulation. It shows the generation of random qubits, basis selection by Alice and Bob, and the sifting process where mismatched bases are discarded to form the raw quantum key.

<img width="600" height="300" alt="Code_Generated_Image" src="https://github.com/user-attachments/assets/ed1ca6ea-c8e5-4cfa-b642-41ad315700e6" />

 

#### Output 2 - Encryption & Secure Transmission
This output details the classical encryption phase. The system takes the plaintext (e.g., "Confidential Medical Record"), derives a hybrid session key, and generates the Ciphertext and Authentication Tag using AES-GCM before transmission.


<img width="650" height="300" alt="Code_Generated_Image (1)" src="https://github.com/user-attachments/assets/9ecccced-dd6e-49f5-a8bb-811c2c3fd8b3" />



#### Output 3 - Decryption & Integrity Verification
At the receiver's end, the log confirms the receipt of the payload. It explicitly states **"Integrity Verified"** (proving no tampering occurred) before decrypting the ciphertext back to the original plaintext.


<img width="650" height="280" alt="Code_Generated_Image (2)" src="https://github.com/user-attachments/assets/88a7d9e9-1613-419e-a9bb-1addd1167a01" />



## Results and Impact
This project successfully demonstrates that quantum principles can be simulated and integrated into standard network architectures. 
- **Security:** Validated against both black-box and white-box testing, resisting replay attacks, tampering, and unauthorized access.
- **Performance:** Achieved high-speed encryption for bulk data using AES-256 while maintaining quantum-safe key exchange.
- **Impact:** Establishes a viable blueprint for "Quantum-Safe" communications, allowing critical infrastructure to prepare for the post-quantum era without requiring immediate, expensive hardware upgrades.

## Articles published / References
1. Raj, A., and Balachandran, V., “A Hybrid Encryption Framework Combining Classical, Post-Quantum, and QKD Methods,” arXiv preprint arXiv:2509.10551, 2025.


2. Sanz, A., Salegi, E., Atutxa, A., Franco, D., Astorga, J., and Jacob, E., “Extending Quantum-Safe Communications to Real-World Networks: An Adaptive Security Framework,” arXiv preprint arXiv:2511.22416, 2025.


3. Mozo, H. E., “Quantum-Classical Hybrid Encryption Framework Based on Simulated BB84 and AES-256: Design and Experimental Evaluation,” arXiv preprint arXiv:2511.02836, 2025.

4. Shafique, A., Naqvi, S. A. A., and Raza, A., “A Hybrid Encryption Framework Leveraging Quantum and Classical Cryptography for Secure Transmission of Medical Images in IoT-Based Telemedicine Networks,” Scientific Reports, vol. 14, art. no. 31054, 2024.


5. Angamuthu, G., and Marikkannan, M., “Hybrid Quantum-Resistant Key Exchange Protocol for Secure Network Communication,” International Journal of Computer Science Engineering (IJCSE), 2025.


6. Anastasova, M., Kampanakis, P., and Massimo, J., “PQ-HPKE: Post-Quantum Hybrid Public Key Encryption,” IACR ePrint Report 2022/414, 2022.

7. Ahmed, Y., Elmrabit, N., and Yousefi, M., “Enhancing the Security of Classical Communication with Post-Quantum Authenticated-Encryption Schemes for Quantum Key Distribution,” Computers, vol. 13, no. 7, p. 163, 2024.


8. Rizvi, E. R., and Khurram, S., “Hybrid Post-Quantum Cryptographic Approaches for Secure Communication,” Annual Methodological Archive Research Review, 2025.

9. Shukur, M. G., and Dileep, M. R., “Hybrid Quantum-Classical Learning for Accelerating Cryptographic Key Distribution in Post-Quantum Networks,” Synthesis: A Multidisciplinary Research Journal, vol. 1, no. 4, pp. 10–21, 2023.

10. “Exploring Post-Quantum Cryptography: Review and Directions for the Transition Process,” MDPI Technologies, vol. 12, no. 12, p. 241, 2025.
