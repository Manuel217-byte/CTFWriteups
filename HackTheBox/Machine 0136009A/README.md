Target: Machine 0136009A
Target IP: 10.129.175.203

Step 1: Web Application Enumeration

Went to [http://10.129.175.203/] which loaded a landing page.

<img width="468" height="289" alt="image" src="https://github.com/user-attachments/assets/3a4129ab-8668-4951-9396-b20d10d09a67" />

I began proxying web traffic through Burp Suite. From there I found /assets/contact.php which had name, email, and message parameters.

<img width="468" height="298" alt="image" src="https://github.com/user-attachments/assets/0963c191-fcfe-4fe9-8be9-c5deef92d3cb" />

I used Burp Repeater with SQL injection payloads but it returned 400 Bad Request.

Step 2: FTP Service Enumeration

I went to check on network services and checked FTP on the target IP

ftp 10.129.175.203

successfully logged in using anonymous account

<img width="468" height="381" alt="image" src="https://github.com/user-attachments/assets/af24b0c0-d1c8-4424-8eaa-7c709519bd96" />

I downloaded the two files allowed.userlist and allowed.userlist.passwd

Step 3: Web Login

With the downloaded lists I used Hydra to get the valid username and password

hydra -L allowed.userlist -P allowed.userlist.passwd 10.129.175.203 \
http-post-form "/login.php:username=^USER^&password=^PASS^:<failure-condition>"

Step 4: Logging In

After finding out the correct username and password  I logged in [http://10.129.175.203/dashboard/index.php] and was the flag was displayed.

<img width="1358" height="1006" alt="image" src="https://github.com/user-attachments/assets/7133d13f-cf82-490c-aa0e-bcfe96ad8d83" />

Risk mitigation 

Implement input validation and sanitization to prevent injection attacks. Deploy a Web Application Firewall (WAF) to detect and block suspicious requests. Secure FTP configurations by disabling anonymous logins, enforcing stringent password policies, and regularly updating the FTP server software. Enforce HTTPS for encrypted traffic between clients and servers. Conduct penetration tests and vulnerability assessments to proactively identify and mitigate security gaps. Implement real-time monitoring and alert systems for prompt detection of unusual activities. 


