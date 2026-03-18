[CTF Writeup.md](https://github.com/user-attachments/files/26096779/CTF.Writeup.md)
# Lab Environment

in this lab environment, I was provided with GUI access to a Kali Linux machine. The target machine was accessible at **http://target.ine.local**.

**Objective:** Perform reconnaissance on the target and capture all the flags hidden within the environment.

- **Flag 1**: The server proudly announces its identity in every response. Look closely; you might find something unusual.

from the task we already know the DNS  http://target.ine.local, still we will apply the command **ifconfig** to display the current configurations. 
in previous labs, Alexis Ahmed mentioned **eth1** is usually the primary source we will find the target.
<img width="1919" height="906" alt="Move1" src="https://github.com/user-attachments/assets/026c185a-680c-4db5-95ed-2dc76bd9bbfb" />


We check the available hosts using the subnet **/24** because our **netmask** is **255.255.255.0**
<img width="783" height="545" alt="Move2" src="https://github.com/user-attachments/assets/36568b16-d4d5-452e-a17e-5096ab91be09" />

Having the information summarized up, we will go for **Passive Reconnaissance Phase**
By simply using **whatweb target.ine.local** we will receive FLAG1.

<img width="1887" height="219" alt="Move3" src="https://github.com/user-attachments/assets/f6e68a8a-4dd3-44f9-9101-75ad26a253c0" />

- **Flag 2**: The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.

Firstly, we should check what are we not allowed to see (by google indexing)
After accessing **target.ine.local/robots.txt** we discovered 4 disallowed subdomains
**/secret-info/ and /data/ seem interesting**
<img width="1098" height="383" alt="Move1" src="https://github.com/user-attachments/assets/bf1a05bb-303c-4ca2-9a3f-339627af350b" />

<img width="854" height="262" alt="Move2" src="https://github.com/user-attachments/assets/13c6c71d-7397-438f-9c64-3d11750c2761" />

Seems to be a .txt file. I will try a new approach and use **HTTrack** to be able to inspect each file

<img width="839" height="181" alt="Move3" src="https://github.com/user-attachments/assets/ba86ee58-cf3d-4c73-8cb2-41ba07572a8c" />

<img width="1804" height="422" alt="Move4" src="https://github.com/user-attachments/assets/80982ca4-35b0-4b75-a7ff-e8f59587f75d" />

After downloading the website, I realized that I did not use  **+ * .txt** to be able to download the disallowed subdomains.

<img width="913" height="223" alt="Move5" src="https://github.com/user-attachments/assets/304626cb-3a36-4e64-8d8f-1ca81d2de787" />

Now we are able to open the **robots.txt** with success using **cat** command to verify the components.

<img width="623" height="274" alt="Move6" src="https://github.com/user-attachments/assets/8e9d19f7-6b82-472c-bfff-805a1a01a325" />

With a short approach on **Curl** tool, we managed to open **flag.txt** file revealing FLAG2.

- **Flag 3**: Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.

Just by reading the task I could tell it is about **connecting on FTP.**
Firstly, we will check the open ports by using (also mentioned in tools section)

<img width="1386" height="474" alt="Move1" src="https://github.com/user-attachments/assets/2a4b9e66-6b7d-4a4c-a2d3-8156bda03338" />

We can see that port 21 is open, we will inspect deeper with **nmap -A -p21 target.ine.local**

<img width="1701" height="704" alt="Move2" src="https://github.com/user-attachments/assets/23e0b5a6-44f3-4f8b-9af8-1f00878466b8" />

Bingo! We revealed 2 .txt files. We will connect by using **ftp target.ine.local** command

<img width="1721" height="536" alt="Move3" src="https://github.com/user-attachments/assets/2c19206d-c1a7-4e86-b9f9-397c2b5f9c5c" />

For username we type **anonymous** and press ENTER for **password**.
We grab the **.txt** files by using **get creds.txt** and **get flag.txt**.

<img width="1895" height="897" alt="Move4" src="https://github.com/user-attachments/assets/d57c875e-7d74-423a-8d64-4836d53165d3" />

I used **ls -lh flag.txt creds.txt flag.txt** command to see where it is located in the virtual machine. 
After opening **flag.txt** we received FLAG3.

- **Flag 4**: A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

We saw **creds.txt** from the previous task which will help us find FLAG4.

<img width="662" height="333" alt="Move1" src="https://github.com/user-attachments/assets/d0be866a-5fc4-46ef-bd56-3e173ecf0a0a" />


We discovered the **mysql port earlier running on  3306**.

<img width="1782" height="912" alt="Move2" src="https://github.com/user-attachments/assets/fbeed4fe-83fc-4c99-955e-fd07162a6e44" />

After successfully connecting inside the **database** we will use the **show databases;** command to list them.


<img width="613" height="245" alt="Move3" src="https://github.com/user-attachments/assets/9590adce-5721-4923-86ef-846ab87ae350" />


Another succesful attempt! We revealed FLAG4.
