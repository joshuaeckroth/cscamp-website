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


