# GDB baby step 1
**Flag: picoCTF{549698}**
## My thought process and approach to the challenge:
At first I had no idea what I was looking at after downloading and opening the file so instead I went to my terminal to see what type of file it was.   
![image](https://github.com/user-attachments/assets/a9b2be45-7f79-41d5-888d-695e874ec205)    
Then I learnt that it was an ELF or an Executable and Linkable Format file so tried to see if I could see what was either hidden or embedded in the file.  
Using one of the hints **gdb is a very good debugger to use for this problem and many others!** I learnt about gdb and then using the second hint **main is actually a recognized symbol that can be used with gdb commands.**, I ran main as an argument.
    
***Got to know about the disassemble command with***
```
(gdb) help disassemble
Disassemble a specified section of memory.
Usage: disassemble[/m|/r|/s] START [, END]
Default is the function surrounding the pc of the selected frame.

With a /s modifier, source lines are included (if available).
In this mode, the output is displayed in PC address order, and
file names and contents for all relevant source files are displayed.

With a /m modifier, source lines are included (if available).
This view is "source centric": the output is in source line order,
regardless of any optimization that is present.  Only the main source file
is displayed, not those of, e.g., any inlined functions.
This modifier hasn't proved useful in practice and is deprecated
in favor of /s.

With a /r modifier, raw instructions in hex are included.

With a single argument, the function surrounding that address is dumped.
Two arguments (separated by a comma) are taken as a range of memory to dump,
  in the form of "start,end", or "start,+length".

Note that the address is interpreted as an expression, not as a location
like in the "break" command.
So, for example, if you want to disassemble function bar in file foo.c
you must type "disassemble 'foo.c'::bar" and not "disassemble foo.c:bar".
```
***Then used it with gdb like :***
```bash
sambhavit@LAPTOP-JL16J5LA:/mnt/c/Users/sambh/Downloads$ gdb debugger0_a
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
```
FINALLY! got the eax(this was after a lot of trial error with a lot of different fuctions because this was me using gdb for the first time)   

Now I can finally focus on just this part:
`0x0000000000001138 <+15>:    mov    $0x86342,%eax`

Using `If the answer was 0x11 your flag would be picoCTF{17}.` given in the question, I realised that I needed to change the hexadecimal`0x86342` to decimal.    
![image](https://github.com/user-attachments/assets/ec9c1e4f-e0ca-4e89-a70c-6e70800c1f75)   


***Using an online converter I got `549698`***
Which gave me the flag : `picoCTF{549698}`
## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1)Learnt about ELF files( was my first time coming across them).   
2)Learnt about gdb   
   - Spent a lot of time trying different fuctions and figuring out gdb as a whole along with learning different fuctions and also using cheatsheets like https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf   
3)Learnt about different assembly (eg: AT&T and INTEL) and how to read assembly code.   

##  Incorrect tangents I went on while solving :
1)Wasted a lot of time looking through the source code give trying to find a flag hidden in it.   
2)Tried using https://hexed.it/ to find out any hidden picoCTF text if there was any instead of focusing on diassembly.   
3)Wasted a lot of time trying out many different commands in GDB insted of focusing on the ones that would just give me the main fuction.
