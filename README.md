INE eJPT Lab: Full Network Enumeration and Exploitation Walkthrough
In this lab environment, I was provided with GUI access to a Kali Linux machine. The target machine was accessible at http://target.ine.local.

Objective: Perform reconnaissance on the target and capture all the flags hidden within the environment.

Flag 1: System Identification
The server announces its identity in every response.

From the task, the DNS http://target.ine.local is already known. First, I applied the ifconfig command to display the current configurations. Following the methodology taught by Alexis Ahmed, eth1 was identified as the primary interface for the target network.

<img width="1919" height="906" alt="Move1" src="https://github.com/user-attachments/assets/582dbc8c-3224-4651-b939-66217a5c7068" />

I checked the available hosts using the /24 subnet because the netmask is 255.255.255.0.

<img width="783" height="545" alt="Move2" src="https://github.com/user-attachments/assets/aac48114-9aab-488c-8f37-d8cb16ff70e5" />

Exploitation:
By using the whatweb target.ine.local command, I retrieved the server headers and received Flag 1.

<img width="1887" height="219" alt="Move3" src="https://github.com/user-attachments/assets/7d2c8647-e593-4d7f-8e5c-21d7bdcc5ade" />

Flag 2: Robots.txt and Information Leakage
The gatekeeper's instructions often reveal what should remain unseen.

First, I checked the robots.txt file at target.ine.local/robots.txt and discovered 4 disallowed subdomains. The directories /secret-info/ and /data/ appeared highly interesting.

<img width="1098" height="383" alt="Move1" src="https://github.com/user-attachments/assets/994b8b9d-c24a-42cc-8fb0-77c15d2b40e5" />

<img width="854" height="262" alt="Move2" src="https://github.com/user-attachments/assets/b4feceea-29c4-48f3-8a81-d0248592cf5f" />

I attempted a new approach using HTTrack to inspect each file. Initially, I realized that I did not use the + * .txt filter to download the disallowed subdomains.

<img width="839" height="181" alt="Move3" src="https://github.com/user-attachments/assets/dfbc19df-6fb0-491e-bc44-64c6c521a9c9" />

<img width="1804" height="422" alt="Move4" src="https://github.com/user-attachments/assets/1c6d1540-d0e1-4eb7-be2b-f22c8a8ccff7" />

<img width="913" height="223" alt="Move5" src="https://github.com/user-attachments/assets/f3d4d1d2-44f3-4cda-9a54-4a043a45a671" />

After correcting the filters and mirroring the site, I used the cat command to verify the components and curl to open the flag.txt file.

<img width="623" height="274" alt="Move6" src="https://github.com/user-attachments/assets/cd221e03-2f48-45ad-812a-6b6e63309942" />

Result: Successfully revealed Flag 2.

Flag 3: Anonymous FTP Access
Anonymous access sometimes leads to forgotten treasures.

I identified that Port 21 (FTP) was open and inspected it further with the following command:
nmap -A -p21 target.ine.local

<img width="1386" height="474" alt="Move1" src="https://github.com/user-attachments/assets/0c408f17-aa50-476f-a10f-168aac06898a" />

<img width="1701" height="704" alt="Move2" src="https://github.com/user-attachments/assets/62d2c312-26d8-4691-a05b-e1ae5336f46f" />

Exploitation:
The scan confirmed that Anonymous access was enabled. I connected via ftp, logged in as anonymous, and used the get command to exfiltrate creds.txt and flag.txt. I used ls -lh to verify their location on the machine.

<img width="1721" height="536" alt="Move3" src="https://github.com/user-attachments/assets/442d736e-cc36-46cd-a2e4-8e263af40385" />

<img width="1895" height="897" alt="Move4" src="https://github.com/user-attachments/assets/35949e0e-0259-429f-9f5e-5e4a8f84d5e0" />

Result: Successfully revealed Flag 3.

Flag 4: Database Compromise and Credential Reuse
A well-named database can be revealing.

Using the creds.txt file obtained from the FTP server, I discovered administrative credentials. Since the earlier scan showed MySQL/MariaDB running on Port 3306, I attempted a connection.

<img width="662" height="333" alt="Move1" src="https://github.com/user-attachments/assets/2ff5644b-5a7e-4a3e-ac88-7b4d9175971c" />

<img width="1782" height="912" alt="Move2" src="https://github.com/user-attachments/assets/aec2cb5c-4933-433c-84ea-2be95e786920" />

Exploitation:
After successfully connecting to the database using the discovered credentials, I executed the show databases; command to list the available data.

<img width="613" height="245" alt="Move3" src="https://github.com/user-attachments/assets/fbf36901-e372-47cc-90db-35ec5d665a4c" />

Result: Successfully revealed Flag 4.

<img width="809" height="522" alt="Achievement" src="https://github.com/user-attachments/assets/ef20642a-ae1a-42b3-8467-051257cbf537" />

Tools Used
Network Discovery: ifconfig, nmap

Web Analysis: whatweb, curl, HTTrack

Exfiltration: ftp

Database Auditing: mysql-client

Additional Resources

The original research notes and structured documentation for this lab were managed using Obsidian. For more details, you can explore the Obsidian Research Notes.
