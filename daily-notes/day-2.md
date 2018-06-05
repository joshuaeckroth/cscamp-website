---
title: Day 2 Notes
layout: note
---

# Day 2 Notes

## TCP/IP notes

Data are sent as packets.

- TCP: handles the packets: deals with out-of-order, dropped packets, slow packets, etc.
  - also handles ports
- IP: where the packets go (IP addresses), routing
  - each machine in the network keeps a "routing table" that says where to send packets based on their IP address
  - the tables have to be dynamic as machines appear and disappear


## Python code

### socketfuncs.py

```
# goal: create a library that has functions for sending/receiving messages

# provide connection object 'conn'
def receive_message(conn):

    # get message size (sent by client)
    msgsize = conn.recv(4)
    msgsize = int(msgsize.decode())

    # receive whole message, possibly in chunks (packets)
    chunks = []
    bytes_received = 0
    while bytes_received < msgsize:
        chunk = conn.recv(min(msgsize - bytes_received, 2048))
        chunks.append(chunk)
        bytes_received += len(chunk)
    msg = b''.join(chunks).decode()

    return msg

# provide socket 's' and message 'msg'
def send_message(s, msg):

    msgsize = len(msg)

    # pad size with zeros if less than 4 digits
    s.send(str.encode("{:0>4d}".format(msgsize)))

    # send message in chunks (packets)
    totalsent = 0
    while totalsent < msgsize:
        sent = s.send(msg[totalsent:].encode())
        totalsent = totalsent + sent
```

### simpleserver.py

```
import socketfuncs
import socket

s = socket.socket()
s.bind(('0.0.0.0', 10333))

while True:
	s.listen(10)
	(conn, address) = s.accept()
	print("Got connection from {}".format(address))
	msg = socketfuncs.receive_message(conn)
	print("Message: {}".format(msg))

s.close()
```

### simpleclient.py

```
import socketfuncs
import socket

s = socket.socket()
ip = input("IP: ")
s.connect((ip, 10333))
msg = input("Message: ")
socketfuncs.send_message(s, msg)
s.close()
```

### bbsserver.py

```
import socketfuncs
import socket

s = socket.socket()
s.bind(('0.0.0.0', 10333))

msghistory = []
while True:
	s.listen(10)
	(conn, address) = s.accept()
	print("Got connection from {}".format(address))
	socketfuncs.send_message(conn, "Welcome")
	while True:
		msg = socketfuncs.receive_message(conn)
		print("Message: {} {}".format(address[0], msg))
		if msg == "quit":
			break
		elif msg == "list":
			socketfuncs.send_message(conn, str(len(msghistory)))
			for m in msghistory:
				socketfuncs.send_message(conn, m)
		else:
			msghistory.append("{} {}".format(address[0], msg))

s.close()
```

### bbsclient.py

```
import socketfuncs
import socket

s = socket.socket()
ip = input("IP: ")
s.connect((ip, 10333))
msg = socketfuncs.receive_message(s)
print(msg)
while True:
	msg = input("Message: ")
	socketfuncs.send_message(s, msg)
	if msg == "quit":
		break
	elif msg == "list":
		msgcount = int(socketfuncs.receive_message(s))
		for i in range(msgcount):
			m = socketfuncs.receive_message(s)
			print(m)
s.close()
```

### bbsclient-v1.1.py

```
import socketfuncs
import socket

s = socket.socket()
ip = input("IP: ")
username = input("Username: ")
s.connect((ip, 10333))
msg = socketfuncs.receive_message(s)
print(msg)
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
	elif msg == "list":
		msgcount = int(socketfuncs.receive_message(s))
		for i in range(msgcount):
			m = socketfuncs.receive_message(s)
			print(m)
s.close()
```

### bbsserver-v1.1.py

```
import socketfuncs
import socket

s = socket.socket()
s.bind(('0.0.0.0', 10333))

msghistory = []
while True:
	s.listen(10)
	(conn, address) = s.accept()
	print("Got connection from {}".format(address))
	socketfuncs.send_message(conn, "BBS Server 1.1 (jeckroth)")
	username = socketfuncs.receive_message(conn)
	while True:
		msg = socketfuncs.receive_message(conn)
		print("Message: {} {}".format(address[0], msg))
		if msg == "quit":
			break
		elif msg == "dm":
			msgto = socketfuncs.receive_message(conn)
			msg = socketfuncs.receive_message(conn)
			print("Got DM from {} to {}: {}".format(username, msgto, msg))
		elif msg == "list":
			socketfuncs.send_message(conn, str(len(msghistory)))
			for m in msghistory:
				socketfuncs.send_message(conn, m)
		else:
			msghistory.append("{} {}".format(address[0], msg))

s.close()
```


