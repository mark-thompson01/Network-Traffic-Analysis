# Network-Traffic-Analysis

## Introduction
In the realm of cybersecurity, understanding how to monitor and analyze network traffic is a fundamental skill. This lab serves as a practical demonstration of tcpdump and Wireshark, two indispensable tools for network analysis. This lab is designed to showcase their capabilities through a controlled creative scenario. The following outlines capturing, saving, and analyzing network traffic, with an emphasis on how these tools apply to real-world cybersecurity challenges.  

The setup consists of a small, virtualized network consisting of two virtual machines (VMs): a Kali Linux VM for capturing and analyzing traffic, and a Ubuntu Linux VM. This setup allows to simulate diverse traffic scenarios and provides meaningful data for analysis.  

## Lab Objectives
The primary goals of this lab are: 

1. To introduce key tcpdump commands and demonstrate how to capture specific types of traffic.  

2. To save captured traffic to a file for further inspection using Wireshark.  

3. To generate and examine diverse network traffic, including simulated malicious activity.

## Lab Setup
The lab environment consists of two interconnected VMs:

> **Kali Linux VM:** Used for capturing traffic with tcpdump and analyzing it with Wireshark.   
> **Ubuntu Linux VM:** Hosts an SSH server and HTTP Python server on port 80. 

These machines are connected via a virtual network to ensure controlled and reproducible results. 


## What is tcpdump?
Tcpdump is a command-line tool used to capture and analyze network traffic. It acts metaphorically as a powerful magnifying glass, allowing us to filter and observe data packets as they are traversing through a network medium. Tcpdump is a highly customizable command and forms the backbone of many network analysis workflows, particularly in cybersecurity. It’s versatility and ability to save traffic for later analysis make it invaluable for professionals working to secure computer networks. 

## Capturing Traffic with tcpdump
With traffic flowing, it's time to capture it using tcpdump:

- **Basic Capture**:
  
  **tcpdump -i eth0**
  
Captures all traffic on the specified network interface. 

- **Filter Captiure**:
  
  **tcpdump -i eth0 port 80**
  
Captures HTTP traffic only, focusing on web interactions.

- **Advanced Filters**:
  
  **tcpdump -i eth0 src 10.38.1.113 and dst port 22**
  
Captures SSH traffic originating from the Kali VM to the Ubuntu VM.


## Generating Network Traffic
To produce traffic for analysis, traffic is realistically simulated and varied network interactions:

1. **Ping Sweeps and Network Scans:**
   > Conduct a ping sweep or a network scan from Kali using tools like fping to identify active hosts. This generates ARP, ICMP, and SYN packets.

![arp-scan fping and nmap sn.PNG](Images/arp-scan%20fping%20and%20nmap%20sn.PNG)

![Ubuntu Port Scan.PNG](Images/Ubuntu%20Port%20Scan.PNG)



2. **File Transfers via SSH:**
   > Establish an SSH session from Kali to Ubuntu and use scp to transfer files. This simulates secure file transfer.

![SSH Login.PNG](Images/SSH%20Login.PNG)

SSH connection from Kali VM to Ubuntu VM successful. Before proceeding further, I'll create a test file on the Kali VM and then transfer it to the Ubuntu VM. 

![Test File Creation and Transfer to Ubuntu.PNG](Images/Test%20File%20Creation%20and%20Transfer%20to%20Ubuntu.PNG)

File transfer verification on Ubuntu VM. 

![File Transfer Verification.PNG](Images/File%20Transfer%20Verification.PNG)


3. **Simulated Malicious Activity:**
   > Use tools like hping3 on Kali to create malformed or suspicious packets, mimicking attack patterns.

**hping flood on port 80**

![hping port 80 flood.PNG](Images/hping%20port%2080%20flood.PNG)

**hping icmp flood**

![hping icmp flood 116.PNG](Images/hping%20icmp%20flood%20116.PNG)

**hping fragmented packets**

![hping Fragmented Packets 116.PNG](Images/hping%20Fragmented%20Packets%20116.PNG)

**hping data injection**

![hping data injection.PNG](Images/hping%20data%20injection.PNG)

**hping spoofed IP source**

![hping spoofed IP source 116.PNG](Images/hping%20spoofed%20IP%20source%20116.PNG)




   
## Saving Captures for Analysis
To preserve captured traffic for analysis with Wireshark, use the -w option:
tcpdump -i eth0 -w network_capture.pcap
This saves the traffic to a PCAP file, which can be loaded into Wireshark for detailed inspection.

![tcpdump capture start 2.PNG](Images/tcpdump%20capture%20start%202.PNG)

## Analyzing Traffic with Wireshark
Wireshark allows for in-depth examination of network data:

1. **Load the Capture:** Open the network_capture.pcap file in Wireshark

2. **Apply Filters:** Use Wireshark's powerful filtering capabilities to isolate specific types of traffic, such as HTTP, ICMP, or SSH.
   
3. **Identify Patterns and Anomalies**:
   - Examine HTTP requests and responses to analyze web activity.
   - Investigate SSH traffic for unauthorized access attempts.
   - Detect unusual packet structures indicative of malicious behavior.

Let's further examine the network_capture.pcap file. 

The commands ran in this lab produce various traffic types on computer networks. In this lab, I've captured the various traffic types to simulate reconnaissance, attacks, and legitimate activity. In this section, the most interesting findings will be analyzed. 

### Reconnaissance Traffic

- **ARP-Scan Traffic**

![Lots of ARP.PNG](Images/Lots%20of%20ARP.PNG)

![ARP Request.PNG](Images/ARP%20Request.PNG)

![ARP Reply.PNG](Images/ARP%20Reply.PNG)



- **Ping Sweep Traffic**

![ICMP.png](Images/ICMP.png)



- **Nmap Scan Traffic**

![Screenshot from 2024-12-13 23-20-00.png](Images/Screenshot%20from%202024-12-13%2023-20-00.png)

Port 22 SYN, ACK

![Port 22 Open.png](Images/Port%2022%20Open.png)

Port 80 SYN, ACK

![Port 80 Open.png](Images/Port%2080%20Open.png)

- **SSH Traffic**

![SSH Filter.png](Images/SSH%20Filter.png)


If we right click on the first SSH packet and select Follow > TCP Stream, we have the TCP Threeway Handshake for the SSH connection. 

![SSH Connection TCP Handshake.png](Images/SSH%20Connection%20TCP%20Handshake.png)


SSH Traffic Details

![SSH Traffic Details 1.png](Images/SSH%20Traffic%20Details%201.png)

![SSH Traffic Details 2.png](Images/SSH%20Traffic%20Details%202.png)

![SSH Traffic Details 3.png](Images/SSH%20Traffic%20Details%203.png)

![SSH Traffic Details 4.png](Images/SSH%20Traffic%20Details%204.png)


Encrypted Traffic Related to SCP File Transfer. 

Here we have the consistent traffic pattern of Encrypted Packet Client/Server communication over the SSHv2 protocol. 

The sequence and frequency of packets align with the behavior of a file transfer, where data is segmented into chucks and set over the SSH connection. 

The varying packet lengths (len=44, len=628, etc.) indicate data transfer activity. Larget packets suggest chucks of the file being sent, while smaller packets include control messages and acknowledgements. 


![SCP File Transfer.png](Images/SCP%20File%20Transfer.png)

In addition to that the packets of this traffic have the TCP ACK and PSH flags set, which are commonly seen during active data transfer. 

![ACK PSH.png](Images/ACK%20PSH.png)


- **TCP SYN Flood on Port 80**

![TCP SYN Flood on port 80.png](Images/TCP%20SYN%20Flood%20on%20port%2080.png)


- **Malformed Packets**

![Malform0.PNG](Images/Malform0.PNG)

![Capture 1.PNG](Images/Capture%201.PNG)


- **Spoofed Traffic**

![Spoofed Traffic.png](Images/Spoofed%20Traffic.png)











  

 ## Real World Application
 This lab demonstrates how tcpdump and Wireshark can be used in a professional setting to monitor, diagnose, and secure network environments. From identifying potential intrusions to analyzing legitimate traffic patterns, these tools provide cybersecurity professionals with critical visibility into network activity. 






















