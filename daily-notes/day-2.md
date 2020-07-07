---
title: Day 2 Notes
layout: note
---

# Day 2 Notes

[Slides about TCP/IP](/course-material/csss-day2-slides.pdf)

---

```
# server.py for chat (incomplete)

import socket
import socketfuncs

history = []

s = socket.socket()
s.bind(("0.0.0.0", 10000))
while True:
    print("Listening...")
    s.listen()
    (conn, address) = s.accept()
    print("Got connection from %s" % address[0])
    msg = socketfuncs.receive_message(conn)
    history.append(msg)
    print("History length: %d" % len(history))
    for h in history:
        print(" >> %s" % h)
s.close()
```

---

```
# client.py for chat (incomplete)

import socket
import socketfuncs

s = socket.socket()
s.connect(("18.234.145.155", 10000))
socketfuncs.send_message(s, "Test message")
s.close()
```

---

Outline for how to update server for sending chat history:

```
Task:

Updated client and server so that server keeps
history of all received messages, and client
is provided this history when client connects.


Server side:

1. open socket, start loop
2. inside loop:
   a. listen, accept
   b. server tells client # of history items
   c. server sends each history item 1 by 1
   d. server receives client's new message
   e. adds message to history, repeats loop
```

---

```
# chat server

import socket
import socketfuncs
import sys

def send_history(conn, history):
	socketfuncs.send_message(conn, f"{len(history)}")
	for h in history:
		socketfuncs.send_message(conn, h)

#Sockets & creates history list
history = []
s = socket.socket()
s.bind(("0.0.0.0", 10125))

try:
	while True:
		print("Listening...")
		s.listen()
		(conn, address) = s.accept()
		print("Connected.")

		send_history(conn, history)

		#Prints new message and updates history
		msg = socketfuncs.receive_message(conn)
		if msg == "history":
			send_history(conn, history)
		else:
			print("New Message: %s" % msg)
			history.append(msg)

except:
	print("Error")
	s.close()
	sys.exit()

s.close()
```

---

```
# chat client

import socket
import socketfuncs
import sys

def receive_history(s):
	print("History:")
	x = socketfuncs.receive_message(s)
	for i in range(int(x)):
		print(socketfuncs.receive_message(s))

#Sockets and prints history
s = socket.socket()
s.connect(("18.234.145.155", 10125))
receive_history(s)
#Tries to catch user force quit during input
try:
	msg = input("Message: ")
except:
	print("Error: Force Quit")
	socketfuncs.send_message(s, "Error: Force Quit")
	s.close()
	sys.exit()

#Makes sure the input exists before sending it to server
if msg == "history":
	socketfuncs.send_message(s, msg)
	receive_history(s)
elif msg != "":
	socketfuncs.send_message(s, msg)
else:
	socketfuncs.send_message(s, "Invalid Input.")
	print("Invalid Input.")

s.close()
```

---

```
# chat server with pickle (to save history after server quits),
# and dates and .com information of sender

import os.path
import pickle
import socket
import socketfuncs
from datetime import datetime

def load_posts():
    if os.path.exists("posts.pkl"):
        with open("posts.pkl", 'rb') as infile:
            return pickle.load(infile)
    else:
        return []

def save_posts(posts):
    with open("posts.pkl", 'wb') as outfile:
        pickle.dump(posts, outfile)

s = socket.socket()
s.bind(('0.0.0.0', 10333))

posts = load_posts()

while True:
    s.listen(10)
    (conn, address) = s.accept()
    try:
        domain = socket.gethostbyaddr(address[0])[0]
    except:
        domain = address[0]
    print("Accepted connection from {}".format(domain))

    # send all posts
    # first, send count of posts
    print("Sending post length: {}".format(len(posts)))
    socketfuncs.send_message(conn, str(len(posts)))
    for i in range(len(posts)):
        print("Sending post: {}".format(posts[i]))
        socketfuncs.send_message(conn, posts[i])

    msg = socketfuncs.receive_message(conn)
    if len(msg) != 0:
        print("Adding post: {}".format(msg))
        time = datetime.now().isoformat()
        posts.append("[{} {}] {}".format(time, domain, msg))
        save_posts(posts)
```

---

```
# chat client that goes with chat server immediately above

import socket
import socketfuncs

s = socket.socket()
s.connect(('18.234.145.155', 10333))

# receive number of posts
numposts = int(socketfuncs.receive_message(s))
# loop through each post
for i in range(numposts):
    post = socketfuncs.receive_message(s)
    print(post)

msg = input("Add your post: ")
socketfuncs.send_message(s, msg)
s.close()
```

