# Forensics Cheatsheet

## File Analysis

```bash
# Basic file info
file <file>
exiftool <file>
strings <file>
strings -n 10 <file>      # Strings with min 10 chars

# Hex dump
xxd <file> | head
hexdump -C <file> | head

# Find hidden strings
strings <file> | grep -i flag
strings <file> | grep -i ctf
grep -a "flag" <file>
```

## Image Forensics

### Metadata & Hidden Data

```bash
# Metadata
exiftool image.jpg

# Strings in image
strings image.jpg | grep -i flag

# Check for appended data
binwalk image.jpg

# Extract hidden files
binwalk -e image.jpg
foremost image.jpg

# Steganography
steghide extract -sf image.jpg
steghide extract -sf image.jpg -p ""     # Empty password
stegseek image.jpg rockyou.txt           # Brute force
zsteg image.png                          # PNG/BMP
```

### Image Manipulation

```bash
# ImageMagick
identify image.png
convert image.png -channel RGB -separate output_%d.png

# Adjust levels/contrast (GIMP or online)
# stegsolve.jar - Check different color planes
```

## Audio Forensics

```bash
# Spectrogram (Audacity or Sonic Visualizer)
# Hidden images in spectrogram

# Morse code
# SSTV (Slow Scan TV) - use SSTV decoder

# Check for hidden data
strings audio.wav
binwalk audio.wav
```

## PDF Analysis

```bash
# Extract text
pdftotext file.pdf

# Metadata
exiftool file.pdf
pdfinfo file.pdf

# Hidden content
strings file.pdf
pdf-parser.py file.pdf
qpdf --qdf file.pdf output.pdf

# JavaScript extraction
pdf-parser.py --search javascript file.pdf
```

## Memory Forensics (Volatility)

```bash
# Identify profile
volatility -f memory.dmp imageinfo

# Common commands
volatility -f memory.dmp --profile=<profile> pslist
volatility -f memory.dmp --profile=<profile> pstree
volatility -f memory.dmp --profile=<profile> cmdline
volatility -f memory.dmp --profile=<profile> filescan
volatility -f memory.dmp --profile=<profile> dumpfiles -Q <offset> -D output/
volatility -f memory.dmp --profile=<profile> hashdump

# Network
volatility -f memory.dmp --profile=<profile> netscan
volatility -f memory.dmp --profile=<profile> connscan

# Registry
volatility -f memory.dmp --profile=<profile> hivelist
```

## Disk Forensics

```bash
# Mount disk image
sudo mount -o loop disk.img /mnt/disk

# Autopsy (GUI tool)
autopsy

# Sleuth Kit
fls disk.img
icat disk.img <inode>
mmls disk.img

# Deleted files
photorec disk.img
testdisk disk.img
```

## Network Forensics (PCAP)

```bash
# Wireshark filters
http
tcp.port == 80
ip.addr == 192.168.1.1
http.request.method == "POST"
frame contains "flag"

# Extract files
# File > Export Objects > HTTP

# tshark (command line)
tshark -r capture.pcap -Y "http" -T fields -e http.file_data

# Extract TCP streams
tcpflow -r capture.pcap

# Strings from pcap
strings capture.pcap | grep -i flag
```

## ZIP/Archive Analysis

```bash
# List contents
unzip -l file.zip
7z l file.zip

# Extract
unzip file.zip
7z x file.zip

# Password cracking
fcrackzip -u -D -p rockyou.txt file.zip
zip2john file.zip > hash.txt
john hash.txt --wordlist=rockyou.txt

# Fix corrupted ZIP
zip -FF file.zip --out fixed.zip
```

## Useful Tools

| Tool | Purpose |
|------|---------|
| binwalk | Analyze and extract embedded files |
| foremost | File carving |
| steghide | Image steganography |
| zsteg | PNG/BMP steganography |
| Volatility | Memory analysis |
| Wireshark | Packet analysis |
| Autopsy | Disk forensics |
| exiftool | Metadata extraction |

## File Signatures (Magic Bytes)

| Signature | File Type |
|-----------|-----------|
| `89 50 4E 47` | PNG |
| `FF D8 FF` | JPEG |
| `47 49 46 38` | GIF |
| `50 4B 03 04` | ZIP/DOCX/XLSX |
| `25 50 44 46` | PDF |
| `7F 45 4C 46` | ELF |
| `4D 5A` | EXE/DLL |
| `52 61 72 21` | RAR |
