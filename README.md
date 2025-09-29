# Wireshark Packet Analysis
## Summary:
### This project demonstrates the fundamentals of network traffic analysis using Wireshark. It contains sample captures, screenshots, and step-by-step notes that show how to filter, and follow streams.

## Basic Analysis:

#### First of all, we will open a sample .pcap file in Wireshark. Here it is:
<img width="1919" height="1018" alt="pcap_file" src="https://github.com/user-attachments/assets/48e0e568-6684-4ce9-a944-ae45597fd70b" />

#### Scrolling through the .pcap file, we notice that the attacker is trying to access the FTP service:
<img width="1919" height="1017" alt="ftp_requests" src="https://github.com/user-attachments/assets/3065c0f2-b484-4a53-bfc7-6872d048730e" />

#### Following the TCP stream of a suspicious packet, we can access the entire conversation between the attacker and service:
<img width="1919" height="1018" alt="follow_tcp_stream" src="https://github.com/user-attachments/assets/b6dc7c09-eaed-4634-9516-0e1c94af1d3c" />
<img width="1302" height="686" alt="trying_to_log_in" src="https://github.com/user-attachments/assets/8893a268-9f7e-4e29-9ed8-b714884d3883" />

#### The attacker is trying to use different credentials to log in, but all the attempts are unsuccessful. Eventually, he managed to log in, as we found a packet mentioning "Login successful":
<img width="1919" height="1018" alt="login_successful" src="https://github.com/user-attachments/assets/bc442818-424d-49c2-90f3-157f0658163f" />

#### Using Follow -> TCP Stream on the relevant packet, we reviewed the client–service conversation:
<img width="1919" height="1023" alt="tcp_follow_login" src="https://github.com/user-attachments/assets/0b7f18a9-8c63-4d55-8cc9-3918460c05f1" />

#### The analysis shows that the password used was "password123." Please note that such passwords are considered weak and can be easily compromised through brute-force attacks.
<img width="1306" height="685" alt="correct_password" src="https://github.com/user-attachments/assets/f6e31d2e-8cb3-4c3d-8c67-67e5ddf87372" />

#### The current FTP working directory also can be figured out. We use ftp.response.code == 257 because 257 is the FTP reply code that returns the server's current working directory, usually in response to the PWD (Print Working Directory) command. This makes it a reliable filter to quickly identify and confirm the active directory during analysis.
#### So, the current working directory is "/var/www/html":
<img width="1919" height="1020" alt="active_ftp_directory" src="https://github.com/user-attachments/assets/40a66820-c3a9-405a-a3bf-b6a03b51144a" />

#### In cases where a backdoor was uploaded, the filename can often be determined from the packet capture. We use "ftp-data" as a filter because FTP separates control and data: the control channel sends commands like STOR <filename>, while the data channel carries the file contents. Inspecting ftp-data lets us verify the uploaded file and, together with the control channel, confirm its filename.
<img width="1918" height="703" alt="ftp-data_filter" src="https://github.com/user-attachments/assets/fc9d41ea-b6d2-4fa9-ae35-eacfe1489f6b" />

#### Eventually, two packets are displayed for analysis. The first packet does not contain the information we are seeking, as its line-based text data section includes only less relevant details.
<img width="1524" height="824" alt="first_packet_analysis" src="https://github.com/user-attachments/assets/107db914-1e9c-457f-8789-7fa5ffc0c363" />

#### In the packet info section of the second packet we see the command "STOR shell.php". The FTP command STOR is used to upload a file from the client to the server, specifying the filename to store on the server. So the filename of the uploaded backdoor is "shell.php". Also, through the details of the second packet, we find out the entire URL of the backdoor.
<img width="1523" height="821" alt="backdoor_url" src="https://github.com/user-attachments/assets/6a552257-6787-4eb5-93ca-abef9cc3b7ef" />

#### As we know the attacker's IP, we can check what commands he executed by applying the filter "ip.addr == 192.168.0.147 && tcp.port == 80". In such a way we isolate traffic between the attacker and the target server on the specific port used by the reverse shell, making it easier to analyze only the relevant conversation.
<img width="1919" height="1019" alt="checking_packets" src="https://github.com/user-attachments/assets/6050cd61-e44d-4da2-8a4e-d4697fb19f33" />

#### Following the TCP Stream we again are accessing the entire client-service conversation. Here are the commands the attacker used:
<img width="1296" height="675" alt="first_mannual_command" src="https://github.com/user-attachments/assets/d253d637-b399-4df9-88be-450cee03a0cd" />

#### In order to spawn a new TTY shell the attacker executed this command "python3 -c 'import pty; pty.spawn("/bin/bash")'". A TTY shell is an interactive shell attached to a pseudo‑terminal (pty) (e.g., /dev/pts/0) that provides full terminal semantics - line editing, job control (Ctrl‑Z/Ctrl‑C), and proper behavior for interactive programs.
<img width="1298" height="681" alt="command_tty_shell" src="https://github.com/user-attachments/assets/64e129f1-359c-433a-bb74-4ed163bd10cb" />

#### To see other commands used by attacker, we need to scroll and analyse the entire client-service exchange. For instance, the "sudo su" command was executed to gain a root shell, etc.

### Thank you for going through this Wireshark primer. It covers the basics of following streams, analyzing FTP/HTTP traffic, and extracting key information. There's much more to explore: advanced protocol analysis, decryption, and artifact extraction, etc. Mastering these fundamentals provides a solid foundation for further study.





