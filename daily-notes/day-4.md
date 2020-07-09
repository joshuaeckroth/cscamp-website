---
title: Day 4 Notes
layout: note
---

# Day 4 Notes

Port scanning:

- nmap 127.0.0.1
- nmap -sV 127.0.0.1
- nmap -sV -p22 127.0.0.1

Directory scanning:

- dirb http://joshcscamp.artifice.cc /usr/share/dirb/wordlists/vulns/apache.txt

Wordpress scanning:

- wpscan --url http://joshcscamp.artifice.cc:10080/ --api-token TOKENHERE

Fuzzing:

- download list of things to fuzz with (like "big list of naughty strings")
- wfuzz -z file,blns.txt http://joshcscamp.artifice.cc/search?q=FUZZ

General website scanning:

- nikto -h http://127.0.0.1

Challenge for the break:

- nmap your own router (if you'd like): find your IP online
- nmap my house: (see Zoom recording for IP) (if host is down, add -Pn option)
- dirb scan on http://joshcscamp.artifice.cc to find interesting things
- wp-scan on http://joshcscamp.artifice.cc:10080 using a WPVulnDB API token
