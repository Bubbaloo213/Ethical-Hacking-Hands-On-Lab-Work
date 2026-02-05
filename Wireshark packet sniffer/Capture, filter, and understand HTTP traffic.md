# Capture, filter, and understand HTTP traffic in Wireshark

## Basic HTTP GET/Response Interaction

### Start Wireshark Capture

1. Start Wireshark (don't start capture yet)
2. Enter display filter: http (lowercase, no quotes). This filter is to show only HTTP messages after capture
3. Select **Capture** > **Options** > Select on the active interface (Ethernet0) > **Start** Wireshark packet capture
  
  <img width="964" height="612" alt="image" src="https://github.com/user-attachments/assets/365e2f5b-2ae2-4cdc-bb02-7319049cd26b" />

### Make HTTP Request

4. Open a terminal
