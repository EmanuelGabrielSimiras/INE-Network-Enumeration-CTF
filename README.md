INE eJPT Lab: Full Network Enumeration and Exploitation Walkthrough
In this lab environment, I was provided with GUI access to a Kali Linux machine. The target machine was accessible at http://target.ine.local.

Objective: Perform reconnaissance on the target and capture all the flags hidden within the environment.

Flag 1: System Identification
The server announces its identity in every response.

From the task, the DNS http://target.ine.local is already known. First, I applied the ifconfig command to display the current configurations. Following the methodology taught by Alexis Ahmed, eth1 was identified as the primary interface for the target network.

<img width="1919" height="906" alt="Move1" src="https://github.com/user-attachments/assets/189563eb-74b8-415b-8961-eae487d02457" />

I checked the available hosts using the /24 subnet because the netmask is 255.255.255.0.

<img width="783" height="545" alt="Move2" src="https://github.com/user-attachments/assets/5d917f23-019a-453d-a09a-04457a2de7fb" />

Exploitation:
By using the whatweb target.ine.local command, I retrieved the server headers and received Flag 1.

<img width="1887" height="219" alt="Move3" src="https://github.com/user-attachments/assets/1e055720-5a72-4408-9699-20919ac352de" />

Flag 2: Robots.txt and Information Leakage
The gatekeeper's instructions often reveal what should remain unseen.

First, I checked the robots.txt file at target.ine.local/robots.txt and discovered 4 disallowed subdomains. The directories /secret-info/ and /data/ appeared highly interesting.

<img width="1098" height="383" alt="Move1" src="https://github.com/user-attachments/assets/49bcd837-3721-4ca2-9a3f-339627af350b" />

<img width="854" height="262" alt="Move2" src="https://github.com/user-attachments/assets/8aed41a5-2c88-4e97-a7b3-3cbc2ba734b5" />

I attempted a new approach using HTTrack to inspect each file. Initially, I realized that I did not use the + * .txt filter to download the disallowed subdomains.

<img width="839" height="181" alt="Move3" src="https://github.com/user-attachments/assets/89d96c7f-981c-4c57-8939-b198a802dec8" />

<img width="1804" height="422" alt="Move4" src="https://github.com/user-attachments/assets/e075a11e-91e5-4ea2-893a-7b26612b191c" />

<img width="913" height="223" alt="Move5" src="https://github.com/user-attachments/assets/3c99ea18-7d0d-4ca8-925a-bf3c53109b8e" />

After correcting the filters and mirroring the site, I used the cat command to verify the components and curl to open the flag.txt file.

<img width="623" height="274" alt="Move6" src="https://github.com/user-attachments/assets/8c16613e-7652-4417-9be8-59e3ae1dfc55" />

Result: Successfully revealed Flag 2.

Flag 3: Anonymous FTP Access
Anonymous access sometimes leads to forgotten treasures.

I identified that Port 21 (FTP) was open and inspected it further with the following command:
nmap -A -p21 target.ine.local

<img width="1386" height="474" alt="Move1" src="https://github.com/user-attachments/assets/11839610-148b-4a7f-878c-f10dc596bd0f" />

<img width="1701" height="704" alt="Move2" src="https://github.com/user-attachments/assets/20004215-0c3e-4872-9c68-ea41b0a66e2b" />

Exploitation:
The scan confirmed that Anonymous access was enabled. I connected via ftp, logged in as anonymous, and used the get command to exfiltrate creds.txt and flag.txt. I used ls -lh to verify their location on the machine.

<img width="1721" height="536" alt="Move3" src="https://github.com/user-attachments/assets/6c22ad3a-d8fc-4dfb-b78f-be5d4f39b9e2" />

<img width="1895" height="897" alt="Move4" src="https://github.com/user-attachments/assets/84176d9d-784e-4a9b-9dfe-9f9504aec8f4" />

Result: Successfully revealed Flag 3.

Flag 4: Database Compromise and Credential Reuse
A well-named database can be revealing.

Using the creds.txt file obtained from the FTP server, I discovered administrative credentials. Since the earlier scan showed MySQL/MariaDB running on Port 3306, I attempted a connection.

<img width="662" height="333" alt="Move1" src="https://github.com/user-attachments/assets/faec187b-df83-47ac-b336-d5d181eaf058" />

<img width="1782" height="912" alt="Move2" src="https://github.com/user-attachments/assets/9d7095f5-4184-4621-944f-d756d94e5875" />

Exploitation:
After successfully connecting to the database using the discovered credentials, I executed the show databases; command to list the available data.

<img width="613" height="245" alt="Move3" src="https://github.com/user-attachments/assets/6b496994-c362-4493-89fa-ccb8b08eb9f5" />

Result: Successfully revealed Flag 4.

<img width="809" height="522" alt="Achievement" src="https://github.com/user-attachments/assets/ef20642a-ae1a-42b3-8467-051257cbf537" />

Tools Used
Network Discovery: ifconfig, nmap

Web Analysis: whatweb, curl, HTTrack

Exfiltration: ftp

Database Auditing: mysql-client

Additional Resources

The original research notes and structured documentation for this lab were managed using Obsidian. For more details, you can explore the **[Obsidian Research Notes](./research-notes/CTF-Writeup.md)**.

