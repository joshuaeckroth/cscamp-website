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

## Domain names

DNS: 35.185.60.223

```
import socketfuncs
import socket

names = {
'bear': '35.237.248.144',
'baboon': '35.237.14.26',
'bishop': '35.185.60.223',
'boone': '35.237.60.25',
'beth': '35.237.10.113',
'banana': '35.237.155.182',
'bartlet': '35.237.41.103',
'bescar': '35.237.69.209',
'balloon': '35.237.82.158',
'burgerking': '35.237.96.244',
'barrel': '35.237.202.155',
'buccaneer': '35.237.157.49',
'broth': '35.237.63.63'}

s = socket.socket()
s.bind(('0.0.0.0', 10334))

while True:
    s.listen(10)
    (conn, address) = s.accept()
    print("Got connection from {}".format(address))
    socketfuncs.send_message(conn, "DNS Server 1.0")
    domain = socketfuncs.receive_message(conn)
    if domainname in names:
        socketfuncs.send_message(conn, names[domainname])
    else:
        socketfuncs.send_message(conn, "unknown domain")

s.close()
```

### bbsclient-with-dns-1.2.py

```
import socketfuncs
import socket
import sys

domain = input("Domain: ")
s2 = socket.socket()
s2.connect(("127.0.0.1", 10334))
msg = socketfuncs.receive_message(s2)
if msg != "DNS Server 1.0":
    print("Wrong DNS protocol")
    sys.exit()
socketfuncs.send_message(s2, domain)
ip = socketfuncs.receive_message(s2)


s = socket.socket()
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
