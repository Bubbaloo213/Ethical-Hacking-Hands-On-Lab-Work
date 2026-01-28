# Getting Started with Wireshark

The objective of this exercise is to gain a foundational understanding of the Wireshark packet sniffer, packet capture, and protocol analysis. 

## Understand how to capture packets using Wireshark

<details>
  <summary> ** What is Wireshark? ** </summary>
              Wireshark is a widely used open-source network protocol analyzer that allows users to capture and interactively browse the traffic running on a computer network.  It supports a wide range of protocols, allowing users to dissect packet details and understand their interactions. 

</details>

### To capture network traffic, select the correct network interface that is actively sending or receiving data.
- Select **Capture** tab > **options** where you'll see a list of avaialble network interfaces.
- Look for the interface that is currently in use. Since we're using a wired connection, 'Ethernet0' is the active interface.

<details>
  <summary>** What is a Network Interface? **</summary>
A hardware or software component that connects a device to a network, enabling communication with other devices. It serves as the point of interaction between the device and the network, allowing data to be sent and received. Network interfaces can be physical, such as Ethernet ports or Wi-Fi adapters, or virtual, such as those created by software for virtual machines or network simulations.

</details>

  <img width="1661" height="718" alt="image" src="https://github.com/user-attachments/assets/3a3747a3-9f0c-4aa7-8ee4-6e09817c0284" />

### Capture your first packets
- Start your web browser and let it display your homepage

  <img width="831" height="461" alt="image" src="https://github.com/user-attachments/assets/c300eb4f-f4c2-4ef7-a7f8-1722c2e505cf" />

- Return to Wireshark, select **Start**

  <img width="968" height="519" alt="image" src="https://github.com/user-attachments/assets/81faed6e-a95e-4a93-9c98-e87792040b44" />

## Generate HTTP traffic for analysis in Wireshark using the CURL command-line tool 

The goal is to create network traffic that can be easily analyzed, specifically focusing on HTTP rather than HTTPS.


  **kali@kali:~$**
  ```javascript
  curl -4 --http1.1 http://gaia.cs.umass.edu/wireshark-labs/INTRO-wireshark-file1.html
  ```


1) Open **Command Prompt** 

## Be familiar with basic filtering techniques and packet analysis.

## Comprehend the encapsulation of protocols (HTTP within TCP within IP within Ethernet/WiFi).

## Be capable of identifying different protocols in captured network traffic.
