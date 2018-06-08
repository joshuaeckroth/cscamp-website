---
title: Day 5 Notes
layout: note
---

# Day 5 Notes

## PGP



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






