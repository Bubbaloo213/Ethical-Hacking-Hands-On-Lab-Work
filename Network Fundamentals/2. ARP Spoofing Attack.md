# ARP Spoofing Attack

The objective of this exercise is to perform ARP spoofing, a technique where an attacker sends false ARP (Address Resolution Protocol) messages over a network. This tricks devices into associating the attacker's MAC address with the IP address of another device (like a router), allowing the attacker to intercept or manipulate traffic.

## Obtain the Metasploitable machine's (Victim Machine) IP and MAC address

### 1. Identify the IP addresses of the other machines on the network to ***spoof*** them. Open the terminal on the Kali Linux machine and enter the following command:

  **kali@kali:~$**
  ```javascript
  sudo netdiscover
  ```

  ***Example***

  <img width="485" height="125" alt="image" src="https://github.com/user-attachments/assets/af97bca2-3335-4f9b-a596-a6e617111892" />


* **What is Netdiscover?:** Netdiscover works by scanning the network using ARP queries. It issues ARP queries for all possible IP addresses on the subnetwork, and when a machine on the network respons, it records and displays the machine's MAC address and IP address.

## Enable IP forwarding on a Kali Linux machine, allowing it to forward packets on behalf of other machines.

### 2. Enter the following command:

  **kali@kali:~$**
  ```javascript
  echo 1 > /proc/sys/net/ipv4/ip_forward
  ```

***Example***

<img width="359" height="111" alt="image" src="https://github.com/user-attachments/assets/04daebc3-24b3-4c8e-b6e5-4c6268d40a7a" />


* **Why Enable IP Forwarding?:** By enabling IP forwarding, your Kali Linux machine (Attacker machine) can act as a router, allowing it to forward packets between different networks.

## Trick the victim into believing you're the router.

### 3. Issue fake ARP replies stating that your MAC address maps to the router's IP address (192.168.1.1) by running the following command

  **kali@kali:~$**
  ```javascript
  arpspoof -i eth0 -t <VICTIM_IP> <ROUTER_IP>
  ```

* **Note:** <VICTIM_IP> is 192.168.1.100 and <ROUTER_IP> is 192.168.1.1

<img width="348" height="176" alt="image" src="https://github.com/user-attachments/assets/7e33163b-5d6c-43c7-bae4-b48809717fac" />

* Here the Attacker's machine is asking the Victim Machine to update it's ARP table and match the Router's IP address (192.168.1.1) with its MAC Address.
* **EXAMINING THE COMMAND'S OUTPUT**
  - 8:0:27:1f:30:76: MAC address of the Kali Linux machine (attacker), which created the packet
  - 8:0:27:fe:31:e6: MAC address of the machine (victim) that will receive the packet
  - 0806: type field indicating that an ARP packet is contained within the Ethernet frame being transmitted
  - 42: represents the total number of bytes associated with the Ethernet frame
  - arp reply 192.168.100.1 is-at 8:0:27:1f:30:76: summary of the ARP reply that falsely states that the routher's IP address (192.168.100.1) is associated with the Kali Linux machine's (attacker machine) MAC address (8:0:27:1f:30:76)

## Trick the router into believing you're the victim to intercept incoming internet traffic on the victim's behalf.

### 4. Issue fake ARP replies stating that your MAC address maps to the victim's IP address (192.168.1.100) by running the following command:

  **kali@kali:~$**
  ```javascript
  arpspoof -i eth0 -t <ROUTER_IP> <VICTIM_IP> 
  ```

* **Note:**  <ROUTER_IP> is 192.168.1.1 and <VICTIM_IP> is 192.168.1.100

<img width="369" height="216" alt="image" src="https://github.com/user-attachments/assets/2ae9cf0c-e84a-4f84-89d3-db9489812d86" />

* Here the Attacker's machine is asking the Router to update it's ARP table and match the Victim's IP address (192.168.1.100) with its MAC Address.

* Both the victim and router have been spoofed.

* **What is dsniff?:** dsniff is a collection of tools used for network traffic interception and analysis. It is commonly used by network administrators for legitimate purposes, such as monitoring network traffic, but it can also be misused for malicious activities.
* What is arpspoof?: One of the tools included in dsniff is called arpspoof. This tool is specifically designed to perform an ARP spoofing attack, which can be used to intercept network traffic between devices on a local area network (LAN).

## Inspect the packets we've intercepted and extract URLs from them.

### 5. Extract the URLs by running the following command:

  **kali@kali:~$**
  ```javascript
  urlsnarf -i eth0
  ```

***Example: Kali Linux VM (Attacker Machine)***
<img width="498" height="35" alt="image" src="https://github.com/user-attachments/assets/35ad01ba-3e08-4162-86a4-fed55ddbb887" />


* This will allow us to generate a list of websites that the victim visits. 

## Log to Metasploitable virtual machine (Victim Machine)

### 6. Search for a website by running the following command:

  **kali@kali:~$**
  ```javascript
  wget http://www.google.com
  ```

***Example: Metasploitable VM (Victim Machine)***
<img width="739" height="431" alt="image" src="https://github.com/user-attachments/assets/79612364-7f85-46ee-83ea-2f59e80ae741" />

## Return to Kali Linux VM (Attacker Machine)

### 7. The URL associated with the web request will show up in the terminal after a couple of minutes.

<img width="869" height="89" alt="image" src="https://github.com/user-attachments/assets/e18ffb33-1efd-4d59-b10e-f6e801ab4386" />

* This is proof that the attacking machine is capturing all of the packets the victim sends and receives from the internet.
