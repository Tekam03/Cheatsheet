# Tekam's cheatsheet about pentesting

<!-- table of content -->

<!-- toc -->

* [Information Gathering](#information-gathering)
  - [Common pages](#common-pages)
  - [Google Dorks](#google-dorks)
  - [Wappalyzer](#wappalyzer)
* [Enumeration](#enumeration)
  - [ffuf](#ffuf)
  - [wfuzz (Enumerate anything)](#wfuzz)
  - [dirb (Web content scanner)](#dirb)
  - [gobuster (Web content scanner)](#gobuster)
  - [whatweb (Web content scanner)](#whatweb)
  - [nikto (Web server scanner)](#nikto)
  - [wpscan (Wordpress)](#wpscan)
* [Scanning](#scanning)
  - [nmap (Network scanner)](#nmap)
* [Exploitation](#exploitation)
* [Privilege Escalation](#privilege-escalation)
* [Post Exploitation](#post-exploitation)


<!-- tocstop -->

<!-- table of content stop -->

# Information Gathering

## Usefull Websites

### SSL/TLS Certificate Logs

https://crt.sh/

https://ui.ctsearch.entrust.com/ui/ctsearchui


## Common pages

- /robots.txt - list of files that are not indexed by search engines
- /sitemap.xml - list of pages on the website

---

## Google Dorks


- site:example.com - search for a specific website address
- intitle:example - search for a specific word in the title
- inurl:example - search for a specific word in the url
- filetype:pdf - search for a specific file type

**examples**

exclude any website that are www.domain.com but only website that has domain.com in it
```
-site:www.domain.com site:*.domain.com
```

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

## ffuf
**Web application fuzzer**

Based on wfuzz

**options** :
- **-c** : Colorize the output
- **-w** : Wordlist
- **-u** : URL
- **-H** : Headers (ex: "Host: FUZZ.example.com")
- **-X** : HTTP method (ex: GET, POST, PUT, DELETE)
- **-e** : Extensions (ex: .php,.html)
- **-d** : Data (ex: "username=admin&password=FUZZ")

- **Matching** : 
  - **-mc** : Match status code
  - **-mr** : Match regex
  - **-mw** : Match word
  - **-ml** : Match length
  - **-mh** : Match headers
  - **-ms** : Match size

**examples** :


Username enumeration
```
ffuf -w /usr/share/wordlists/seclists/Usernames/Names/names.txt -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=test" -u http://website.com/customers/signup -mr "username already exists"
```


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
- **-H** : to use a **HEADER** (ex : "Host: FUZZ.test.com")
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

---

## nikto
**Web server scanner**

Returns generic information about config, vulnerabilities, etc

**options** :
- **-h** : followed by the **host**
- **-root** : followed by the **root directory**

**examples** :

Basic usage :
```
nikto -h http://target
```

With **root directory** :
```
nikto -h http://target -root /path/to/root
```

---

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

## nmap
**Network Scanner**

Can find open ports, services, etc.

**options** :
- **Important options**:
  - **-sV** : Enable version detection
  - **-O** : Enable OS detection
  - **--script** : followed by the **script name** to run (ex: vuln)
  - **-A** : Enable OS detection, version detection, script scanning, and traceroute (Very slow)

- **Scan Type** *(Facultative)*:
  - **-sS** : TCP SYN scan (**default**)
  - **-sT** : TCP connect scan (**default without root**)
  - **-sU** : UDP scan

- **Host Discovery** *(Facultative)* :
  - **-sn** : Disable port scanning. Host discovery only. (Range of IP)
  - **-Pn** : Disable host discovery. Port scan only. (Range of IP)

- **Port scanning** *(Facultative)*:
  - **-p** : followed by the **port** to scan (ex: 80)
  - **-p** : followed by the **ports range** to scan (ex: 1-100)
  - **-p** : followed by the **ports list** to scan (ex: 80,443,8080)
  - **-p** : followed by the **service name** to scan (ex: http,https)
  - **-p-** : scan all ports
  - **-F** : scan **100 most common ports**
  - **--top-ports** : followed by the top **number of ports** to scan (ex: 100)

- **Aggressivity** *(Facultative)*:
  - **-T0** : Paranoid
  - **-T1** : Sneaky
  - **-T2** : Polite
  - **-T3** : Normal
  - **-T4** : Aggressive
  - **-T5** : Insane

**examples** :

Basic usage :
```
nmap http://target
```

With **version detection** :
```
nmap -sV http://target
```

With **OS detection** :
```
nmap -O http://target
```

With **script scanning** :
```
nmap --script vuln http://target
```

Normal scan :
```
sudo nmap -sV -O http://target
```

Normal Fast scan :
```
sudo nmap -F -sV -O http://target
```




# Exploitation

# Privilege Escalation

# Post-exploitation


# TO DO
sublist3r (dns)
dnsrecon

