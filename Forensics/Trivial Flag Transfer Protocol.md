# Trivial Flag Transfer Protocol
**Flag: picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}**
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

Then went to export>TFTP packages
![image](https://github.com/user-attachments/assets/942c99b9-a542-463a-a3e7-6f950273e38f)
Then saved it to a folder      
![image](https://github.com/user-attachments/assets/ebb6454d-d9f9-4168-bfc4-9537afe3fc12)
After playing around a bit, I found `steghide` in the files
![image](https://github.com/user-attachments/assets/a59e0ba7-a38d-4001-8447-0fcccc454a17)
Then opened up the `/home/sam/Downloads/TFTP/instructions.txt` file which had    `GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQV JVYYPURPXONPXSBEGURCYNA`
I used `https://www.dcode.fr/cipher-identifier` to identify the type of cipher it was.           
![image](https://github.com/user-attachments/assets/eed52045-3314-45cb-8de1-4f61ca16c7c1)
The decoded message after adding some spaces became:           
`TFTP DOESN'T ENCRYPT OUR TRAFFIC, SO WE MUST DISGUISE OUR FLAG TRANSFER. FIGURE OUT A WAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN`.            
Then decrypted the `plan` file as well which had `VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF`, and used the same method to get:      
![image](https://github.com/user-attachments/assets/23912202-f04d-4e5f-a471-ffacc9f73e78)      
The decoded message after adding some spaces became:       
`I USED THE PROGRAM AND HID IT WITH DUE DILIGENCE. CHECK OUT THE PHOTOS.`     

Using this I was able to understand that they used `steghide` and hid something in the photos.

The password seems to be `DUEDILIGENCE`.

After trying the password with all the pictures I got:
![image](https://github.com/user-attachments/assets/c94ebe7f-ab3c-4896-8927-2eb24386ad35)

which gave me the flag: **picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}**

---

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1. Understanding the .pcapng file format and its compatibility with tools like Wireshark.
2. Exporting TFTP packets using Wireshark.
3. Using dcode.fr cipher identifier for cipher decryption(good tool for the future).
4. Improved my skills with steghide for steganography tasks.
##  Incorrect tangents I went on while solving :
1. Tried Searching for the flag directly in the file using Hexedit and Wireshark              
   ![image](https://github.com/user-attachments/assets/2db7160a-84dc-4966-b8fe-8abb3ac5e635)
   although I didn't find `picoCTF`, I found `pico`, which made me waste more time on it than I would've liked.                  
   ![image](https://github.com/user-attachments/assets/066b7c21-f2f5-42cf-b6af-e541a3252eef)


               

