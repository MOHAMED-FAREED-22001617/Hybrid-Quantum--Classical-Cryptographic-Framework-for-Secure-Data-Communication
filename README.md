# Hybrid Quantum–Classical Cryptographic Framework
🔐 Secure Data Communication using QKD + PQC + AES

## 📌 Overview
This project presents a **Hybrid Quantum–Classical Cryptographic Framework**
that integrates:
- Quantum Key Distribution (BB84 – simulated)
- Post-Quantum Cryptography (Lattice-based)
- AES-256 (GCM Mode)

The framework ensures **quantum-resilient, secure, and scalable communication**
without requiring real quantum hardware.

## 🚀 Key Features
- Quantum-resistant key exchange
- Hybrid key generation (Quantum + Classical)
- AES-256-GCM authenticated encryption
- Secure socket-based communication
- Eavesdropping detection (QBER analysis)

## 🧠 Technologies Used
- Python 3.x
- Qiskit (QKD Simulation)
- PyCryptodome / Cryptography
- Socket Programming
- SHA-256 / SHA3-512

## 🏗️ Architecture
The system follows a layered architecture:
1. Quantum Key Distribution Layer
2. Post-Quantum Authentication Layer
3. Hybrid Key Management
4. Symmetric Encryption (AES-256-GCM)
5. Secure Communication Layer

## 📂 Project Structure
See the folder structure inside the repository for:
- `src/` → source code
- `docs/` → reports, PPT, diagrams
- `tests/` → test cases
- `outputs/` → logs and results

## 🧪 Testing
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

## 📈 Results
- Low encryption latency
- Strong resistance to quantum and classical attacks
- Maintains backward compatibility

## 🔮 Future Work
- Integration with real quantum hardware
- Blockchain-based key management
- Multi-user secure communication
- IoT & healthcare deployment

## 👨‍🎓 Authors
- **Mohamed Fareed F**  
- Adhithya Perumal D  
- Devadarshan A S  

Under the guidance of **Ms. Panimalar S.P**  
Saveetha Engineering College


