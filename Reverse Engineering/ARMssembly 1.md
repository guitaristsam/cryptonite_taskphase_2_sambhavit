# ARMssembly 1
**Flag: picoCTF{00000d2a}**
## My thought process and approach to the challenge:
Looking through the function, to understand how the code works,

```
func:
	sub	sp, sp, #32
	str	w0, [sp, 12]
	mov	w0, 79
	str	w0, [sp, 16]
	mov	w0, 7
	str	w0, [sp, 20]
	mov	w0, 3
	str	w0, [sp, 24]
	ldr	w0, [sp, 20]
	ldr	w1, [sp, 16]
	lsl	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 24]
	sdiv	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 12]
	sub	w0, w1, w0
	str	w0, [sp, 28]
	ldr	w0, [sp, 28]
	add	sp, sp, 32
	ret
	.size	func, .-func
	.section	.rodata
	.align	3

```



Then I tried breaking it down to understand each part, which helped me understand that:   
1.
```
sub sp, sp, #32      ; Step 1
str w0, [sp, 12]     ; Step 2
mov w0, 79           ; Step 3

```

`sub sp, sp, #32:` Reserves 32 bytes of space on the stack;             
`str w0, [sp, 12]` Stores the current value of w0 in the memory location [sp + 12].          
`mov w0, 79`Loads the immediate value 79 into w0.          

2.
```
ldr
```
`ldr` is used to load data from memory into a register.      
3.
```
lsl
```
`lsl` stands for Logical Shift Left. It is used to shift the bits of a register to the left by a specified number of positions.
4.
```
sdiv 
```
`sdiv` performs signed integer division of two registers, storing the result in the destination register.     

Moving on,      
Current stack:          
```
stack + 12 = user input  
stack + 16 = 79  
stack + 20 = 7  
stack + 24 = 3  
```

then
```
ldr	w0, [sp, 20]
ldr	w1, [sp, 16]
lsl	w0, w1, w0
str	w0, [sp, 28]
```
Load 7 (from 20) into w0.          
Load 79 (from 16) into w1.               
Perform a left shift (lsl) of 79 by 7.               
Store the result at stack offset 28.           
```
79 in binary: 01001111
Left shift by 7: 01001111 → 1000000000000
Convert to decimal: 10112.
```
Result(10112) gets stored at stack offset of 28.
```
stack + 28 = 10112  
```
Moving on,    
```
ldr	w1, [sp, 28]
ldr	w0, [sp, 24]
sdiv	w0, w1, w0
str	w0, [sp, 28]
```
Load 10112 into w1.       
Load 3 into w0.               
Perform signed division (sdiv): 10112 // 3.           
Store the result at stack offset 28.        
```
10112 // 3 = 3370
```
```
stack + 28 = 3370  
```
Moving on,   
```
ldr	w1, [sp, 28]
ldr	w0, [sp, 12]
sub	w0, w1, w0
str	w0, [sp, 28]
ldr	w0, [sp, 28]
add	sp, sp, 32
ret
```
Load 3370 into w1.             
Load the user input into w0.             
Subtract: 3370 - user_input.            
Store the result at stack offset 28.    

---
Understanding main:               
```
cmp	w0, 0
bne	.L4
```
we can see that the function is compared to 0. If it is not 0, the program jumps to .L4, printing You Lose :(. Otherwise, it prints You win!.        

Therefore, To win, the subtraction in func must equal 0. This means that user input should be equal to 3370.        
Using format given in the question we convert 3370 to a 32-bit hex value:     
![image](https://github.com/user-attachments/assets/06ed0db3-43d3-4d91-88e7-fd8b8fe3c15d)

which will be **`0x00000d2a`**, Giving us the flag <ins>**`picoCTF{00000d2a}`**</ins>.

---

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1. ARM Assembly Fundamentals:

Learnt and understood ARM instructions like a sub, str, mov, ldr, lsl and sdiv.            
Understood how the stack is used to store variables in ARM assembly.                     

2. Bitwise Operations:

Developed an understanding of commands and uses of functions like `lsl`.              

3. Reverse Engineering:

I got further experience in understanding stack layouts and seeing variable changes in real time step-by-step in assembly.             

4. Critical Thinking:

Gained an ability to trace back from a desired outcome.           

---

##  Incorrect tangents I went on while solving :

1. Initially misinterpreted the Role of lsl:

I wasted time recalculating results incorrectly.         

2. Misjudged the Objective:

Wasted time by misunderstanding the code.

