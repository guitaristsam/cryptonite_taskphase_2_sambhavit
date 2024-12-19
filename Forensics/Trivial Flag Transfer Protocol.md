# Trivial Flag Transfer Protocol
**Flag: picoCTF{}**
## My thought process and approach to the challenge:
Downloading the file and saw that it was named `tftp.pcapng`; I had never heard of a pcapng file before, ran 
```
┌──(sam㉿voldermort)-[~/Downloads]
└─$ file tftp.pcapng
tftp.pcapng: pcapng capture file - version 1.0
```
After some research, I came across https://pcapng.com/, which helped me get to know that:            
`File format is supported by Wireshark and tcpdump.`.             
Opened it using Wireshark and saw this:
![image](https://github.com/user-attachments/assets/903753a3-6b26-4b8f-a68e-37b0657c4834)
(The bottom right seemed very similar to https://hexed.it/, so I tried to open it there)        
![image](https://github.com/user-attachments/assets/040a2ccc-968d-47cc-bb43-1eaf9e7133a0)
but this also wasn't that useful.     



## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.

##  Incorrect tangents I went on while solving :
1. Tried Searching for the flag directly in the file using Hexedit and Wireshark              
   ![image](https://github.com/user-attachments/assets/2db7160a-84dc-4966-b8fe-8abb3ac5e635)
   although I didn't find `picoCTF`, I found `pico`, which made me waste more time on it than I would've liked.                  
   ![image](https://github.com/user-attachments/assets/066b7c21-f2f5-42cf-b6af-e541a3252eef)


               

