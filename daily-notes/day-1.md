---
title: Day 1 Notes
layout: note
---

# Day 1 Notes

## Cybersecurity notes

Definitions:

- white-hat: professional pen-testers, security governance, IT professionals, etc.
- black-hat: hackers, criminals
- gray-hat: white-hat by day, black-hat by night; and/or does black-hat activities but reports them responsibly
- "script kiddie": people who just download solutions and run them

Techniques:

- DDOS: distributed denial of service
  - flood server with requests, from thousands of machines
- Trojans: a program that looks innocuous (legit) but in fact has something alongside that monitors your keys, etc.
  - Danooct1
  - antivirus
- worm:
  - Morris Worm
  - ILOVEYOU
  - programs that copy themselves to other machines
- ransomware:
  - wannacrypt
  - encrypts all your files with symmetric encryption
  - if you pay a bitcoin address, they give you the password back
  - offsite/offline backups
- virus:
  - it's in a .EXE, and it copies itself to other .EXEs
  - antivirus
- rootkit:
  - establishes root access for someone else
  - monitor OS files to see if they changed
- key logger:
  - often in a trojan
  - keeps a history of every key you type
- phishing:
  - tricking users to give away passwords
- spear phishing:
  - tricking a particular person
  - Podesta
- botnets:
  - machines that probably act normal but can be controlled remotely
  - DDOS
  - Spam
- Slowloris:
  - keeps connections alive as long as possible
- 0days:
  - an exploit that hasn't been published yet
  - 5day: known publicly for 5 days
- Meltdown & Spectre:
  - hardware error in Intel processors
- Heartbleed:
  - SSH exposed
- SQL-injection:
  - sanitize inputs
- XSS (cross-site-scripting):
  - javascript runs on a website, is believed to be from that website (e.g., facebook), yet, it's actually JS provided a person
  - can show your login/etc. on a site
  - sanitize inputs
- MITM (man-in-the-middle):
  - your machine is between the user and the real server
  - you watch the traffic
  - combined with SSL stripping, you can make them think they're using a secure site but it's really yours
- social engineering:
  - Kevin Mitnick
- fuzzing:
  - attack a system with bizarre inputs until it breaks
- dumpster diving
- candydrop:
  - drop off a USB drive, someone is curious and plugs it into a computer

## Python examples

### simple.py

```
name = input("What is your name? ")
print("Nice to meet you, {}!".format(name))
print("Really nice to meet you, " + name)
```

### retirement.py

```
age = int(input("Enter your age: "))
if age >= 65 or age == 30:
    print("You are retirement age")
elif (age >= 80 and age <= 90) or age >= 100:
    print("You are way over retirement age")
else:
    print("Not yet ready for retirement")
```

### zodiac.py

```
# goal: determine astrology sign for a birth date

# dates: https://en.wikipedia.org/wiki/Zodiac#Table_of_dates

import sys

name = input("Name: ")
month = int(input("Birth month (1-12): "))
if month > 12 or month < 1:
    print("Invalid month!")
    sys.exit()
day = int(input("Birth day (1-31): "))
if day > 31 or day < 1:
    print("Invalid day!")
    sys.exit()

if (month == 3 and day >= 21) or (month == 4 and day <= 20):
    sign = "Aries"
elif (month == 4 and day >= 21) or (month == 5 and day <= 21):
    sign = "Taurus"
elif (month == 5 and day >= 22) or (month == 6 and day <= 21):
    sign = "Gemini"
elif (month == 6 and day >= 22) or (month == 7 and day <= 22):
    sign = "Cancer"
elif (month == 7 and day >= 23) or (month == 8 and day <= 22):
    sign = "Leo"
elif (month == 8 and day >= 23) or (month == 9 and day <= 23):
    sign = "Virgo"
elif (month == 9 and day >= 24) or (month == 10 and day <= 23):
    sign = "Libra"
elif (month == 10 and day >= 24) or (month == 11 and day <= 22):
    sign = "Scorpio"
elif (month == 11 and day >= 23) or (month == 12 and day <= 21):
    sign = "Sagittarius"
elif (month == 12 and day >= 22) or (month == 1 and day <= 20):
    sign = "Capricorn"
elif (month == 1 and day >= 21) or (month == 2 and day <= 19):
    sign = "Aquarius"
else:
    sign = "Pisces"

print("{}'s sign is {}.".format(name, sign))
```

