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
wfuzz -c -z list,admin,root http://target/FUZZ
```

---

## dirb
**Web Content Scanner**

Find directories and files on websites

options :
- -r : recursive
- -z : use a wordlist



## Exploitation

## Privilege Escalation

## Post-exploitation
