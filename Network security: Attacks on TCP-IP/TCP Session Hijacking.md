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

