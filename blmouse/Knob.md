[TOC]

# Knob Attack

Pairing is a mechanism to share a long term secret key with the ble device
Protocol used is  secure simple Pairing / secure simple connection
may use challenge response mechanism or DH

If encryption is required

a session key is generated using 

Attack is to compute the encryption key not perform Pairing

capable of 

- decrypt packets 
- inject packets


## Generation of session key for encryption

Encryption is generate with 16 bytes -> Reduction function to reduce entropy -> passed to encryption function AES-CCM

In the reduction function phase
There is a entropy negotiation between Alice and Bob which allows Eve/Mallory to snif and inject a low byte entropy as low as 1 byte

Points to note

- Negotiation is not authenticated
- Inject any level of entropy byte (Reduce Key size to perform bruteforce later)
- bruteforce in realtime

## Stages of Knob Attack

### Phase 1 - Setup Pairing

Alice and Bob perform ssp to establish long term key

### Phase 2 - Setup encrypted session for transmission

- Alice and Bob try to setup a new encrypted session
- they come to the entropy negotiation stage
- Eve injects the entropy to be one byte
- Eve sniffs ciphertext
- Bruteforces cipher text with enc key of 1 byte which (8 bits pow 2 = 256 possibilities)
- finds encryption key
- decrypt all subseqeunt connections
- also capable of injecting packets (There is no signing in ble)


Sessions can be forced to be renegotitated -> deauth attacks and DOS