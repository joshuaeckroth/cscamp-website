---
title: Day 1 Notes
layout: note
---

# Day 1 Notes

## 9-10am Zoom

Techniques of cybersecurity

Red team / blue team:

- red team attempts to attack systems (approved by organization)
- blue team attempts to secure systems

Discovery:

- network mapping: finding IP addresses
- port scanning: finding open ports on a machine
  - port = number (like 22) for a specific software (like SSH) on a machine
- phishing: fake email and login page for finding out passwords, etc.
- password cracking: reverse engineering hashed passwords
- vulnerability enumeration: looking in public databases for vulnerable software
  - CVE: common vulnerabilities and exposures
  - 0-day exploit: the exploit is released before the fix
    - 3-day exploit: the exploit is released 3 days after the fix
- fuzzing: generating random/bad data to see if it breaks things

Attacks:

- exploiting CVEs
- DDOS: distributed denial of service
- MITM: man in the middle
- Rootkits and trojans
- Viruses
- XSS: cross-site scripting
- SQL injection: hijacking a database

Defense:

- Hiding open ports (firewalls)
- Updating software
- Training people: prevent phishing
- Good password hashing
- Encryption
- Antivirus


```
# gpa.py

name = input("What is your name? ")

gpa = 0.0
n = 0
while True:
	grade = input("What grade did you receive? (A, B, etc.) ")
	if len(grade) == 0:
		break

	# Let's convert grade into GPA

	if grade == "A" or grade == "a":
		gpa = gpa + 4.0
	elif grade == "B" or grade == "b":
		gpa = gpa + 3.0
	elif grade == "C" or grade == "c":
		gpa = gpa + 2.0
	elif grade == "D" or grade == "d":
		gpa = gpa + 1.0

	n = n + 1

if n > 0:
	gpa = gpa / n

	print("Your GPA is: %.2f" % gpa)

	if gpa >= 3.0:
		print("You're on the honor's list.")
	elif gpa < 2.0:
		print("You're at risk of not graduating.")
	else:
		print("You're doing fine.")
```

Python challenge for afternoon:

- Create a guess-the-number game:
  - use if/else to see if they're too high or too low
  - print a message when it's correct or too high or too low
  - allow three guesses
- Further challenge:
  - allow infinite guesses (i.e., a loop)
  - (quit a loop by typing Control-C in Linux)

