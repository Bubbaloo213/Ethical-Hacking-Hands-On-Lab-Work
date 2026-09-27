# Creating Reverse Shell using TCP Session Hijacking

The objective of the lab is to demonstrate how a TCP session hijacking attack can be used to establish a reverse shell on a victim machine. To learn how an attacker can inject a reverse-shell command into an existing TCP session and use it to gain remote shell access, allowing them to execute additional commands on the victim machine.

## Assumptions
- In this lab, we have three machines
      * 1 Attacker machine: seed-attacker (10.9.0.1)
      * 1 Victim machine: IP Address - 10.9.0.5
      * 1 User 1 machine: IP address - 10.9.0.6
      * 1 User 2 machine: IP address - 10.9.0.7
- We use containers to set up the lab environment
- The attacker can observe the TCP traffic between A and B.
- All these machines are on the same LAN

### Start the Netcat listener on the Attacker Machine

1) Inside the Attacker Machine, open a terminal and run the following command. The Attacker machine is listening on port 9090 and waiting for a TCP connection.

  ```javascript
  nc -lnv 9090
  ```

<img width="823" height="115" alt="image" src="https://github.com/user-attachments/assets/a6e7dedc-0a02-43e1-b90e-c17f182976cc" />

### Establish a fresh Telnet connection

2) Go to **User 1 (10.9.0.6)** and start Telnet with the **Victim machine (10.9.0.5)**
