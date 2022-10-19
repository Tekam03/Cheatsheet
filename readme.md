# Tekam's cheatsheet about pentesting

<!-- table of content -->

<!-- toc -->

* [Information Gathering](#information-gathering)
  - [Common pages](#common-pages)
  - [Google Dorks](#google-dorks)
  - [Wappalyzer](#wappalyzer)
* [Enumeration/Scanning](#enumerationscanning)
  - [FFUF](#ffuf)
  - [dirb](#dirb)
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

# Enumeration/Scanning

## ffuf 
**Fast Web Fuzzer** 

Changes the word **FUZZ** in the url with a **wordlist** and checks if the url exists

**Directory Discovery**
```
ffuf -w /path/to/wordlist -u http://target/FUZZ
```


**match 200 responses**
```
ffuf -w /path/to/wordlist -u http://target/FUZZ -mc 200
```

<!-- **Adding classical header (some WAF bypass)**
```
ffuf -c -w "/opt/host/main.txt:FILE" -H "X-Originating-IP: 127.0.0.1, X-Forwarded-For: 127.0.0.1, X-Remote-IP: 127.0.0.1, X-Remote-Addr: 127.0.0.1, X-Client-IP: 127.0.0.1" -fs 5682,0 -u https://target/FUZZ
``` -->

---

## wfuzz
**Web application fuzzer**

enumerate directories and files

options :
- -c : show only the status code

## dirb
**Web Content Scanner**

Find directories and files on websites

options :
- -r : recursive
- -z : use a wordlist



## Exploitation

## Privilege Escalation

## Post-exploitation
