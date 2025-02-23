## CIA Triad

- Confidentiality  
  - Data is only accessible to authorized users  
- Integrity  
  - Data is only modified by authorized users  
- Availability  
  - Service is accessible by authorized users  
- Are they equally important?  
  - Situational

### Three extra bits

- Authentication  
  - Someone is who they say they are  
- Non-repudiation  
  - Prove the integrity and origin of data  
- Code validation  
  - Assure that code is checked for vulnerabilities

## OSI Security Architecture

- Security attack  
  - Any action that compromises the security of information owned by an organisation  
- Security mechanism  
  - A process that is designed to detect / prevent / recover from a security attack  
    - Encryption  
    - Digital signature  
    - Traffic padding  
- Security service  
  - A service that ensures the safety and security of a system …?
## 5Ds of Security

- Deter  
- Detect  
- Deny  
- Delay  
- Defend

## Security Mechanisms

### Cryptographic techniques

- Basis for security services and mechanisms like de/encipherment, authentication exchange, password storage and checking, etc.  
- Two variables:  
  - Key (sym or asym)  
  - Initial vector  
    - Randomisation effect  
    - Prevents replay attacks

Attacks against cryptographic techniques/protocols

| Attack | Description | Countermeasure |
| ----- | ----- | ----- |
| Cryptoanalysis | Reverse engineering a plaintext | Don’t use weak keys, and make existing keys longer |
| Message/field insertion, deletion and modification | \<self-explanatory\> | Integrity checking (hashing, Message Authentication Code, digital signature)  |
| Replay  | Resending a previously valid ciphertext | Freshness in protocol run Timestamp, sequence number, nonce |
| Man in the middle | Intercepting stuff | Well designed crypto protocols Use formal verification |

### 

### Digital Signature

- Attaching a redundant bit to transmitted data to allow the recipient to verify the sender and the integrity of the data  
- Provides:  
  - Authentication  
  - Integrity  
  - Non-repudiation

### Authentication Exchange

- Sender and receiver negotiate to ensure the identity of each other  
- General means of authentication  
  - What you know (password)  
  - What you are (biometrics)  
  - What you have (passport, phone)  
  - 2FA / MFA

#### Choosing means of authentication

- If peer entities and communication means are trusted  
  - Use password  
- If peer entities but not communication means are trusted  
  - Use combination of password and cryptographic protocols  
- If neither are trusted  
  - Use above \+ non-repudiation mechanisms

### Other security mechanisms

- Access control  
  - Limiting access to a resource to users who are authorised  
- Traffic padding  
  - Inserting bits into gaps in a data stream to obfuscate traffic analysis  
- Notarisation  
  - Use a trusted third party to assure security of data exchange  
- Security audit trail  
  - Monitor and trace  
- Security recovery  
  - ??  
- Economics  
  - Make it cost-ineffective to attack  
    - E.g. if it costs to send spam emails, makes it harder for spammers  
- Deception  
  - Getting adversaries to reveal themselves  
  - Honeypot  
    - Leaving a server intentionally vulnerable to attack in order to gather information about the attacker

## Security attacks

### Jamming

- Attacks availability  
- Block up (jam) the transmission channel  
- Make service unavailable

### Sniffing

- Attacks confidentiality  
- Eavesdropping on network conversations  
- Capture packets for replay, MITM and packet injection attacks

### Spoofing

- Attacks integrity  
- Pretending to be someone they’re not  
- Examples  
  - Email spoofing (e.g. scam emails pretending to be from your organisation)  
  - Text message spoofing  
  - Call ID spoofing  
  - URL spoofing  
  - DNS spoofing  
  - IP address spoofing  
    - Changing the source addr in an IP packet  
  - MAC spoofing  
    - Changing the MAC address of an NIC

[[2 - TCP IP, sniffing and traffic analysis]]