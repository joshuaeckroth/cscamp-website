---
title: Day 5 Notes
layout: note
---

# Day 5 Notes

## PGP

- encrypt, ensure only Alice can read it: encrypt with Alice’s public key
- decrypt, read something meant for you: decrypt with your private key
- sign, prove you wrote it: sign with your private key
- verify, prove a signature is value: verify with other’s public key
- encrypt & sign, prove you wrote it and ensure only Alice can read it: encrypt with Alice’s public key, then sign

Create gpg keys:

```
gpg --gen-key # RSA+DSA, 2048 key size, choose good password
```

Oops, not enough entropy; gpg will take a really long time to compute random keys due to insufficient entropy generation

```
watch cat /proc/sys/kernel/random/entropy_avail
```

“entropy”:
- need *really good* random numbers for cryptography use
- random numbers are actually pseudorandom numbers, computed from the prior number
- e.g., if an attacker knows a system is running on a vm, and knows the os generated keys (e.g., SSH keys, SSL keys, etc.) during an install process, the attacker could run the same install process on his own vm and possibly generate the same keys
- need to inject some unpredictable input from a different source

```
cat /proc/sys/kernel/random/entropy_avail
```

Probably very low.

```
cat /dev/random | hexdump -C
```

Run some job like `sudo apt-get update`, watch /dev/random spit out a bit more.

> Linux already gets very good quality random data from the aforementioned hardware sources, but since a headless machine usually has no keyboard or mouse, there is much less entropy generated. Disk and network I/O represent the majority of entropy generation sources for these machines, and these produce very sparse amounts of entropy. Since very few headless machines like servers or cloud servers/virtual machines have any sort of dedicated hardware RNG solution available, there exist several userland solutions to generate additional entropy using hardware interrupts from devices that are “noisier” than hard disks, like video cards, sound cards, etc. This once again proves to be an issue for servers unfortunately, as they do not commonly contain either one. Enter haveged. Based on the HAVEGE principle, and previously based on its associated library, haveged allows generating randomness based on variations in code execution time on a processor. Since it's nearly impossible for one piece of code to take the same exact time to execute, even in the same environment on the same hardware, the timing of running a single or multiple programs should be suitable to seed a random source. The haveged implementation seeds your system's random source (usually /dev/random) using differences in your processor's time stamp counter (TSC) after executing a loop repeatedly. Though this sounds like it should end up creating predictable data, you may be surprised to view the FIPS test results in the bottom of this article. [source]

> haveged  generates  an  unpredictable  stream  of  random numbers harvested from the indirect effects of hardware events on hidden processor state (caches, branch predictors, memory translation tables, etc) using the HAVEGE (HArdware Volatile Entropy Gathering and Expansion) algorithm. (man page)

```
sudo apt-get install haveged
```

Now check again:

```
cat /proc/sys/kernel/random/entropy_avail
```

Try creating key again:

```
gpg --gen-key # RSA+DSA, 2048 key size, choose good password
```


Look at your public key, put somewhere public:

```
gpg --armor --export [myemail or keyid]
```

Encrypt/decrypt, sign/check signature files

- encrypt: `gpg --recipient [user-id] --encrypt [file]`
    - decrypt on same machine fails, don’t have that secret key
- decrypt: `gpg --output [outfile] --decrypt [file]`
- add `--armor` for ASCII version
- sign (prove you wrote it): `gpg --clearsign --sign [file]`
- verify signature: `gpg --verify [file]`
    - does not warn about “detached file” if not using clearsign
- encrypt & sign: `gpg --encrypt --sign -r [other-person] [file]`

## SSL

- critical issue: not easy to share public keys ahead of time
- thus, we use a certificate service that is default-trusted by the browser

- web server has a private and public key
- company sends public key to a certificate authority that verifies your identity
- they use their certificate to sign your public key, you save the signed version

- when a browser visits your site, the browser gets the signed public key
  - the browser checks that the signature is valid against its root certificates

- future communication is encrypted (symmetric)
  - the password is generated as follows:
    - browser creates a random number, and encrypt it with server's public key, and send to them
      - only server can read that number
    - this number is then used by both sides to create a strong random password

## Tor

Use a proxy for the client:

### bbsclient-proxy.py

```
import socketfuncs
import socket
import sys
import socks # uses PySocks library

socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 9050)
socket.socket = socks.socksocket

domain = input("Domain: ")

s = socket.socket()
username = input("Username: ")
s.connect((domain, 10333))
msg = socketfuncs.receive_message(s)
print(msg)
if msg != "BBS Server 1.2":
	print("Wrong protocol!")
	sys.exit()
socketfuncs.send_message(s, username)
while True:
	msg = input("Message: ")
	socketfuncs.send_message(s, msg)
	if msg == "quit":
		break
	elif msg == "dm":
		msgto = input("To? ")
		msg = input("Message (DM): ")
		socketfuncs.send_message(s, msgto)
		socketfuncs.send_message(s, msg)
	elif msg == "list" or msg == "listdm" or msg == "listdms":
		msgcount = int(socketfuncs.receive_message(s))
		for i in range(msgcount):
			m = socketfuncs.receive_message(s)
			print(m)
s.close()
```

### .onion domains

Edit `/etc/tor/torrc` to have these lines:

```
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 11333 127.0.0.1:10333
```

Restart tor, then find the onion address in `/var/lib/tor/hidden_service/hostname`, e.g.,

```
5qliq3izxfymwy6o.onion
```

Use that for the "domain" when connecting to BBS server with the client. Use port 11333 in the client.


## Overview of topics






