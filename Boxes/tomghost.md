Target: 10.10.8.116


From our nmap scan, I found 3 ports open

<img width="605" height="188" alt="image" src="https://github.com/user-attachments/assets/c72520ea-9315-45fa-af0a-018976f65774" />


One of the port is 8009 running ajp13 which is the apache jserve protocol used to send reverse proxy to the backend application server. It has a file read vulnerability which allows an attacker to read the content of the ```WEB-INF/web.cml``` file on apache tomcat. I exploited this vulnerability using metasploit and I was able to get the credentials to login using ssh


<img width="605" height="378" alt="image" src="https://github.com/user-attachments/assets/a65ff042-f223-4515-90e5-e44322387be9" />


Finding the first flag was easy, i just had to use the find command to search for it - ```find / -type f -name user.txt 2>/dev/null```


<img width="495" height="63" alt="image" src="https://github.com/user-attachments/assets/626df96e-65ca-41f4-8fb2-a91e7c1116ef" />


And this is where the main 'hacking' started hehe.

Listing our current directory, there is an encypted file with its key and trying to decrypt this file require a passphrase so I downloaded the file and the key to my own machine and used a johntheripper tool ```gpg2john``` to find and decrypt the passphrase


<img width="606" height="183" alt="image" src="https://github.com/user-attachments/assets/33c0cd87-1235-4025-9238-f48d91194381" />


'Alexandru' is the passphrase to decrypt the file and back to the target machine. I imported the key on the target machine and tried to decrypt the file again with the passphrase and got the credentials for another user 'merlin'


<img width="602" height="245" alt="image" src="https://github.com/user-attachments/assets/259b3d9f-33a8-4167-8492-9fbe36b156e5" />


I switched to the user and tried to gain root privilege. I used ```sudo -l``` to determine what commands I can run as a sudoer


<img width="604" height="63" alt="image" src="https://github.com/user-attachments/assets/1de63858-51a6-414a-8632-4a8d040580a0" />


I found a way to exploit this ```/usr/bin/zip``` binary in https://gtfobins.github.io/gtfobins/zip/#sudo and escalated privileges to read the root flag. And voila!


<img width="586" height="193" alt="image" src="https://github.com/user-attachments/assets/aed61e6e-8b33-484f-a1f2-a99823405f3a" />







~cyb3rl1lyy💻🌺
