# Lab6-LianyuTHM

## Step 1: Reconnaissance

Start by scanning the target machine to identify open ports and services.

```bash
nmap -sV -sC 10.49.143.115
```
<img width="771" height="527" alt="lianyu1" src="https://github.com/user-attachments/assets/208daeaf-3166-4b2e-9f63-adf8d6fef6ff" />

Findings :
- Port 21 → FTP
- Port 22 → SSH
- Port 80 → HTTP
- Port 111 → RPC

## Step 2 — Quick check of FTP (always try anonymous)
- Because FTP was open, I tried anonymous login first — it’s cheap and often yields hints or files.
<img width="303" height="148" alt="image" src="https://github.com/user-attachments/assets/665ea903-0750-416e-b57e-2a8ea6392661" />
- Outcome: anonymous login failed

##  Step 3 — Web Enumeration
Visiting http://10.49.143.115 . I found a page titled ARROWVERSE with a big blurb about Oliver Queen / Lian Yu (background lore). This was visible content but didn’t contain the flag itself.
