# Wireshark Packet Analysis
## Summary:
### This project demonstrates the fundamentals of network traffic analysis using Wireshark. It contains sample captures, screenshots, and step-by-step notes that show how to filter, follow streams, and interpret common protocols.

#### First of all, we will open the .pcap file in Wireshark. Here it is:
<img width="1919" height="1018" alt="pcap_file" src="https://github.com/user-attachments/assets/48e0e568-6684-4ce9-a944-ae45597fd70b" />

#### We notice that the attacker is trying to access the FTP service:
<img width="1919" height="1017" alt="ftp_requests" src="https://github.com/user-attachments/assets/3065c0f2-b484-4a53-bfc7-6872d048730e" />

#### Following the TCP stream of a suspicious packet, we can access the entire conversation between the attacker and service:

<img width="1919" height="1018" alt="follow_tcp_stream" src="https://github.com/user-attachments/assets/b6dc7c09-eaed-4634-9516-0e1c94af1d3c" />
<img width="1302" height="686" alt="trying_to_log_in" src="https://github.com/user-attachments/assets/8893a268-9f7e-4e29-9ed8-b714884d3883" />
