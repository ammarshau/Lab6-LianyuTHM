# Lab6-LianyuTHM

## Step 1: Reconnaissance

Start by scanning the target machine to identify open ports and services.

```bash
nmap -sC -sV 10.49.143.115
```
<img width="771" height="527" alt="lianyu1" src="https://github.com/user-attachments/assets/208daeaf-3166-4b2e-9f63-adf8d6fef6ff" />

Findings :
- Port 21 → FTP
- Port 22 → SSH
- Port 80 → HTTP
- Port 111 → RPC
