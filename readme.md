# Tekam's cheatsheet about pentesting

# Information Gathering


## Manual Discovery

---

### Common pages

- /robots.txt - list of files that are not indexed by search engines
- /sitemap.xml - list of pages on the website

---

### Google Dorks

- site:example.com - search for a specific website address
- intitle:example - search for a specific word in the title
- inurl:example - search for a specific word in the url
- filetype:pdf - search for a specific file type

### Wappalyzer

- https://www.wappalyzer.com/ - browser extension that detects technologies used on websites

---

## Automated Discovery

---

### FFUF - Fast Web Fuzzer

- Directory discovery - 

```
ffuf -w /usr/share/wordlists/dirb/common.txt -u http://example.com/FUZZ
```

ffuf -w /path/to/wordlist -u https://target/FUZZ


## Enumeration/Scanning

## Exploitation

## Privilege Escalation

## Post-exploitation
