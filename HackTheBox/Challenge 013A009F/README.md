Target: Challenge 013A009F
Artifact Analyzed: chase.pcapng

Step 1: Artifact Extraction

When starting the HTB challenge I am meet with a challenge zip archive. Which contained a packet capture file named chase.pcapng. It was opened in Wireshark.

<img width="468" height="352" alt="image" src="https://github.com/user-attachments/assets/fa4d2556-c9fd-44cb-9061-70134a0f6690" />

Step 2: Stream and Protocol Analysis

The capture revealed TCP and HTTP communication between 22.22.22.7 and 22.22.22.5

Following the TCP stream on a packet revealed that the attacker ran commands like ipconfig and cd /.

<img width="325" height="243" alt="image" src="https://github.com/user-attachments/assets/e8f4f304-446d-4091-82d0-b7cd42dbb245" />

Step 3: Traffic Filtering and Artifact Hunting

To find attacker's actions, reverse shells or anything uploaded this filter was applied:
tcp.port == 4444 or tcp.port == 1337 or tcp.port == 9001

This confirmed active reverse connection traffic

I used this filter: frame contains "exe" or frame contains "php" or frame contains "sh"
To find requests interacting with upload.aspx, execution via web shell cmd.aspx, and the download of nc64.exe.

Step 4: HTTP Request Isolation and Anomaly Detection

I filtered for outbound and inbound web requests. 
http.request

One of the requests said GET /JBKEE62NIFXF60DMOUZV6NZTMFGV6URQMNMH2IBA.txt HTTP/1.1

<img width="468" height="352" alt="image" src="https://github.com/user-attachments/assets/4eeb4b66-197d-409c-9797-56b478909ffa" />

When entering JBKEE62NIFXF60DMOUZV6NZTMFGV6URQMNMH2IBA into cyberchef and used the MAgic/From Base 32 recipe I decoded it into the flag

<img width="1358" height="858" alt="image" src="https://github.com/user-attachments/assets/4827475f-68fb-4e65-a9d7-3393b7c888c0" />
