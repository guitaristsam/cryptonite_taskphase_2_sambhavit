# Vigenere
**Flag: picoCTF{d0nt_us3_v1g3n3r3_c1ph3r_ae82272q}**
## My thought process and approach to the challenge:

### Description
Can you decrypt this message?        
Decrypt this message using this key "CYLAB"     

A file called `cipher.txt` was attached, which contained `rgnoDVD{O0NU_WQ3_G1G3O3T3_A1AH3S_cc82272b}`.    
Immediately, I recognized that it would be a VIGENERE CYPHER because I had done a VIGENERE CYPHER in a previous hackathon.   
Then, I used an online decoder[Key was given(CYLAB)].    
![image](https://github.com/user-attachments/assets/78f7b11f-b80f-4b18-91ca-cc6254aed988)

This helped me get the flag: `**picoctf{d0nt_us3_v1g3n3r3_c1ph3r_ae82272q}**.`

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1) Vigenere Cipher Decryption: Strengthened my understanding of how the Vigenere cipher uses a repeating key to encode and decode text.     
2) Cryptographic Weakness: Realized why the Vigenere cipher is very weak by modern standards since its repetitive key pattern can be exploited very easily.
3) Pattern Recognition: Improved my ability to recognize the structure of typical CTF flags and the type of cypher and encoding/decoding needed.

##  Incorrect tangents I went on while solving :
1) Went on no incorrect tangents (pretty straightforward and easy challenge).
