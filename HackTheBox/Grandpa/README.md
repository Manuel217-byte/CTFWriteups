Target IP: 10.129.164.136

Target Machine: Granpa

Status Incomplete

Step 1: Port Scanning

I did a standard nmap scan on the target.

nmap 10.129.164.136

I got no response.

So I went with a aggressive scan that bypassed host discovery and scanned all ports. 

nmap -Pn -sS -p- --min-rate=1000 10.129.164.136

Found 80/tcp open running HTTP

Step 2: Web Server Fingerprinting

Went to [http://10.129.164.136]. It lead to a IIS Under Construction page.

<img width="468" height="277" alt="image" src="https://github.com/user-attachments/assets/57e7e4fe-3ea6-44a0-8a71-609eb4fdf62b" />

ran curl -v and whatweb to see any server information

I found that the web server is Microsoft-IIS/6.0 (Windows Server 2003) and framework is MicrosoftOfficeWebServer 5.0, ASP.NET

Step 3: Weaponization

I searched Metasploit for  known IIS 6.0 remote exploits and selected the ScStoragePathFromUrl WebDAV buffer overflow module

<img width="468" height="275" alt="image" src="https://github.com/user-attachments/assets/9bea2590-a31a-46d2-a7e6-94523025b36f" />

I payload executed successfully. Opening a Meterpreter session.

Step 4: Host Enumeration

Commands such as getuid and sysinfo failed due to restrictive token sandboxing.

I dropped to Windows command prompt and did whoami. Current identity being nt authority\network service

<img width="468" height="286" alt="image" src="https://github.com/user-attachments/assets/e0fdc2ce-7f3d-4eae-bcec-e5d913ea2d42" />

I enumerated local accounts using net users.

Found Administrator, ASPNET, Guest, IUSR_GRANPA, IWAM_GRANPA, Lakis, SUPPORT_388945a0.

Checked the profile Lakis

I attempted to enter the directory with cd "C:\Documents and Settings\Lakis"
But access is denied. Which meant that nt authority\network service does not have read permissions over profiles

I was unable to finish this CTF. However in order to naviate to Lakis I would need to do a privilege escalation. I would use a privilege escalation exploit in Metasploit and try token impersonation to steal Lakis token. then cd Desktop.
