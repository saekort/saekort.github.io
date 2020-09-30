## Nmap

NMap is *the* portscanning tool.

### Nmap basics

Start by scanning basic ports, detect services and operating system. Output in all formats and give verbose feedback.

```bash
nmap -sC -sV -v -O -oA <output filename> <IP-range>
```

Immediately open up a second console and run the same, but on all ports. This can finish while you examine the output from the first scan. The parameter to add is `-p-`.

### Nmap scripting engine

The Nmap Scripting Engine (NSE) saves its scripts in: `/usr/share/nmap/scripts`.

Basic vulnerability checking scripts:

```bash
nmap -v <IP-RANGE> --script vuln
```
