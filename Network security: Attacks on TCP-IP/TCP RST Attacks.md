# TCP RST Attacks on telnet Connections

The objective of this exercise is to create one forged TCP RST (Reset) packet that looks like it came from the legitimate Telnet client. The goal is to make the existing Telnet connection close.

## Assumptions
- In this lab, we have three machines
      - 1 attacker machine: seed-attacker (10.9.0.1)
      - 1 victim machine: IP Address - 10.9.0.5
      - 1 user machine: IP address - 10.9.0.6
- We use containers to set up the lab environment
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

9) Narrow it further. Once you see traffic, use:

  ```javascript
  ip.src == 10.9.0.6 && ip.dst == 10.9.0.5 && tcp.port == 23
  ```

10) d
