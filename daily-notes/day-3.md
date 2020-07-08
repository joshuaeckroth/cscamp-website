---
title: Day 3 Notes
layout: note
---

# Day 3 Notes

```
jbarto          100.25.117.93
nbradberry      54.162.223.29
kclark          54.237.207.138
kcottrell       52.91.148.120
mdavis          52.3.240.52
aendicott       100.26.253.25
mgarcia         34.201.68.57
jax             35.175.134.171
rkuiper         54.157.175.29
eluedeke        100.25.19.163
amiller         34.232.52.172
lsharkey        54.146.80.134
josh            18.234.145.155
```

---

```
- Goal: use names of machines instead of IP addresses
- Restriction: don't ask everyone to buy new domains
- Technique:
  - Update client only: needs to look up the IP of the server for a given name
  - How? Where is the information for this lookup maintained?
  - Let's make a single DNS server that is definitive for this camp
    - Write a new file (new server file, sockets, etc.)
    - This DNS server will listen for a connection;
      then receive message with the name (the query);
      lookup the answer (the IP address), and send it back;
      go back to listening
  - Client: before attempting to connect to chat server, ask DNS server
    to give us the IP address for a name (typed by the user)
- Extra goal: reverse dns?
```

---

Notes about DNS:

DNS = domain name system

Issue: IP addresses are a pain, maybe nobody would use the internet if that's
all we had?

Solution: some sort of global dictionary/phonebook

Possible answers:

1. There's just one dictionary, under a certain IP address
   - Problem: that one address gets lots of load
   - Solution: when a machine gets the definitive answer, it can "cache it"
2. Everybody has their own "local" copy
   - Problem: they can get out of date
   - Solution: tell everyone about changes?


Actual DNS solution:

- hierarchy of definitiveness:
  - if nobody local knows the answer ("what IP goes with this domain?"),
    then ask someone up the chain
  - need to know the hierarchy (IP addresses)
  - save a copy of information I obtain from up the chain into a local database
    - this saves time for future requests
  - every so often, my copy should go out of date and force me to ask again
  - the root DNS servers are the final answer
    - in reality, they have don't have the whole database;
      much is delegated to level-1 or lower levels
- this hierarchy avoids the extreme traffic at the root, while still ensuring
  (more or less, not real-time) that everyone's copy is up to date

---

Symmetric encryption demo in Python:

```
import encfuncs

password = encfuncs.get_password()
print(password)

msg = input("Enter message: ")
print("Message: " + msg)

msg_enc = encfuncs.encrypt(msg, password)
print("Encrypted message: " + msg_enc)

password = encfuncs.get_password()
print(password)

orig_msg = encfuncs.decrypt(msg_enc, password)
print("Decrypted message: " + orig_msg)
```

---

Notes about encryption

- Symmetric encryption:
  - You have a key (password) that is used to encrypt the data
  - Only this key can decrypt the data
  - Issue: how to pre-share the key? (how does the other side know the key?)

- Asymmetric encryption:
  - One key is used for encryption, a different key is used for decryption
  - Each person/machine/program will generally created two keys:
    - Key A, Key B -- these always go together
      - Impossible to guess B if you only have A, and vice versa
    - Logic: if you encrypt with key A, you must decrypt with key B;
      and vice versa
  - Call one key public and one key private
    - Public one is shared as widely as possible (a billboard is good)
      - It's associated with your name/machine
    - Private key should never be shared
      - It might even have a password to protect it
  - Result:
    - If you encrypt a file with your private key, only your public key
      can decrypt it
      - This proves you encrypted the file (and no one else did)
      - It's like a signature
      - Everyone can read the file (everyone has your public key),
        but they can verify you created the file
    - If you encrypt a file with Alice's public key, only Alice's private key
      can decrypt that file
      - This ensures only Alice can read your file that you generated
    - Do both: Bob wants to send Alice a message (secretly), and prove that
      he sent it
      - Bob encrypts the file with Alice's public key
      - Bob then signs the file with his private key
      - sends to Alice...
      - Alice verifies Bob's signature with his public key
      - then Alice decrypts the file with her private key
  - Tools for asymmetric:
    - PGP = Pretty Good Privacy
    - gpg program on linux
    - email: SMIME 

- GPG commands
  - Generate a key: gpg --gen-key
  - Export your public key for uploading online: gpg --armor --export
    - Then go to https://keyserver.ubuntu.com/ to upload
  - Receive a public key: gpg --keyserver keyserver.ubuntu.com --recv-key user-id
  - Encrypt a file for someone: gpg --recipient user-id --encrypt file
  - Decrypt a file: gpg --output decryptedfile.txt --decrypt file
  - Sign a file (prove it's from you): gpg --clearsign --sign file
  - Verify a signature (prove it's from them): gpg --verify file
  - Encrypt & sign: gpg --sign --recipient user-id --encrypt file
```

Afternoon tasks:

- DNS server:
  - Continue working on DNS server
- Chat server:
  - Encrypt all traffic with symmetric encryption
- GPG, asymmetric encryption (see commands above):
  - generate a key
  - publish that key on the key server
  - receive my public key: user id is 0e937ba99960ce0068947456c148b0e10386531b
  - create a text file with some simple contents
  - sign and encrypt that file
  - I will then copy it this afternoon and verify the signature and decrypt it

