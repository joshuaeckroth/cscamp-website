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

---

Password cracking techniques:

```
hashcat --force --status -o cracked.txt -m 0 -a 0 hashes.txt english2.txt
hashcat --force --status -o cracked.txt -m 0 -a 0 hashes.txt english2.txt -r /usr/share/hashcat/rules/best64.rule
```

Find other password dictionaries online and download with:

```
wget "http://....."
```

Also look at the other rules that come with hashcat:

```
ls /usr/share/hashcat/rules
```

Challenge! Crack these passwords (save them to a file first):

```
098f6bcd4621d373cade4e832627b4f6
ad0234829205b9033196ba818f7a872b
5d41402abc4b2a76b9719d911017c592
5f4dcc3b5aa765d61d8327deb882cf99
827ccb0eea8a706c4c34a16891f84e7b
819d15e35ccf3b53e42a027ff3056f7c
90774825fdd73b752d03ab6d286a7d8b
d2a69d671403ea3e207b5279efa9bb69
fc5868ad290ecbd988f829d535a4a5e4
180138e984fab7a7b84b1ccceebcff18
5985effe477ed16749a104be9376ca10
098f6bcd4621d373cade4e832627b4f6
ad0234829205b9033196ba818f7a872b
5d41402abc4b2a76b9719d911017c592
5f4dcc3b5aa765d61d8327deb882cf99
4b2b8b7e3866f4331b1910a4b1421419
1eb0d687f34d3160bca348632ab8e76c
f32dc058682127043c1419f71a8deadb
77e124cfa7fc87e04e2a1f2531d1ed27
5d75d387736252b2176ce1d94c87a3a2
361633153a464830a1fe85dec5efab17
dc647eb65e6711e155375218212b3964
f6039d44b29456b20f8f373155ae4973
a317874219c9cfbc945347321711ac7d
06c6f75866aaa61923ad464f803847d2
2d43e63193c00fc58f835c54ffec0498
54fb3e2d90c948c9d0eb99c173dd9a83
433e3b37ca7ee49a7ec609014917575c
9b6684d6b9615d4c91368546f96ffe2c
ac06f796b4fea95a6a44935cd744cab6
01678934c5d65bad3821ad75226eb2a0
af78267790131a480717622ac1b531fd
c6bc7417cf3e643bd11a2e67b40c7311
c51901eb5928a9d511a086909bcc9c93
3cb2ae3c3a7dc72b428b670a6ec28a70
d31f217e444ed5210b8634e91ea594d6
fff0c6e44004116d73e0c096403611ef
28998ab604d3fa9d08ccd78d10b97618
b497dd1a701a33026f7211533620780d
5b4b1489ee415562a7b45a78361d7ba8
10ae9fc7d453b0dd525d0edf2ede7961
b471a28a954744826ebba359fb9cb279
f400243856b84f9619d954762a75f89c
96e7e6e33212e1ef6571e99d99cc9bd0
7765a7317143fa722b02849cd0c516f3
037d5ef68dc4981169963e1180942155
8b3d20e89763abd7e127f4d4c72f1571
25f9e794323b453885f5181f1b624d0b
ab87572f58b90318534dcccb3b640374
9b589ed9c519cce13b2e7602be1ed08f
75304059ac1546f45d8f1d97edec843d
5f4dcc3b5aa765d61d8327deb882cf99
319f4d26e3c536b5dd871bb2c52e3178
e10adc3949ba59abbe56e057f20f883e
25d55ad283aa400af464c76d713c07ad
25f9e794323b453885f5181f1b624d0b
482c811da5d5b4bc6d497ffa98491e38
884ecc7ac05cb5d52aa970f523a3b7e6
5e0539d9a3a1719c20dfa205f949c496
967ef2eb34634ba418db94dab610ba6f
620829225c0923a37aacd8eafd21335c
0b3f45b266a97d7029dde7c2ba372093
2deb210ba69bb8dbafd513fbeb0ec47d
e2797f6581dacf849a76bb9dd0faf631
47b7bfb65fa83ac9a71dcb0f6296bb6e
5d10cc941c0f3747b6bccc0bbcbd3d34
2ee0030bda47f9398b9a9506410bf073
780a0ffebc366689b3353abb76eb3ffd
00098fbb5c7fb40080da5bc19b7d9754
c75d9d8e4fb4feb7ab1e319e5554a3ea
225eb9cbcb223fc924e2952b845850c5
f24a9cb58c6bffe4230e779972e83932
add4548e6a979f4a8aa70977097f7cec
058c69c686dd8aabd063e14d951a3f23
482c811da5d5b4bc6d497ffa98491e38
d2bc2f8d09990ebe87c809684fd78c66
6c569aabbf7775ef8fc570e228c16b98
5d41402abc4b2a76b9719d911017c592
4ccd8c52f55404609e9c417fd95b13b2
ca353a3d33c8659bd1854e7c95019bb0
d9ce320e9607636e2dba50d23dc1704d
abfce0ecd55a9245fc17c581039632fa
31435008693ce6976f45dedc5532e2c1
6ab3d0526bb1d023ab7c4fee4fb684df
b7e283a09511d95d6eac86e39e7942c0
43fc93a3fdbbc8a49ccf054d04904a3d
8d8a78edd0ed18df0ffbdcfac20055e4
17accfd6c9f3ebca1c389a71f0f0ca4f
bcea987445159d51ba5ee93618db97d4
2f9200512ae24676b7b9a4acbc33d9c1
6b647077fe1663fd20a9922432c28a16
90f5479a539b5ea9dc03d2c14006d412
1f7a4251e1bb07f1e76f5707aeb22b34
6ee76142121ae4c2c3c80a05efdbd08a
ef7219c9e91b82b4cb99c14e3cdcfd19
2e8be1294260cc102262facf12a6ae74
```
