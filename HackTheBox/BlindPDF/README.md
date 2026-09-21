BlindPDF

Target IP: 94.237.58.4:49767

First thing I did was enter the ip into the search bar of a browser to visit the website.

I am meant with this:

<img width="468" height="280" alt="image" src="https://github.com/user-attachments/assets/8b208ee1-1f90-41f2-8c4d-9e02fff4d3eb" />

Since this is a website that converts html to generate PDFs. I would have to enter something into the input box that will give the flag. So, I have to craft JavaScript script that explicitly displays the file content. From the hint that was given it says to leak the flag with this /etc/passwd. 

Here is the code:

<img width="1448" height="786" alt="image" src="https://github.com/user-attachments/assets/1a81e0de-b87e-4acc-b6b6-e5ca8c16df15" />


When inputting the code it reveals the flag.

<img width="468" height="283" alt="image" src="https://github.com/user-attachments/assets/364ff21d-d200-4817-9e43-bdc32ca85e1e" />

Mitigation strategy:

Harden the PDF-rending engine. Keep up with security releases as some CVEs are already patched. Another mitigation strategy is whitelist and sanitizes user HTML. It could be configured to strip out any content that can be used to obtain information. Also, you can sandbox the process so that there is minimal privileges for the user. 
