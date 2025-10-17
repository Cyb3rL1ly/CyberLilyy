target: 10.10.154.62
From our nmap scan, I found 3 ports open
![[Pasted image 20251017185538.png]]
ajp13 is the apache jserve protocol is used to send reverse proxy to the backend application server. It has a file read vulnerability which i exploited using metasploit and i was able to get the credentials to login using ssh
![[Pasted image 20251017190923.png]]
finding the first flag was easy, i just had to use the find command to search for it - ```find / -type f -name user.txt 2>/dev/null```
![[Pasted image 20251017191234.png]]
And this is where the main 'hacking' started hehe.
Listing our current directory, there is an encypted file with its key, trying to decrypt this file needed a passphrase so i downloaded the file and the key to my own machine and used johntheripper tool ```gpg2john``` to find and decrypt the passphrase
![[Pasted image 20251017191901.png]]

'Alexandru' is the passphrase to decrypt the file and back to the target machine. i imported the key and tried to decrypt the file again with the passphrase and got the credentials for another user 'merlin'
![[Pasted image 20251017192140.png]]
i switched to the user and tried to gain root privilege. I used ```sudo -l``` to determine what commands i can run as the user 'merlin'![[Pasted image 20251017192316.png]]
I found a way to exploit this in https://gtfobins.github.io/gtfobins/zip/#sudo and escalated privileges to read the root flag.
![[Pasted image 20251017192721.png]]
