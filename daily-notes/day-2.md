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



