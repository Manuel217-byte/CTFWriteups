Target: Machine 013A009B
Target IP: 10.129.185.152

Step 1: Network Recon

I used Nmap to detect running services.

nmap 10.129.185.152

3 ports were discovered to be open:
  21/tcp: FTP (vsFTPd 3.0.3)
  22/tcp: SSH
  80/tcp: HTTP

Step 2: Service Enumeration

I attempted to access the FTP server using anonymous credentials. But I got a 530 Login incorrect

ftp 10.129.185.152

Next I did Web investigation. I entered the target IP 10.129.185.152 into the web browser. This showed a security dashboard with the user account Nathan.

<img width="373" height="228" alt="image" src="https://github.com/user-attachments/assets/97955ece-181e-4281-a00d-cd1277eb8585" />

I explored the website. By changing the pathname of [http://10.129.185.152/data/2] to [http://10.129.185.152/data/0]. Which lead to a download of a pcap file.

Step 3: Traffic Analysis 

I filtered the stream for FTP packets. Then inspected the plaintext between 192.168.196.1 and 192.168.196.10. Which revealed the USER nathan, PASS Buck3tHATFLOR4, 230 Login successful.

<img width="405" height="246" alt="image" src="https://github.com/user-attachments/assets/8588181a-3324-4041-807c-00c5bbec4702" />

Step 4: Logging In

I used the credentials to ssh into my target. 
ssh nathan@10.129.185.152

I navigated to his home directory and found the flag.
cd /home/nathan
cat user.txt

<img width="1358" height="820" alt="image" src="https://github.com/user-attachments/assets/6b72113d-3f7a-4cc2-99ba-6c9ae25f3e0f" />
