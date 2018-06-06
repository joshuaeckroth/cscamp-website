---
title: Day 3 Notes
layout: note
---

# Day 3 Notes

## bbsserver-v1.2.py

```
import socketfuncs
import socket
import threading

dms = {}
msghistory = []

def handleUser(conn, address):
	global dms, msghistory
	print("Got connection from {}".format(address))
	socketfuncs.send_message(conn, "BBS Server 1.2")
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
			if msgto in dms:
				dms[msgto].append(msg)
			else:
				dms[msgto] = [msg]
		elif msg == "listdm" or msg == "listdms":
			if username in dms:
				socketfuncs.send_message(conn, str(len(dms[username])))
				for m in dms[username]:
					socketfuncs.send_message(conn, m)
			else:
				socketfuncs.send_message(conn, "0")
		elif msg == "list":
			socketfuncs.send_message(conn, str(len(msghistory)))
			for m in msghistory:
				socketfuncs.send_message(conn, m)
		else:
			msghistory.append("{} {}".format(address[0], msg))


s = socket.socket()
s.bind(('0.0.0.0', 10333))

while True:
	s.listen(10)
	(conn, address) = s.accept()
	userThread = threading.Thread(target=handleUser, args=(conn, address))
	userThread.start()
s.close()
```

## bbsclient-v1.2.py

```
import socketfuncs
import socket
import sys

s = socket.socket()
ip = input("IP: ")
username = input("Username: ")
s.connect((ip, 10333))
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

## DNS server

Goal of DNS server:

1. bind to port 10334
2. accept connection
3. send protocol info ("DNS Server 1.0")
4. receive message containing domain name to check
5. look in dictionary of names
6. if name found in dictionary, send ip address
7. else, send "unknown domain"

Update BBS client:

1. ask user for domain name
2. open connection with DNS server
3. ask DNS server for ip for given domain
4. get IP from DNS server
5. use IP to connect to BBS server (as usual)


