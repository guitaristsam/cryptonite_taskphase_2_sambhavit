# Vault door 3
**Flag:`picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}`**
## My thought process and approach to the challenge:
Didn't understand the code first because I don't have a lot of experience with Java.  
Firstly tried to understand parts of the code and relate it to the languages I already knew.  

Understood these things:  
1)The code checks if the password that is the input is 32 characters long.  
2) A character array is used to rearrange the characters from the input in a way that:

  - **The first 8 characters are kept the same.**  
    because of:
    ```java
    for (i = 0; i < 8; i++) {
        buffer[i] = password.charAt(i);
    }
    ```

  - **The next 8 characters are taken from the end of the password.**  
    because of:
    ```java
    for (; i < 16; i++) {
        buffer[i] = password.charAt(23 - i);
    }
    ```

  - **Last 16 characters: The remaining characters are filled in based on specific rules:**
    - **Even indexed positions (16, 18, 20, ...)** take characters from the end of the input (like the 30th character, 28th character, etc.).  
      because of:
      ```java
      for (; i < 32; i += 2) {
          buffer[i] = password.charAt(46 - i);
      }
      ```
    
    - **Odd indexed positions (17, 19, 21, ...)** take characters directly from the input.  
      because of:
      ```java
      for (i = 31; i >= 17; i -= 2) {
          buffer[i] = password.charAt(i);
      }
      ```


***Using this knowledge wrote a program in C that would help in reversing the code:***
```C
#include <stdio.h>
#include <string.h>

int reverse_vault_password(const char* target, char* buffer) {
    for (int i = 31; i >= 17; i -= 2) buffer[i] = target[i];
    for (int i = 16; i < 32; i += 2) buffer[i] = target[46 - i];
    for (int i = 0; i < 8; i++) buffer[i] = target[i];
    for (int i = 8; i < 16; i++) buffer[i] = target[23 - i];
    
    return 0;
}

int main() {
    const char* target_password = "jU5t_a_sna_3lpm18gb41_u_4_mfr340";
    char reconstructed_password[33] = {0};

    int result = reverse_vault_password(target_password, reconstructed_password);
    if (result == 0) {
        printf("Reconstructed Password: %s\n", reconstructed_password);
    } else {
        printf("Error reconstructing password.\n");
    }

    return 0;
}

```

***This gave me the output for the flag***
```
Reconstructed Password: jU5t_a_s1mpl3_an4gr4m_4_u_1fb380
```
## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.  
1)Learnt a lot about Java in order to understand the code   
2)Learnt how to break down code into small parts to understand and Reverse Engineer it part by part.   
3)Learnt a lot about how to make your writeups in Github understandable and visually appealing (Took help from https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)   
4)First time writing code in C to reverse enginner something written in Java

##  Incorrect tangents I went on while solving :
1)Spent time finding something hidden in the code instead of just understanding and reverse engineering the code
2)Tried overcomplicating the code too much when it was very simple
