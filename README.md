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

 ```
gobuster dir -u http://10.49.143.115/ -w /usr/share/wordlists/dirb/big.txt
```

<img width="682" height="393" alt="image" src="https://github.com/user-attachments/assets/b03ab5a7-fba6-4e82-a88e-227c660da583" />


<img width="1100" height="812" alt="image" src="https://github.com/user-attachments/assets/3a2e09a3-278f-402c-9336-e4efcf1548a4" />


<img width="1100" height="410" alt="image" src="https://github.com/user-attachments/assets/035bcc3d-d6b6-4676-9d95-9e8e5087cb50" />


- noticed a hidden codeword: vigilante (same color as the background).


<img width="615" height="221" alt="image" src="https://github.com/user-attachments/assets/1c157226-dd3a-44a7-97a0-03a06d54bb53" />


## Step 5 — Continue bruteforcing discovered path
- Because /island exists, run Gobuster on /island (look deeper) — authors often nest secret files.


```
gobuster dir -u http://10.49.143.115/island/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```


<img width="907" height="306" alt="image" src="https://github.com/user-attachments/assets/027987c8-64e7-408e-bfbc-7b042053d514" />


Result found ```bash /island/2100 ```


<img width="1100" height="749" alt="image" src="https://github.com/user-attachments/assets/c5430a04-fae1-444e-94ec-13c5664df56f" />


- Page-Source


<img width="1100" height="750" alt="image" src="https://github.com/user-attachments/assets/e6dcc600-a405-403a-92cd-1e9296e88a4b" />


```
<!DOCTYPE html>
<html>
<body>

<h1 align=center>How Oliver Queen finds his way to Lian_Yu?</h1>


<p align=center >
<iframe width="640" height="480" src="https://www.youtube.com/embed/X8ZiFuW41yY">
</iframe> <p>
<!-- you can avail your .ticket here but how?   -->

</header>
</body>
</html>

```


## files with the .ticket extension exist in this directory.

ran Gobuster again with the file extension flag -x .ticket



``` gobuster dir -u http://thm/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x .ticket -t 50 ```



<img width="938" height="325" alt="image" src="https://github.com/user-attachments/assets/3e42bae7-8d16-4c9f-9b44-d108e752fe90" />


Open: http://10.49.143.115/island/2100/green_arrow.ticket


<img width="1100" height="747" alt="image" src="https://github.com/user-attachments/assets/5cc7b5d7-db35-4aa3-a2fe-512a57d38f36" />


```
This is just a token to get into Queen's Gambit(Ship)

RTy8yhBQdscX
```


## Step 6 — Decode the .ticket (Base58)


```
python3 - <<'PY'
import base58
print(base58.b58decode("RTy8yhBQdscX").decode())
PY
```


<img width="416" height="102" alt="image" src="https://github.com/user-attachments/assets/98c4bcbe-2d93-42c4-9952-1f526f09732b" />


## Step 7: FTP Access

- logged into the FTP using vigilante as our username and the password we found


```
ftp 10.49.143.115
```


```
username:vigilante
password: !#th3h00d
```


<img width="297" height="147" alt="image" src="https://github.com/user-attachments/assets/c2eade67-d220-4b8b-b09d-fb3343461d27" />

```
ls
```


Output

```
229 Entering Extended Passive Mode (|||46232|).                                                                     
150 Here comes the directory listing.                                                                               
-rw-r--r--    1 0        0          511720 May 01  2020 Leave_me_alone.png                                          
-rw-r--r--    1 0        0          549924 May 05  2020 Queen's_Gambit.png                                          
-rw-r--r--    1 0        0          191026 May 01  2020 aa.jpg                                                      
226 Directory send OK. 
```

get Queen's_Gambit.png 


<img width="922" height="277" alt="image" src="https://github.com/user-attachments/assets/ee41915f-4057-489a-8973-46363aa8077e" />


#ONLY 'GET' Queen's_Gambit.png BECAUSE EARLIER I ALREADY GET OTHERS FILES


```bash
exiftool info Leave_me_alone.png
```




<img width="487" height="252" alt="image" src="https://github.com/user-attachments/assets/4bad8732-ce62-4041-b238-bf5b5b734b62" />


<img width="942" height="638" alt="lianyu 2" src="https://github.com/user-attachments/assets/0194b576-cae6-427f-bf2b-837e10b800cf" />

- FIXED THE PNG HEADER BITS 


<img width="867" height="742" alt="lianyu4" src="https://github.com/user-attachments/assets/25cc8fff-8bc2-4900-a047-951f2e14d5a3" />

- Open again -- it revealed the hidden password

 Step 10 — Cracking aa.jpg
 
 - Try steghide with the password:
 - Enter passphrase: password
 - file named ss.zip was extracted
# I ALREADY DID EARLIER 

<img width="510" height="193" alt="lianyu5" src="https://github.com/user-attachments/assets/5f2629bb-419e-4031-a43d-909f160bb3b1" />

# IT LOOKS LIKE THIS BEFORE :

```
┌──(mar㉿kali)-[~]
└─$ unzip ss.zip 
Archive:  ss.zip
  inflating: passwd.txt              
  inflating: shado 
```

## Reading the Files

<img width="632" height="205" alt="lianyu7" src="https://github.com/user-attachments/assets/09f7ab6f-b4d0-44e2-a765-c6a25f3276c6" />

- tried to get the something with 'passwd.txt'


<img width="477" height="63" alt="lianyu6" src="https://github.com/user-attachments/assets/2904531b-d05d-4929-b100-255cacace311" />

- tried on shado
- look like a credentials for later use

## Gain access

<img width="536" height="331" alt="lianyu8" src="https://github.com/user-attachments/assets/de122220-8072-40a6-bc87-097902bd6174" />

- checked if there is other user than vigilante
- slade ✅


```ssh slade@thm```

<img width="808" height="561" alt="image" src="https://github.com/user-attachments/assets/87ccf988-fd26-4c38-9a17-229917fb87f3" />



``` ls ```


```cat user.txt ```


<img width="347" height="83" alt="lianyu10" src="https://github.com/user-attachments/assets/d7963220-40f8-4a49-9621-1f94cff59395" />





































