# Network-Traffic-Analysis

## Introduction
In the realm of cybersecurity, understanding how to monitor and analyze network traffic is a fundamental skill. This lab serves as a practical demonstration of tcpdump and Wireshark, two indispensable tools for network analysis. This lab is designed to showcase their capabilities through a controller creative scenario. The following outlines capturing, saving, and analyzing network traffic, with an emphasis on how these tools apply to real-world cybersecurity challenges.  

The setup consists of a small, virtualized network consisting of three virtual machines (VMs): a Kali Linux VM for capturing and analyzing traffic, a Windows 10 VM, and an Ubuntu Linux VM hosting network services. This setup allows to simulate diverse traffic scenarios and provides meaningful data for analysis.  

## What is tcpdump?
Tcpdump is a command-line tool used to capture and analyze network traffic. It acts metaphorically as a powerful magnifying glass, allowing us to filter and observe data packets as they are traversing through a network medium. Tcpdump is a highly customizable command and forms the backbone of many network analysis workflows, particularly in cybersecurity. It’s versatility and ability to save traffic for later analysis make it invaluable for professionals working to secure computer networks. 

## Lab Objectives
The primary goals of this lab are: 

1. To introduce key tcpdump commands and demonstrate how to capture specific types of traffic.  

2. To save captured traffic to a file for further inspection using Wireshark.  

3. To analyze captured traffic and uncover insights relevant to cybersecurity scenarios.  

4. To generate and examine diverse network traffic, including simulated malicious activity.

## Lab Setup
The lab environment consists of three interconnected VMs:

> Kali Linux VM: Used for capturing traffic with tcpdump and analyzing it with Wireshark.  
> Windows 10 VM: Generates user activity such as web browsing and file transfers.  
> Ubuntu Linux VM: Hosts an HTTP server and other network services for interaction.


