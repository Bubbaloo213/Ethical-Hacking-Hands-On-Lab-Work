# Analyzing Captured Traffic

The objective of this exercise is to know how an attacker can use Wireshark to steal private data from the unencrypted packets intercepted in an ARP spoofing attack. This is to proof why it's so important to ensure that your web traffic is encrypted using HTTPs. 

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
3. We will pretend to be the victim and generate packets by opening the Firefox browser and entering the  Metasploitable Web Server's IP address into the address bar.
   <br />
   <img width="552" height="489" alt="image" src="https://github.com/user-attachments/assets/63531a94-c3e4-413b-bff9-7022e4e5d269" />
   <br />
4. Within Wireshark, click the red stop icon to end the capture.
   <br />
  <img width="1276" height="133" alt="image" src="https://github.com/user-attachments/assets/c12fb8d8-673e-4eb7-85de-becbbb67c339" />
   <br />

## Analyzing Captured Traffic

5. Within Wireshark, filter so that you only view the packets that have been sent to the Metasploitable web server.
   <br />
   <img width="1290" height="645" alt="image" src="https://github.com/user-attachments/assets/b9d1f015-a7bd-44af-aa9d-c3a70d735bd0" />
   <br />

## Identify the TCP packets trnasmitted between the Kali Linux Machine and the Metasploitable web server.
   
6. Righ-click one of the packets with the desitnation address of the Metasploitable web server and select **Conversation Filter > TCP**.
   <br />
   <img width="1087" height="518" alt="image" src="https://github.com/user-attachments/assets/46edc403-595c-429c-8c27-ebc1d7dcf6f1" />
   <br />

   <details>
  <summary> ** Why are there so many packets if all we did was load a single web page? ** </summary>
              This happens because the server breaks the web page into smaller pieces and then transmits them as spearate packets if a file is too large to be transmitted in a single packet. The recipient will reassemble these packets to recover the original file.  

</details>

## Wireshark reassembles the packets from a packet stream and recover the original file. 
7. Click a packet and select **Follow > TCP Stream**
   <br />
  <img width="1075" height="482" alt="image" src="https://github.com/user-attachments/assets/3e15de2d-dd8e-4e60-8304-588293f6f3dc" />
   <br />
8. You should see the HTML corresponding to the page.
   <br />
   <img width="1280" height="641" alt="image" src="https://github.com/user-attachments/assets/1c0d34b0-6f5d-4f82-b86b-8afaf251424b" />
   <br />
