# Analyzing Captured Traffic

## Manually obtain the IP address of the Metasploitable Web Server
** Note: These next steps are to be executed in the Metasploitable Web Server machine

1. Within command prompt, run the following commands:

<br />

  **msfadmin@metasploitable:~$**
  ```javascript
  ifconfig eth0
  ```
<br />
  <img width="720" height="200" alt="image" src="https://github.com/user-attachments/assets/30d519da-f3f2-4a76-a094-1b0f30f988a0" />

### Wireshark Packet Cature Process
** Note: These next steps are to be executed in the Kali Linux machine

2. We will prenet to be the victim and generate packets by opening the Firefox browser and tnering the  Metasploitable Web Server's IP address into the address bar.
