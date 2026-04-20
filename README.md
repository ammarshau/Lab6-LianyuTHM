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
- Visiting http://10.49.143.115 . I found a page titled ARROWVERSE with a big blurb about Oliver Queen / Lian Yu (background lore). This was visible content but didn’t contain the flag itself.
<img width="933" height="943" alt="image" src="https://github.com/user-attachments/assets/8388fa57-11ba-4faf-897e-ce38356ed69b" />

## Step 4 — Directory enumeration with Gobuster
- moved to Port 80 (HTTP). Running Gobuster for directories revealed a hidden folder /island.

 ```bash
gobuster dir -u http://10.49.143.115/ -w /usr/share/wordlists/dirb/big.txt
```
<img width="682" height="393" alt="image" src="https://github.com/user-attachments/assets/b03ab5a7-fba6-4e82-a88e-227c660da583" />
THERE IS MORE SCREENSHOT, BUT I FORGOT, LATER I WILL PUT
##Step 5 — Continue bruteforcing discovered path
- Because /island exists, run Gobuster on /island (look deeper) — authors often nest secret files.
```bash
gobuster dir -u http://10.49.143.115/island/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
<img width="907" height="306" alt="image" src="https://github.com/user-attachments/assets/027987c8-64e7-408e-bfbc-7b042053d514" />
Result found ```bash /island/2100 ```
- Page-Source

ran Gobuster again with the file extension flag -x .ticket
```bash gobuster dir -u http://thm/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x .ticket -t 50 ```
<img width="938" height="325" alt="image" src="https://github.com/user-attachments/assets/3e42bae7-8d16-4c9f-9b44-d108e752fe90" />

Open: http://10.49.143.115/island/2100/green_arrow.ticket





























