# Tekam's cheatsheet about pentesting

<!-- table of content -->

<!-- toc -->

* [Information Gathering](#information-gathering)
  - [Common pages](#common-pages)
  - [Google Dorks](#google-dorks)
  - [Wappalyzer](#wappalyzer)
* [Enumeration](#enumeration)
  - [wfuzz (Enumerate anything)](#wfuzz)
  - [dirb (Web content scanner)](#dirb)
  - [gobuster (Web content scanner)](#gobuster)
  - [wpscan (Wordpress)](#wpscan)
* [Scanning](#scanning)
* [Exploitation](#exploitation)
* [Privilege Escalation](#privilege-escalation)
* [Post Exploitation](#post-exploitation)


<!-- tocstop -->

<!-- table of content stop -->

# Information Gathering



## Common pages

- /robots.txt - list of files that are not indexed by search engines
- /sitemap.xml - list of pages on the website

---

## Google Dorks


- site:example.com - search for a specific website address
- intitle:example - search for a specific word in the title
- inurl:example - search for a specific word in the url
- filetype:pdf - search for a specific file type

---

## Wappalyzer
browser extension that detects technologies used on websites

- https://www.wappalyzer.com/

---

# Enumeration
**Good wordlists** :
- /usr/share/wordlists/dirb/wordlists/common.txt -> common words
- /usr/share/spiderfoot/dicts/subdomains.txt -> subdomains

---

## wfuzz
**Web application fuzzer**

enumerate directories and files

 the url must contain **_FUZZ_** to be replaced by the wordlist


**options** :
- **-c** : show the **status code** in **color**
- **-z** : followed with **payload** a comma then what you want
  - **file** : to use a **wordlist**
  - **range** : to use a **range**
  - **list** : to use a **list** of words
- **-d** : to use a **POST body**
- **--hc/sc CODE** : to **hide/show** the **status code**


**examples** :

With a **wordlist** :
```
wfuzz -c -z file,/path/to/wordlist http://target/FUZZ
```

With a **range** :
```
wfuzz -c -z range,1-100 http://target/FUZZ
```

With a **list** :
```
wfuzz -c -z list,admin-root-login http://target/FUZZ
```

With a **list** and **no 404 status code** :
```
wfuzz -c --hc 404 -z list,admin-root-login http://target/FUZZ
```

With 2 **wordlists** :
```
wfuzz -c -z file,/path/to/wordlist1,file,/path/to/wordlist2 http://target/FUZZ/FUZ2Z
```

POST request :
```
wfuzz -c -z file,/path/to/wordlist --hc 404 -d "username=admin&password=FUZZ" http://target/login.php
```

---

## dirb
**Web content scanner**

Enumerate public directories and files


**options** :
- **-r** : do **not do recursive search**
- **-x** : followed by a **file extension**

**examples** :

Basic usage :
```
dirb http://target
```

With a **wordlist** :
```
dirb http://target /path/to/wordlist
```

With a **wordlist** and a **file extension** :
```
dirb http://target /path/to/wordlist -X .txt
```

---
## gobuster
**Web content scanner**

Enumerate public directories and files

**commands** :
- **dir** : to enumerate directories
- **dns** : to enumerate subdomains
- **vhost** : to enumerate virtual hosts
- **fuzz** : to enumerate FUZZ

**options** :
- **-u** : followed by the **url**, can include subdirectories
- **-w** : followed by the **wordlist**
- **-d** : followed by the **domain** (target.com) (only for dns)

**examples** :

Enumerate directories :
```
gobuster dir -u http://target -w /path/to/wordlist
```

Enumerate subdomains (subdomain.target.com):
```
gobuster dns -d target.com -w /path/to/wordlist
```

Enumerate with FUZZ :
```
gobuster fuzz -u http://target/FUZZ -w /path/to/wordlist
```

---

## whatweb
**Gives information about a website**

**options** :
- **-a** : aggressivity
  - **1** : stealthy
  - **3** : Aggressive
  - **4** : Heavy
- **-v** : verbose

**examples** :

Basic usage :
```
whatweb http://target
```

With **aggressivity** and **verbose** :
```
whatweb -a 3 -v http://target
```

## wpscan
**WordPress website scanner**

Can be used to enumerate users, plugins, themes, etc. and to try to login

**options** :
- **--url** : followed by the **url**
- **-e** : followed by the **plugin/theme/user** to enumerate **(p/t/u)**
- **-U** : followed by the list of **username** to enumerate to login
- **-P** : followed by the list of **password** to enumerate to login

**examples** :

Basic usage :
```
wpscan --url http://target
```

Enumerate users :
```
wpscan --url http://target -e u
```

Try to login :
```
wpscan --url http://target -U /path/to/wordlist -P /path/to/wordlist
```

---


# Scanning

# Exploitation

# Privilege Escalation

# Post-exploitation
