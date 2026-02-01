# OSINT Cheatsheet

## Search Engines

### Google Dorks

```
site:example.com                    # Specific site
filetype:pdf                        # File type
inurl:admin                         # URL contains
intitle:"index of"                  # Title contains
"exact phrase"                      # Exact match
-exclude                            # Exclude term
cache:example.com                   # Cached version

# Combine
site:github.com "password" filetype:txt
site:pastebin.com "api key"
```

### Other Search Engines

```
# Bing, DuckDuckGo, Yandex
# Shodan (IoT/servers)
shodan.io

# Censys
censys.io

# Wayback Machine
web.archive.org
```

## Social Media

### Username Search

```bash
# Check username across platforms
namechk.com
namecheckr.com
whatsmyname.app
sherlock (tool)
```

```bash
# Sherlock
sherlock username
```

### Platform-Specific

```
# Twitter/X
twitter.com/search
# Use advanced search operators

# Instagram
# Check followers, following, tagged locations

# LinkedIn
# Company employees, job titles

# Facebook
# Graph search, groups, events
```

## Domain & IP

```bash
# WHOIS
whois example.com
whois 1.2.3.4

# DNS records
dig example.com ANY
nslookup example.com
host example.com

# Subdomain enumeration
sublist3r -d example.com
amass enum -d example.com

# Reverse IP
# Find other domains on same IP
viewdns.info/reverseip/
```

## Email OSINT

```bash
# Email validation
hunter.io
emailrep.io

# Data breaches
haveibeenpwned.com

# Email headers
# Analyze for origin, path, timestamps
```

## Image OSINT

```bash
# Reverse image search
images.google.com
tineye.com
yandex.com/images

# EXIF data
exiftool image.jpg

# GPS coordinates
# Convert to location
```

### Image Analysis

```
# Look for:
- Visible text/signs
- Landmarks
- Weather conditions
- Time of day (shadows)
- Language on signs
- Vehicle plates
- Clothing brands
```

## Geolocation

```bash
# Coordinates lookup
google.com/maps
openstreetmap.org

# Street View
google.com/maps?layer=c

# Historical imagery
Google Earth Pro

# What3Words
what3words.com
```

## Website Analysis

```bash
# Technology stack
wappalyzer.com
builtwith.com

# Archive
web.archive.org

# SSL certificate
crt.sh

# Robots.txt & sitemap
example.com/robots.txt
example.com/sitemap.xml
```

## Document Metadata

```bash
# PDF, Office docs
exiftool document.pdf
# Look for: author, creation date, software used

# Online
foca (tool)
```

## Username Patterns

```
# Common formats
firstname.lastname
firstnamelastname
f.lastname
firstname_lastname
lastname.firstname
```

## Phone Numbers

```
# Lookup
truecaller.com
whitepages.com
numverify.com

# Format: include country code
+1 555-123-4567
```

## Tools

| Tool | Purpose |
|------|---------|
| Sherlock | Username search |
| theHarvester | Emails, subdomains |
| Maltego | Visual link analysis |
| Recon-ng | Reconnaissance framework |
| SpiderFoot | Automated OSINT |
| Shodan | IoT/server search |
| Wayback Machine | Historical websites |

## Common CTF OSINT Tasks

```
1. Find person from limited info
2. Identify location from image
3. Track username across platforms
4. Find hidden info in website history
5. Decode coordinates
6. Analyze social media for clues
```

## Quick Checklist

- [ ] Google the exact text/name
- [ ] Reverse image search
- [ ] Check EXIF data
- [ ] WHOIS lookup
- [ ] Wayback Machine
- [ ] Social media search
- [ ] Username enumeration
- [ ] Check source code/robots.txt
