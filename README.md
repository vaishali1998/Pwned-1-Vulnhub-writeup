# Pwned:1 ~Vulnhub Writeup
- VM name : Pwned
- Difficulty : Easy
- DHCP : Enabled
- Goal : 3 flags

## Scanning

First we use nmap for port scanning and other information gathering on target host.

**nmap IP_Address**

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled.png)

nmap -sV -A ip_address (Service version scanning)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%201.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%201.png)

nmap -sV -A --script vuln ip-address (Vulnerability Scanning)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%202.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%202.png)

## Enumeration

Open target in web browser.

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%203.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%203.png)

We found information about the hacker “Annlynn” in the body and commented lines of the source code.

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%204.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%204.png)

Lets scan target host using nikto  and we found directory /nothing

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%205.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%205.png)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%206.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%206.png)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%207.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%207.png)

We found nothing.

Let's use gobuster to retrieve more drectories and we fount directory **/hidden_text.**

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%208.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%208.png)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%209.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%209.png)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2010.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2010.png)

We found file secret.dic which contains directory listing. Only directory pwned.vuln is accessible

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2011.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2011.png)

In view-source we found that there is a condition in PHP with some credentials. The user already gave us the clue for which service to use (FTP).

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2012.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2012.png)

## Exploitation

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2013.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2013.png)

Using given credential we successfully connected to ftp server of target. and found two files note.txt and SSH Private key.

In note.txt we found one user "ariana"

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2014.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2014.png)

Downloading ssh private key 

get id_rsa

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2015.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2015.png)

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2016.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2016.png)

After that we gave 600 permission to ssh private key and tried to connect through SSH with the user “Ariana“.

ssh -i id_rsa ariana@target-ip

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2017.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2017.png)

We found two more files after successfully connected through ssh.

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2018.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2018.png)

We got our first flag user1.txt

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2019.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2019.png)

## Privilege escalation (selena)

We execute the command “sudo -l“ and found we are able to execute script messenger.sh

sudo -u selena /home/messenger.sh

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2020.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2020.png)

user: selena

message: /bin/bash

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2021.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2021.png)

We got shell of user selena and also second flag user2.txt

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2022.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2022.png)

## Privilege escalation (root)

python3 -c "import pty;pty.spawn('/bin/bash');"

Execute command "id" and we found user selena belong to group docker 

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2023.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2023.png)

we run command  **docker run -v /:/mnt --rm -it alpine chroot /mnt sh** to escalate privileges with a shell as root.

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2024.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2024.png)

Finally we got root shell and flag.

![Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2025.png](Pwned%20Vulnhub%20writeup%20d0d482b847ec4ff4be6dd526a92ba4d0/Untitled%2025.png)
