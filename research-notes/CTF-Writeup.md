# Lab Environment

In this lab environment, I was provided with GUI access to a Kali Linux machine. The target machine was accessible at **http://target.ine.local**.

**Objective:** Perform reconnaissance on the target and capture all the flags hidden within the environment.

---

### Flag 1
**Task:** *The server proudly announces its identity in every response. Look closely; you might find something unusual.*

From the task, the DNS **http://target.ine.local** is already known. First, I applied the `ifconfig` command to display the current configurations. Following the methodology taught by Alexis Ahmed, **eth1** was identified as the primary interface for the target network.

<img width="1919" height="906" alt="Move1" src="https://github.com/user-attachments/assets/026c185a-680c-4db5-95ed-2dc76bd9bbfb" />

I checked the available hosts using the **/24** subnet because the netmask is **255.255.255.0**.

<img width="783" height="545" alt="Move2" src="https://github.com/user-attachments/assets/36568b16-d4d5-452e-a17e-5096ab91be09" />

**Exploitation:**
By using the `whatweb target.ine.local` command, I retrieved the server headers and received **Flag 1**.

<img width="1887" height="219" alt="Move3" src="https://github.com/user-attachments/assets/f6e68a8a-4dd3-44f9-9101-75ad26a253c0" />

---

### Flag 2
**Task:** *The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.*

First, I checked the `robots.txt` file at **target.ine.local/robots.txt** and discovered 4 disallowed subdomains. The directories `/secret-info/` and `/data/` appeared highly interesting.

<img width="1098" height="383" alt="Move1" src="https://github.com/user-attachments/assets/bf1a05bb-303c-4ca2-9a3f-339627af350b" />

<img width="854" height="262" alt="Move2" src="https://github.com/user-attachments/assets/13c6c71d-7397-438f-9c64-3d11750c2761" />

I attempted a new approach using **HTTrack** to inspect each file. Initially, I realized that I did not use the `+ * .txt` filter to download the disallowed subdomains.

<img width="839" height="181" alt="Move3" src="https://github.com/user-attachments/assets/ba86ee58-cf3d-4c73-8cb2-41ba07572a8c" />

<img width="1804" height="422" alt="Move4" src="https://github.com/user-attachments/assets/80982ca4-35b0-4b75-a7ff-e8f59587f75d" />

<img width="913" height="223" alt="Move5" src="https://github.com/user-attachments/assets/304626cb-3a36-4e64-8d8f-1ca81d2de787" />

After correcting the filters and mirroring the site, I used the `cat` command to verify the components and `curl` to open the `flag.txt` file.

<img width="623" height="274" alt="Move6" src="https://github.com/user-attachments/assets/8e9d19f7-6b82-472c-bfff-805a1a01a325" />

**Result:** Successfully revealed **Flag 2**.

---

### Flag 3
**Task:** *Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.*

I identified that Port 21 (FTP) was open and inspected it further with the following command:
`nmap -A -p21 target.ine.local`

<img width="1386" height="474" alt="Move1" src="https://github.com/user-attachments/assets/2a4b9e66-6b7d-4a4c-a2d3-8156bda03338" />

<img width="1701" height="704" alt="Move2" src="https://github.com/user-attachments/assets/23e0b5a6-44f3-4f8b-9af8-1f00878466b8" />

**Exploitation:**
The scan confirmed that Anonymous access was enabled. I connected via `ftp`, logged in as `anonymous`, and used the `get` command to exfiltrate `creds.txt` and `flag.txt`.

<img width="1721" height="536" alt="Move3" src="https://github.com/user-attachments/assets/2c19206d-c1a7-4e86-b9f9-397c2b5f9c5c" />

<img width="1895" height="897" alt="Move4" src="https://github.com/user-attachments/assets/d57c875e-7d74-423a-8d64-4836d53165d3" />

**Result:** Successfully revealed **Flag 3**.

---

### Flag 4
**Task:** *A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.*

Using the `creds.txt` file obtained from the FTP server, I discovered administrative credentials. Since the earlier scan showed MySQL running on Port 3306, I attempted a connection.

<img width="662" height="333" alt="Move1" src="https://github.com/user-attachments/assets/d0be866a-5fc4-46ef-bd56-3e173ecf0a0a" />

<img width="1782" height="912" alt="Move2" src="https://github.com/user-attachments/assets/fbeed4fe-83fc-4c99-955e-fd07162a6e44" />

**Exploitation:**
After successfully connecting to the database using the discovered credentials, I executed the `show databases;` command to list the available data.

<img width="613" height="245" alt="Move3" src="https://github.com/user-attachments/assets/9590adce-5721-4923-86ef-846ab87ae350" />

**Result:** Successfully revealed **Flag 4**.
