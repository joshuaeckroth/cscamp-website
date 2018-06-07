---
title: Day 4 Notes
layout: note
---

# Day 4 Notes

## Passwords

Role of passwords: to ensure only certain people have access to certain resources; to prove that you are who you say you are

How do passwords allow a system to know who you are? The fundamental idea is only you know that password. "Asymmetry of information": only you know something, no one else does.

## How systems handle passwords

Suppose you want to allow logins with your BBS server: you would need a dictionary of usernames and associated passwords. But if the code is open-source, that's not going to work. Also, if it's closed-source, you probably still don't want it in the code because employees can read it.

Solutions: use a database or a file to store the usernames and passwords.

## How systems SHOULD handle passwords

If they're just stored in their original form, that opens up some risk:

- If the file is opened by anyone, the passwords are visible.
- If the database is ready by anyone, the passwords are visible.

To ensure even IF the database is ready by outside parties, STILL the passwords are not known, you have to obscure them... in such a way that they are still usable as passwords.

Solution:

- Transform the passwords into an "encoded" form that cannot be reversed into the original password. We call this a hash.
- When a user tries to log in, run the hash function on their password (that they typed) and check that it equals the hash in the database.

## Hashing

A hash is a function that converts user input into a fixed-length output that seems random based on the input. It CANNOT BE REVERSED.

Common hash functions: MD5, SHA-1, SHA-256, SHA-512, Bcrypt

For example, MD5:

```
$ echo "password1" | md5sum
10b222970537b97919db36ec757370d2  -
```

## Password complexity

Only digits:

- 6 length: 10^6 = 1,000,000
  - <1 sec
- 10 length: 10^10 = 10,000,000,000
  - <1 sec

Only letters:

- 6 length: 26^6 = 308,915,776
  - <1 sec
- 10 length: 26^10 = 141,167,095,653,376
  - 44 min

Upper/lower case letters and numbers:

- 6 length: 62^6 = 56,800,235,584
  - 1 sec
- 10 length: 62^10 = 839,299,365,868,340,224
  - 181 days

Upper/lower case letters, numbers, symbols (32):

- 6 length: (52+10+32)^6 = 689,869,781,056
  - 13 sec
- 10 length: (52+10+32)^10 = 53,861,511,409,489,970,176
  - 32 years


What happened to me & Tracy:

- We lost $200 out of our bank of america checking
- Somebody set up a mobile transfer using the email associated with the account
- It was a yahoo account, but this password was not hacked
- The backup email account for this main account... was a yahoo account
- We forgot that account existed
- So the MD5 of that backup was hacked
- Then they did a password recover for the main account, which sent an email to the backup
- They changed the password for the main account
- Then they set up mobile banking

Suppose another scenario:

- You have a strong facebook password
- It's shared with your MyHeritage account
- MyHeritage is hacked
- Hackers get your password
- They automatically attempt logging in to popular sites
- (There are thousands of MyHeritage's out there)

## Password manager

```
import random
import pickle
import os.path

db = {}

def save_passwords():
	with open("passwords.pkl", "wb") as f:
		pickle.dump(db, f)

def load_passwords():
	global db
	if os.path.exists("passwords.pkl"):
		with open("passwords.pkl", "rb") as f:
			db = pickle.load(f)

def generate_password():
	length = random.randint(10, 15)
	password = ""
	for i in range(length):
		password += chr(random.randint(33, 126))
	return password

load_passwords()
while True:
	cmd = input("Type 'new' or 'show': ")
	if cmd == "new":
		website = input("Website: ")
		username = input("Username: ")
		password = generate_password()
		print("Generated password: {}".format(password))
		if website in db:
			db[website].append((username, password))
		else:
			db[website] = [(username, password)]
		save_passwords()
	elif cmd == "show":
		website = input("Website: ")
		if website in db:
			for (username, password) in db[website]:
				print("{} {}".format(username, password))
		else:
			print("Unknown website")
	elif cmd == "quit":
		break
```

## Encryption

Unlike hashing, encryption is reversible. Usually, we reverse it with a password ("key").

There are two kinds of encryption:

- Symmetric
  - The SAME key encrypts and decrypts
  - AES, DES, Blowfish
- Asymmetric
  - A different key decrypts
  - PGP, RSA
































