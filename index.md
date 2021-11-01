## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/magixUnbreakable/magixUnbreakable.github.io/edit/main/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

# Hashing - Crypto 101
##   https://tryhackme.com/room/hashingcrypto101
##   create by: magixUnbreakable
##   creation date: 1.11.2021

### TASK 3: USES FOR HASHING
	2 main purposes
	- verify integrity of data
	- verifying passwords
	RAINBOW TABLE
	- lookup table of hashes to plaintexts, so you can quickly find out what password a user had just from the hash
	
**_QUESTIONS:_**
**1. Crack the hash "d0199f51d2728db6011945145a1b607a" using the rainbow table manually.**
	- look provided hash in example table in this task.

**2. Crack the hash "5b31f93c09ad1d065c0491b764d04933" using online tools**
	- I used this [link](https://hashes.com/en/decrypt/hash)
  
### TASK 4: RECOGNISING PASSWORD HASHES 
**_QUESTIONS:_**
**1. How many rounds does sha512crypt ($6$) use by default?**
	- reading [man page](https://man7.org/linux/man-pages/man5/login.defs.5.html) is quite helpful

**2. What's the hashcat example hash (from the website) for Citrix Netscaler hashes?**
	- check provided hashcat [link](https://hashcat.net/wiki/doku.php?id=example_hashes)
	- look for "Citrix Netscaler"

**3. How long is a Windows NTLM hash, in characters?**
	- quick google search give us answer
	
### TASK 5: PASSWORD CRACKING
**_QUESTIONS:_**
**1. Crack this hash: $2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG**
	- for this hash I'm gona use _hashcat_
	- stored this hash in simple text file on my attack machine:
	
```
$ cat hash1.hash 
$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG
```
	- we know from previous tasks that "$2a$" is bcrypt()
	- let's indentifie the hash mode we would liek to use
```
$ hashcat -h | grep bcrypt
3200 | bcrypt $2*$, Blowfish (Unix)                     | Operating System
```
- we got only 1 option, let's start the hashcat

```
$ hashcat -m 3200 hash1.hash /usr/share/wordlists/rockyou.txt
```

- -m 					= hash type
- hash1.hash  				= file where i stored the hash
- /usr/share/wordlists/rockyou.txt	= my stored rocklist.txt

```
$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG:*****
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: bcrypt $2*$, Blowfish (Unix)
Hash.Target......: $2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01p...ddr1ZG
Time.Started.....: Mon Nov  1 04:01:02 2021 (17 secs)
Time.Estimated...: Mon Nov  1 04:01:19 2021 (0 secs)
```

**2. Crack this hash: 9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1	hint: SHA2-256**
	- 1st let's check online tools for crack hashes i.e.: [crackstation](https://crackstation.net)
	- which give us answer
	
**3. Crack this hash: $6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0**
	- start of the hash $6$ indicate that this is SHA 512
	- using hashcat, we find mode to use

```
hashcat -h | grep sha512
  21000 | BitShares v0.x - sha512(sha512_bin(pass))        | Raw Hash
   1710 | sha512($pass.$salt)                              | Raw Hash, Salted and/or Iterated
   1720 | sha512($salt.$pass)                              | Raw Hash, Salted and/or Iterated
   1740 | sha512($salt.utf16le($pass))                     | Raw Hash, Salted and/or Iterated
   1730 | sha512(utf16le($pass).$salt)                     | Raw Hash, Salted and/or Iterated
  20200 | Python passlib pbkdf2-sha512                     | Generic KDF
   6500 | AIX {ssha512}                                    | Operating System
   1800 | sha512crypt $6$, SHA512 (Unix)                   | Operating System
  21600 | Web2py pbkdf2-sha512                             | Framework
```

- now we got mode (1800), lets create text file with given hash and start hashcat:

```
hashcat -m 1800 hash3.hash /usr/share/wordlists/rockyou.txt

$6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0:*****
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: sha512crypt $6$, SHA512 (Unix)
Hash.Target......: $6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3...Yr1wQ0
Time.Started.....: Mon Nov  1 04:25:58 2021 (10 secs)
Time.Estimated...: Mon Nov  1 04:26:08 2021 (0 secs)
```

**4. Bored of this yet? Crack this hash: b6b0d451bbf6fed658659a9e7e5598fe		hint: Try online, rockyou isn't always enough.**
	- using online [tool](https://crackstation.net) we get our last answer for this task.
	
### TASK 6: HASHING FOR INTEGRITY CHECKING
	- hash before send file and hash after received file => check integrity, even if one bit was changed hashes will not match.
	HMACS
	- verify the authenticity and integrity of data
	
**_QUESTIONS:_**
**1. What's the SHA1 sum for the amd64 Kali 2019.4 ISO? http://old.kali.org/kali-images/kali-2019.4/**

	- navigate to provided [link](http://old.kali.org/kali-images/kali-2019.4/SHA1SUMS) to retrieve our flag
	
**2. What's the hashcat mode number for HMAC-SHA512 (key = $pass)?**
	- we can find it easily by looking in hashcat help page

```
hashcat -h | grep HMAC-SHA512
```
