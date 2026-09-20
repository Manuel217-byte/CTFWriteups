Target: Machine 014300A0
Target IP: 10.129.176.163

Step 1: Network Recon

Did a full range port scan using Nmap

nmap -sS -p- 10.129.176.163

ports 22/tcp:SSH and 80/tcp:HTTP were open.

Step 2: Web application

Went to [http://10.129.176.163/]. It lead to a e-commerce platform named BeautyStylers

The homepage showed a promotional notice. I need to purchase face powder for $0 to get the flag. Use code BEAUTYFRIDAY for 20% discount.

<img width="468" height="297" alt="image" src="https://github.com/user-attachments/assets/1c5a4546-5e72-4dcf-9ed8-04e008130eb7" />

I added face powder to the cart and went to checkout (/cart.php)

Step 3: Parameter Tampering 

Intercepted /cart.php traffic in Burp Suite

Increased the quantity of face powder to 5 items so $100 in total. Applied the discount code BEAUTYFRIDAY which gave a $20 discount.

Intercepted each item removal request and modified the parameter to recalc_discount=0 before forwarding.

By overriding the recalc parameter the $20 discount stays locked in while reducing to 1 Face Powder. Dropping the final price to $0.

<img width="468" height="297" alt="image" src="https://github.com/user-attachments/assets/01f852fd-c712-4042-9345-a1492a3fb9ec" />

Step 4: Flag retrieval

Filled out the payment info. Submitted the order with $0 total. The server processed it and gave me the flag.

<img width="1736" height="1080" alt="image" src="https://github.com/user-attachments/assets/da08142e-6dc2-42bf-befd-2dc69cd3b054" />

Risk mitigation strategy:

Implement strict input validation and sanitization for user inputs and HTTP parameters. Use server-side verification of prices and total amount. The price calculations were done on client side. So, you can validate the cart price with a server elsewhere. Also use a Web Application Firewall rules to detect and abnormal request behaviors that show signs of attempted malicious activity. 
