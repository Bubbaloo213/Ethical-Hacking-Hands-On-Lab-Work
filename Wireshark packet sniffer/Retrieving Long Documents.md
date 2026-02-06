# Retrieving Long Documents

The objective of this exercise is to analyze how when you send a request or receive a response using HTTP, that data might be too large to fit into a single TCP segment. So, TCP splits the data into smaller segments to send it over the network.

When Wireshark sees that a single HTTP response is too large for one TCP segment, it marks those segments as “TCP segment of a reassembled PDU” (Protocol Data Unit). This means that Wireshark recognizes that these segments belong to the same larger message.

<details>
  <summary> ** What is HTTP? ** </summary>
              HTTP (Hypertext Transfer Protocol) is a protocol used for transferring data over the web. When you visit a website, your browser uses HTTP to request and receive data (like HTML files, images, etc.).

</details>

<details>
  <summary> ** What is TCP? ** </summary>
              TCP (Transmission Control Protocol) is a protocol that ensures reliable communication between computers over a network. It breaks down data into smaller pieces (called segments) for transmission.

</details>

## Capture Large File Transfer

1) Clear previous capture and start Wireshark as new
2) Run

## Answer the Below Questions

* How many HTTP GET request messages did your host send? **ANSWER:** 1

<img width="1661" height="180" alt="image" src="https://github.com/user-attachments/assets/98be432b-3329-44e4-8b7d-1824b54fa91c" />

* Which packet number in the trace contains the GET message for the Bill or Rights? **ANSWER:** 21

  <img width="1661" height="793" alt="image" src="https://github.com/user-attachments/assets/8cc10a83-cdd0-4f09-bc74-8682f97ba5e3" />

* Which packet number in the trace contains the status code and phrase associated with the response to the HTTP GET request? **ANSWER:** 27

  <img width="1661" height="796" alt="image" src="https://github.com/user-attachments/assets/de79cb6f-e2ec-497d-83a3-b31318171df9" />

* What is the status code and Phrase in the response?  Do not include \r\n in your answer.

* How many data-containing TCP segments were needed to carry the single HTTP response and the text of the Bill of Rights?
