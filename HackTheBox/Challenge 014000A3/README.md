Target: Challenge 014000A3
Target IP: 83.136.249.53:43488

Step 1: Reconnaissance

Did a port scan on 83.136.249.53 with nmap

nmap -p 43488 83.136.249.53

I discovered that port 43488/tcp is open

I went to [http://83.136.249.53:43488/login] which showed a Member Login portal

<img width="468" height="349" alt="image" src="https://github.com/user-attachments/assets/e540312b-d560-4a62-b00f-449fdff10d1c" />

SQL Injection attempts were unsuccessful. But the page does have a option to register a new user.

Step 2: Account Creation

I registered a new user with username:hackerman123 and password:123

When signing in with this new account I get the message that I am not an admin.

<img width="468" height="298" alt="image" src="https://github.com/user-attachments/assets/2166a2cd-34aa-4430-9c7b-354b26cc4c83" />

I opened developer tools in the browser to find stored session cookies and found a base64 encoded cookie string.

Cookie: eyJ1c2VybmFtZSI6ImhhY2tlcm1hbjEyMyJ9

Step 3: Cookie Manipulation

I decoded the cookie in the terminal using base64 -d. It revealed the JSON formatting.

{"username": "hackerman123"}

I forged an administrative session be crafting a JSON object telling the user is admin and base64 encoding it:

echo -n '{"username": "admin"}' | base64
eyJ1c2VybmFtZSI6ICJhZG1pbiJ9

<img width="468" height="297" alt="image" src="https://github.com/user-attachments/assets/8d8796fb-edd2-44ec-8597-d0e4d9d64c0d" />

I replaced the existing cookie in developer tools with the new one eyJ1c2VybmFtZSI6ICJhZG1pbiJ9 and refreshed the page.

Step 4:

When visiting the refreshed dashboard the flag is revealed.

<img width="1738" height="1084" alt="image" src="https://github.com/user-attachments/assets/791c7b0c-00ae-4bf1-bcee-bffa1000694f" />


