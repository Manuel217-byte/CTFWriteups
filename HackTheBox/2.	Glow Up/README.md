I am given a file called artifacts.js. So this is a forensics challenge. When I opened the file, I noticed that some of it was in Base64.

<img width="468" height="85" alt="image" src="https://github.com/user-attachments/assets/ece2a7c7-dd02-449b-85ab-14ef7d421cb1" />

I entered one of the texts into CyberChef. For the recipe I have to set it to from Base64. Next, I need to pick a conversion. I started with XOR since that is a common operator. 

<img width="468" height="256" alt="image" src="https://github.com/user-attachments/assets/f9c26757-c945-41f1-854f-e260f3947331" />

I got a link to a website. 

When I converted another text from the file and used Base64 and XOR I got the flag.

<img width="1406" height="768" alt="image" src="https://github.com/user-attachments/assets/b160ed68-92a2-431c-9e15-0e9764c2393e" />

Risk Mitigation strategy

To mitigate this anti-virus and endpoint protection is necessary. Since this was a malware from a phishing attack. An anti-virus can prevent any malicious or unauthorized downloads. Since this malware does connect to another network DNS filtering and firewall rules for any untrusted websites can be used. Since this was phishing also spread user awareness and training. 
