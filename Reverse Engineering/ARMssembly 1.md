# ARMssembly 1
**Flag: picoCTF{}**
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
`ldr` is used to load data from memory into a register. while ``is used to
## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.



##  Incorrect tangents I went on while solving :

