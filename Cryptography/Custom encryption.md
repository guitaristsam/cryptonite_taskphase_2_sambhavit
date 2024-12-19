# Custom encryption
**Flag: picoCTF{custom_d2cr0pt6d_8b41f976}**
## My thought process and approach to the challenge:
Saw that there was a `flag_info file` and a `code file` that was attached,          
Inspecting the code file first, I saw :
```python
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")


```
Looked confusing at first but I broke the code down into small sections to understand each part,      
By researching a bit I got to know that this code was a hybrid encryption algorithm that combines Diffie-Hellman key exchange, XOR-based encryption, and a basic custom encoding scheme.           

### Some Key Functions:
1. `generator(g, x, p)`               
   • Implements modular exponentiation, returning `g^x mod p`.              
   • Used in the Diffie-Hellman key exchange process.(Got to know more about this using https://www.youtube.com/watch?v=UDyRrlCLhp0&t=423s&ab_channel=PracticalNetworking)         
2. `encrypt(plaintext, key)`          
   • Encodes each character by multiplying its ASCII value (ord(char)) with the key and a fixed multiplier(311).           
3. `is_prime(p)`        
   • Checks if a number p is prime           
4. `dynamic_xor_encrypt(plaintext, text_key)`                   
   • Performs a dynamic XOR encryption:                   
   • Reverses the plaintext.                 
   • Iterates through each character and XORs it with the corresponding character from text_key (repeated if necessary).             
   • Returns the XOR-encrypted text.             
5. `test(plain_text, text_key)`                        
   • Initializes prime numbers, `p=97` and `g=31` for the Diffie-Hellman key exchange.                   
   • Verifies p and g as primes                 
   • Randomly selects a and b                 
   • Computes public keys using generator function mentioned above.               
   • Calculates the shared secret key           
   • Verifies that both parties compute the same shared key.              
   • Encrypts the `plain_text` using dynamic XOR and the `encrypt` function              
   • Outputs the final calculated cipher text
---

### The main function:
The script expects a command-line argument `sys.argv[1]` for the message to encrypt.
Calls the `test` function with the provided message and a fixed text key `"trudeau"`.

---
## Modifying the script:
Using what I learnt about the functions and how the process works I added a `decrypt` function to reverse the encryption:         
```python
def decrypt(cipher, key):
    plaintext = []
    for num in cipher:
        if num != 0:  # Handle zero padding
            plaintext.append(chr(num // (key * 311)))
        else:
            plaintext.append(chr(0))
    return ''.join(plaintext)
```
Also added `dynamic_xor_decrypt` function to reverse the XOR encryption:
```python
def dynamic_xor_decrypt(cipher_text, text_key):
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(cipher_text[::-1]):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
    return plain_text[::-1]
```
Then to calculate the shared key:                
```python
shared_key = pow(generator(g, a, p), b, p)
```
Then using this I created a script using `nano cuse.py`
then added the logic:                   
```python
from random import randint
import sys

def generator(g, x, p):
    return pow(g, x) % p

def decrypt(cipher, key):
    plaintext = ""
    for num in cipher:
        char_code = num // (key * 311)
        plaintext += chr(char_code)
    return plaintext

def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True

def dynamic_xor_decrypt(ciphertext, text_key):
    plaintext = ""
    key_length = len(text_key)
    for i, char in enumerate(ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plaintext = decrypted_char + plaintext
    return plaintext

def test(text_key, a, b, cipher):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return

    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return

    semi_cipher = decrypt(cipher, shared_key)
    original_text = dynamic_xor_decrypt(semi_cipher, text_key)
    print(f'decrypted text is: {original_text}')

if __name__ == "__main__":
    a = int(sys.argv[1])
    b = int(sys.argv[2])
    cipher = eval(sys.argv[3])
    test("trudeau", a, b, cipher)
```
Then gave it permissions to execute using `chmod +x cuse.py`.     
Then I ran the code with the `decrypt` function to get the flag (gave it values of a,b and the ciphertext which were present in the `flag_info` file)
![image](https://github.com/user-attachments/assets/cf727748-5012-49bd-b279-a400eabd7e0a)

This gave me the flag : **`picoCTF{custom_d2cr0pt6d_8b41f976}`**
## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
Learnt about Diffie-Hellman key exchange and its application.           
Learned about combining XOR operations with key-based encryption for hybrid security.             
Improved my debugging skills for Python scripts in a Linux environment, specifically Kali Linux.             

##  Incorrect tangents I went on while solving :
Miscalculated the shared key due to confusion about the order of operations in modular arithmetic, leading to invalid decryption attempts.          
Attempted to manually decrypt parts of the cipher without using the script, which was error-prone and inefficient.               
