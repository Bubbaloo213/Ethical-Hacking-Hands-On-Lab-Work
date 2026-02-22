# Analyzing Packets Collected by Your Firewall

The objective of this exercise is to use Wireshark and tcpdump to determine if your newtork is being hacked. 

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
  <br />
  
## Wireshark Packet Cature Process

** Note: These next steps are to be executed in the Kali Linux machine

2. Within Wireshark, start the packet capture process by clicking the icon.
   
   <br />
   <img width="1280" height="150" alt="image" src="https://github.com/user-attachments/assets/8a14ef55-f8f2-48ac-89ac-1e1d71735969" />
   <br />
