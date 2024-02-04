---
tags:
  - programming/hacking/general
---

# What resource do you have?

Everyone has to Google. Deal with it. This is where Google comes in.

## Typical Research Question

You have JPEG image that might have some extra data in. How to figure this out? 

1. Google basic question - find specific name
2. Use specific name to find way to implement it
3. Implement it and answer question

In the case of the hidden JPEG data, we find we ant to use `steghide` program and install it

```bash
    sudo apt-get install steghid
    steghid info $file
    steghide extract -sf $file
```

## Useful links for finding exploits


ExploitDB: https://www.exploit-db.com/
NVD: https://nvd.nist.gov/vuln/search
CVE Mitre: https://cve.mitre.org/
