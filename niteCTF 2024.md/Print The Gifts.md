# Print The Gifts

## Failed Challenge Write-Up

### General Context and Progress

- **Challenge Type**: The task involved exploiting a binary program (probably in C or C++) by using `ncat` to retrieve a flag. The focus was on finding format string vulnerabilities, memory overwrites, and potential buffer overflow issues.
- **Goal**: The goal was to grab the flag, which was in the format `nite{}`, by exploiting the weaknesses in the binary through `ncat`.
- **Exploits and Vulnerabilities**: The main vulnerabilities I was working with were format string issues (`%x`, `%s`, `%n`), buffer overflows, and other memory manipulation techniques.

---

### Tools and Techniques Explored

#### GDB Debugging

- I tried using GDB to go through the program's execution, hoping to:
  - Step through the code.
  - Watch how memory changes.
  - Find any clues that could help me exploit it.
- I looked at memory with commands like `x/16x`, `x/16s`, and `x/16b`, aiming at places like `0x7fffffffdb70`, but couldn’t find any useful information about the flag from the memory.

#### Format String Vulnerability

- I tested the `%n` format specifier to overwrite memory by writing the number of characters already printed into certain memory spots.
- However, manipulating memory directly led to segmentation faults, most likely because I was trying to access invalid memory or using the wrong addresses.

#### Memory Layout Exploration

- I checked various parts of memory, hoping to find the flag, but nothing concrete came up.
  - The flag wasn’t loaded into memory, nor was it being pulled from a file (e.g., `flag.txt` wasn’t being opened).
  - It seems the flag could either be dynamically generated or only revealed through a specific exploit.

---

### Key Insights

1. **Flag Generation**: Since there was no `flag.txt` and the flag wasn’t directly loaded into memory, it suggests the flag is generated on the fly or exposed through exploitation.
2. **Format String Attacks**: The `%n` vulnerability works for memory overwriting, but the key is finding the right memory location to target.
3. **Segmentation Faults**: These faults happened when accessing the wrong memory, which shows that getting the right addresses is crucial for success.

---

### What I Could Have Done Next

1. **Keep Testing `%n`**:
   - I could keep exploring `%n` to find memory addresses that can be safely overwritten, especially in the stack or heap.

2. **Look for Memory Leaks**:
   - By exploiting format string vulnerabilities like `%x` or `%s`, I could try to find leaks that might give away addresses or even part of the flag.

3. **Check the `ncat` Output**:
   - It would be useful to pay attention to what `ncat` is printing during interaction, as it might leak parts of the flag or give hints.

4. **Be Aware of ASLR**:
   - Since ASLR randomizes memory addresses, hardcoding addresses like `0x7fffffffdb70` won’t work. I need to dynamically figure out memory locations during runtime.

5. **Attach GDB After Execution**:
   - I could attach GDB to the process using its PID to inspect where `%n` is writing during runtime.

6. **Understanding the Flag Flow**:
   - The flag isn’t just sitting in memory, so I need to understand how the program handles it and find a way to trigger its exposure via `%n`.

7. **Using the Right Tools**:
   - Tools like `pwntools` could help automate some of the tasks like generating payloads and interacting with the program.

8. **Reverse Shell**:
   - I could have attempted a reverse shell and used a command like `cat flag.txt` if the flag were stored in such a way.

---

### Lessons Learned

1. **Understanding Memory Layout**:
   - I learned about memory layout and how ASLR works, which is essential when dealing with binary exploits.

2. **Real-Time Debugging**:
   - Using GDB helped me understand how the program behaves and where I could tweak it for exploitation.

3. **Leveraging Automation**:
   - I realized how helpful tools like `pwntools` can be in automating repetitive tasks, which makes the exploitation process more efficient.
