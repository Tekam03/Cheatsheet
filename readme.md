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
  - [Metasploit](#metasploit)
  - [msfvenom (Payload generator)](#msfvenom)
  - [sqlmap (SQL injection)](#sqlmap)
* [Privilege Escalation](#privilege-escalation)
  - [suid](#suid)
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

---


# Exploitation

## metasploit
### msfconsole
**Metasploit Framework**

Framework to exploit vulnerabilities

**commands** :
- **search** : to search for a **module**
- **use** : to **use** a **module**
- **show options** : to show the **options** of the **module**
- **set** : to **set** the **options** of the **module**
- **run** : to **run** the **module**

**examples** :

Search for a module :
```
msfconsole
search type:exploit platform:windows
```

Use a module :
```
use exploit/windows/smb/ms08_067_netapi
```

Show the options of the module :
```
show options
```

Set the options of the module :
```
set RHOSTS
set RPORT
set PAYLOAD
set LHOST
set LPORT
```

Run the module :
```
run
```

---

## msfvenom
**Metasploit Framework**

Framework to create payloads

**options** :
- **-p** : followed by the **payload** to use (ex: windows/meterpreter/reverse_tcp)
- **-f** : followed by the **format** of the **payload** (ex: exe)
- **-o** : followed by the **output** of the **payload** (ex: /path/to/output)
- **-l** : to **list** the **payloads**
- **-a** : followed by the **architecture** of the **payload** (ex: x86)
- **-e** : followed by the **encoder** of the **payload** (ex: x86/shikata_ga_nai)
- **-i** : followed by the **iterations** of the **encoder** (ex: 5)
- **-b** : followed by the **bad characters** of the **payload** (ex: "\x00\x0a\x0d")

**examples** :

Create a payload :
```
msfvenom -p windows/meterpreter/reverse_tcp -f exe -o /path/to/output
```

Create a payload with **encoder** :
```
msfvenom -p windows/meterpreter/reverse_tcp -f exe -e x86/shikata_ga_nai -i 5 -o /path/to/output
```


---

## sqlmap
**SQL Injection**

Can be used to enumerate databases, tables, columns, etc. and to try to login

**options** :
- **-u** : followed by the **url**
- **--dbs** : to enumerate **databases**
- **--tables** : to enumerate **tables**
- **--columns** : to enumerate **columns**
- **--dump** : to **dump** the **data**
- **--users** : to enumerate **users**
- **--passwords** : to enumerate **passwords**
- **--current-user** : to enumerate the **current user**
- **--current-db** : to enumerate the **current database**
- **--batch** : to **batch** the **output**
- **--crawl** : to **crawl** the **url**

**examples** :

Enumerate databases :
```
sqlmap -u http://target --dbs
```

Enumerate tables :
```
sqlmap -u http://target --tables
```

Enumerate columns :
```
sqlmap -u http://target --columns
```

Dump data :
```
sqlmap -u http://target --dump
```

Enumerate users :
```
sqlmap -u http://target --users
```

Enumerate passwords :
```
sqlmap -u http://target --passwords
```

Enumerate current user :
```
sqlmap -u http://target --current-user
```


---






# Privilege Escalation

## Linux
## suid
**SUID**

**suid** is a permission that allows a user to run a program with the permissions of the file owner.

Website to find **suid** binaries : https://gtfobins.github.io/

**examples** :

Find **suid** binaries :
```
find / -perm -u=s -type f 2>/dev/null
```

Better script to find **suid** binaries :
[**suid3num**](https://raw.githubusercontent.com/Tekam03/Cheatsheet/main/Executables/suid3num.py)

---





# Post-exploitation


# TO DO
sublist3r (dns)
dnsrecon

