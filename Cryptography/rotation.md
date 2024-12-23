# rotation
**Flag: picoCTF{r0tat1on_d3crypt3d_949af1a1}**
## My thought process and approach to the challenge:
A file called `encrypted.txt` was attached, which contained `xqkwKBN{z0bib1wv_l3kzgxb3l_949in1i1}`.        
With no idea of what cipher it was, I used https://www.dcode.fr/cipher-identifier which helped me find what kind of cipher it was.
![image](https://github.com/user-attachments/assets/18b60006-3079-4aa5-86e7-c7a1b3f08e6b)
This helped me get to know that it was an `Affine Cipher`.         
Then using the Affine Cipher Decoder(https://www.dcode.fr/affine-cipher):
![image](https://github.com/user-attachments/assets/e279f0fc-226f-41f2-9cbc-58e7b6adbe2a)
This helped me get the flag: **picoCTF{r0tat1on_d3crypt3d_949af1a1}**

---

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1. Improved upon identifying ciphers
2. Using online decoders effectively

---

##  Incorrect tangents I went on while solving :
1) Went on no incorrect tangents (pretty straightforward and easy challenge).
