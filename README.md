**Enumeration (Walkthrough)**

To start my lab, I accessed the Metasploitable 2 terminal and executed the "ifconfig" command. This allowed me to identify the target machine's network configuration specifically its IPv4 address, which is 192.168.0.223. I also noted the IPv6 link-local address as fe80::a00:27ff:fe94:ba85, ensuring I had the correct target details for both network protocols before proceeding with further enumeration.

<img width="732" height="408" alt="1ifconfig" src="https://github.com/user-attachments/assets/185fe3db-f373-44c0-b463-9873b74ace27" />

**Challange 1 - NetBIOS Enumeration**

I used the command "nbtstat -a 192.168.0.223" in the Windows command prompt to perform NetBIOS enumeration. This command retrieved the NetBIOS Remote Machine Name Table for the target confirming the hostname as METASPLOITABLE and the workgroup as WORKGROUP.

<img width="500" height="450" alt="Challenge 1 - nbtstat" src="https://github.com/user-attachments/assets/5aa6b01f-5000-4088-8cc5-998b4231813f" />

**Challenge 2 - Fast Nmap Scan**

Next, I switched to my Windows machine and used the "nbtstat -a 192.168.0.223" command to perform NetBIOS enumeration on the target. This allowed me to view the NetBIOS Remote Machine Name Table, which confirmed the hostname as METASPLOITABLE and the workgroup as WORKGROUP.

<img width="756" height="625" alt="Challenge 2 - nmap" src="https://github.com/user-attachments/assets/dbf8a914-ecb1-4267-8bd8-b7669aacade1" />

**Challenge 3 - DNS Records**

I used three different commands to perform a comprehensive DNS enumeration of the domain roblox.com. First, I used "nslookup" to perform a basic query, which returned the primary IP address for the domain. Second, I used the "dig ANY" command to retrieve all available DNS records in a single query, providing a broader view of the domain's configuration. Finally, I executed the "dig MX" command specifically to identify the Mail Exchange records, which revealed the mail servers responsible for receiving email on behalf of the domain.

<img width="292" height="171" alt="Challenge 3 - Nslookup" src="https://github.com/user-attachments/assets/141bee18-c9c9-492c-9626-28b60d3f2475" />

<img width="607" height="407" alt="Challenge 3 - dig ANY" src="https://github.com/user-attachments/assets/7fa45e5b-a24a-4d2f-a8e2-a4844b869e7e" />

<img width="660" height="698" alt="Challenge 3 - dig MX" src="https://github.com/user-attachments/assets/ba03d660-8081-48f5-9e71-0de6e64189bc" />

**Challenge 5 - TTL OS Fingerprinting**

I performed basic network connectivity testing and initial OS fingerprinting by using the "ping 192.168.0.223" command from my Kali Linux terminal. The successful ICMP replies confirmed the target was reachable and I analyzed the Time to Live (TTL) value of 64. This specific TTL value is a characteristic indicator that the target machine is running a Linux/Unix operating system

<img width="597" height="628" alt="Challenge 5 - OS Fingerprinting" src="https://github.com/user-attachments/assets/900efd30-a22e-4d02-9c24-13b4212fa895" />

**Challenge 9 - FTP Banner**

I used the Netcat command "nc 192.168.0.223 21" to connect to the target's FTP port. Upon connection, the server immediately returned a service banner identifying itself as vsFTPd 2.3.4.

<img width="230" height="82" alt="Challange 9 - FTP Banner" src="https://github.com/user-attachments/assets/a5e5e6ff-d04f-4b84-bf9e-f87fa106960e" />

**Challenge 10 - Anonymous FTP Login**

I attempted an anonymous login to the target at "192.168.0.223" by using the "ftp" command. I successfully logged in using the username anonymous with no password, which confirmed that the service allows unauthenticated access. Once connected, I executed the "ls -al" command to list the directory contents, verifying that I could interact with the remote file system. This discovery highlights a significant security misconfiguration as it allows any user on the network to potentially view or download files from the server.

<img width="525" height="306" alt="Challenge 10 - FTP" src="https://github.com/user-attachments/assets/d3e0dde9-e0cb-4ef2-8b12-154a2212207c" />

**Challenge 11 - SMB NSE Enumeration**

I first ran the "nmap --script smb-os-discovery -p445 192.168.0.223" command to identify the operating system, which confirmed the target is running Samba 3.0.20 on a Debian system. I then used the "nmap --script smb-enum-users -p445 192.168.0.223" command to extract a complete list of local user accounts, such as root, postfix, and postgres, which provided the account names and their associated RIDs.

<img width="657" height="432" alt="Challenge 11 - SMB NSE Enumeration-OS" src="https://github.com/user-attachments/assets/6ddf3a48-22e5-40a9-a0e8-0dad6d7afaa6" />

<img width="725" height="842" alt="Challenge 11 - SMB NSE Enumeration-Users1" src="https://github.com/user-attachments/assets/05c64951-37ca-41a9-8f7e-c9513945cae9" />

<img width="551" height="833" alt="Challenge 11 - SMB NSE Enumeration-Users2" src="https://github.com/user-attachments/assets/6b9f5b9d-1645-4f54-8c5a-6c419a9ac04a" />

**Challenge 16 - Version Detection**

I used the command "nmap -sV 192.168.0.223" to perform service version detection on all open ports. This allowed me to identify the exact software and version numbers for various services such as vsftpd 2.3.4 on port 21, Apache httpd 2.2.8 on port 80 and Samba smbd 3.X.

<img width="997" height="645" alt="Challenge 16 - Version Detection" src="https://github.com/user-attachments/assets/6cbb92f6-0b7e-44d8-b5c2-7f945bfd7346" />

**Challenge 19 - RPC Info**

I used the command "rpcinfo -p 192.168.0.223" to enumerate the Remote Procedure Call (RPC) services on the target. This command provided a list of all RPC programs registered with the portmapper including nfs, mountd and portmapper itself along with their associated port numbers and protocols.

<img width="382" height="465" alt="Challenge 19 - RPC Info" src="https://github.com/user-attachments/assets/d380989f-d4a8-449f-8081-3d883c80ab44" />

**Challenge 27 - IPv6 Discovery**

I used the command "nmap -6 -O fe80::a00:27ff:fe94:ba85" to perform a scan and OS detection over the IPv6 protocol. This command confirmed that the target is active on the IPv6 network and identified its operating system as Linux 2.6.X.

<img width="752" height="390" alt="Challenge 27 - IPv6 Discovery" src="https://github.com/user-attachments/assets/8491340d-4519-4886-99ae-1cb403aa7f72" />

**Challenge 29 - SMTP Enumeration via Nmap**

I executed "nmap -p25 --script=smtp-enum-users 192.168.0.223" to attempt to list valid users on the server though the command returned an unhandled status code for the RCPT method. Next, I ran "nmap -p25 --script=smtp-open-relay 192.168.0.223" to check if the server could be used to send unauthorized emails. The results confirmed that the server is not an open relay, indicating that this specific mail vulnerability is not present on the target.

<img width="722" height="495" alt="Challenge 29 - SMTP Enumeration via Nmap" src="https://github.com/user-attachments/assets/ada3a975-5054-49af-b559-4d2548a2d54e" />
