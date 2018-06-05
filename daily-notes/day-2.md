---
title: Day 1 Notes
layout: note
---

# Day 1 Notes

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

