# <ins>Day 6: If I can't find a nice malware to use, I'm not going.</ins>

## <ins>Challenge Objective:</ins>
> Analyze malware behaviour using sandbox tools.
> Explore how to use YARA rules to detect malicious patterns.
> Learn about various malware evasion techniques.
> Implement an evasion technique to bypass YARA rule detection.

First I started the machine and went into the attackbox:         
![image](https://github.com/user-attachments/assets/7305ec9a-e268-43bb-b993-247b2ca6750f)

First I navigated to the `C:\Tools` directory:   
![image](https://github.com/user-attachments/assets/db5ecacf-b37c-4695-a150-a2b86892f509)

There I saw:
```bash
-a----       10/28/2024   9:21 AM           5817 JingleBells.ps1
```
Then I did `PS C:\Tools > cat JingleBells.ps1` to get:
```C
# Define the YARA rule path
$yaraRulePath = "C:\Tools\YARARULES\CheckRegCommand.yar"
# Define the path to the YARA executable
$yaraExecutable = "C:\ProgramData\chocolatey\lib\yara\tools\yara64.exe"
$logFilePath = "C:\Tools\YaraMatches.txt"


# Function to log event data to a file
function Log-EventDataToFile {
    param (
        [string]$commandLine,
        [string]$yaraResult,
        [string]$eventId,
        [string]$eventTimeCreated,
        [string]$eventRecordID
    )

    # Prepare log entry
    $logEntry = "Event Time: $eventTimeCreated`r`nEvent ID: $eventId`r`nEvent Record ID: $eventRecordID`r`nCommand Line: $commandLine`r`nYARA Result: $yaraResult`r`n"
    $logEntry += "--------------------------------------`r`n"

    # Debugging output to ensure logging function is called
    Write-Host "Logging to file: $logFilePath"
    Write-Host $logEntry

    # Append the log entry to the file
    Add-Content -Path $logFilePath -Value $logEntry
}


# Function to run YARA on the command line and log result only if a match is found
function Run-YaraRule {
    param (
        [string]$commandLine,
        [string]$eventId,
        [string]$eventTimeCreated,
        [string]$eventRecordId

    )

    # Create a temporary file to store the command line for YARA processing
    $tempFile = [System.IO.Path]::GetTempFileName()
    try {
        # Write the command line to the temporary file
        Set-Content -Path $tempFile -Value $commandLine

        # Run YARA on the temporary file
        $result = & $yaraExecutable $yaraRulePath $tempFile

        # Only log if YARA finds a match (non-empty result)
        if ($result) {
            #Write-Host "YARA Match Found for Command Line: $commandLine"
            Write-Host "YARA Result: $result"

            # Log the event data to a file in C:\Tools
            Log-EventDataToFile -commandLine $commandLine -yaraResult $result -eventId $eventId -eventTimeCreated $eventTimeCreated -eventRecordID $eventRecordId

            # Display warning
            $warning = "Malicious command detected!THM{GlitchWasHere}"
            Show-MessageBox -Message $warning

        }
    } finally {
        # Clean up the temporary file after processing
        Remove-Item -Path $tempFile -Force
    }
}

# Function to display Popup
Add-Type -AssemblyName System.Windows.Forms
function Show-MessageBox {

    param (
        [string]$Message,
        [string]$Title = "Notification"

    )

   # Start a new PowerShell job that runs asynchronously and doesn't block the main flow
    Start-Job -ScriptBlock {
        param($msg, $ttl)
        Add-Type -AssemblyName System.Windows.Forms
        [System.Windows.Forms.MessageBox]::Show($msg, $ttl)
    } -ArgumentList $Message, $Title

}

# Function to handle Sysmon events
function Handle-SysmonEvent {
    param (
        [System.Diagnostics.Eventing.Reader.EventLogRecord]$event
    )
    #write-host "handling event"

    # Extract Event ID, time created, and relevant properties
    $eventID = $event.Id
    $eventRecordID = $event.RecordId
    $eventTimeCreated = $event.TimeCreated
    $commandLine = $null

    #write-host "Current event record id: " $event.RecordId
    #write-host "Last event record id: " $lastRecordId
    if ($eventID -eq 1 -and ($event.Properties[4] -notcontains "C:\ProgramData\chocolatey\lib\yara\tools\yara64.exe") ) {
    # Event ID 1: Process Creation
        $commandLine = $event.Properties[10].Value  # Get the command line used to start the process
        #write-host $commandLine
        if ($commandLine) {
        Run-YaraRule -commandLine $commandLine -eventId $eventID -eventTimeCreated $eventTimeCreated -eventRecordId $eventRecordID

    }
    }


}

# Poll for new events in the Sysmon log
$logName = "Microsoft-Windows-Sysmon/Operational"
wevtutil cl "Microsoft-Windows-Sysmon/Operational"

# Initialize lastRecordId safely
$lastRecordId = $null
[int[]]$processedEventIds=1
try {
    $lastEvent = Get-WinEvent -LogName $logName -MaxEvents 1
    if ($lastEvent) {
        $lastRecordId = $lastEvent.RecordId
    } else {
        Write-Host "No events found in Sysmon log."
    }
} catch {
    Write-Host "Error retrieving last record ID: $_"
    exit
}

Write-Host "Monitoring Sysmon events... Press Ctrl+C to exit."

while ($true) {
    try {
        # Get new events after the last recorded ID
        $processedEventIds= $processedEventIds|Sort-Object
        #write-host $processedEventIds
        $lastRecordId=$processedEventIds[-1]
        #write-host $lastRecordId
        $newEvents = Get-WinEvent -LogName $logName | Where-Object { $_.RecordId -gt $lastRecordId }
        #write-host $newEvents.Count

        if ($newEvents.Count -eq 0) {
            Write-Host "No new events found."
        }

        else{

        foreach ($event in $newEvents) {
            # Check if this event ID has already been processed
            if ($event.RecordId -in $processedEventId ) {
             Write-Host "Record ID $($event.RecordId) has already been processed"

            }

            else{
                # Process only new events
                Handle-SysmonEvent -event $event

                # Add the event ID to the processed set
                $processedEventIds+=$event.RecordId
                #write-host "processed events: " $processedEventIds
                }

                # Update the last processed event ID

            }
        }
    } catch {
        Write-Host "Log is empty"
    }

    # Sleep for 5 seconds before checking again
    Start-Sleep -Seconds 2
}
```
Then I went to the file explorer to find the malware:
![image](https://github.com/user-attachments/assets/17deeaed-1274-4969-a5a0-859599ac822c)

Then I ran the malware and got this:
![image](https://github.com/user-attachments/assets/f46ed4d5-9d99-420f-9536-b2b16259e4f4)

Then I closed and restarted: `.\JingleBells.ps1`
![image](https://github.com/user-attachments/assets/2b9f90a2-4c01-4f27-b57b-a044335513ef)
Then I found the obfuscated malware:
![image](https://github.com/user-attachments/assets/9ab55db4-6f11-4a72-ad93-608bf403beaa)
Then I ran the malware but it was not found by the system.(Which means the obfuscated malware worked)               

Then ran `floss.exe C:\Tools\Malware\MerryChristmas.exe |Out-file C:\tools\malstrings.txt` in the terminal:
![image](https://github.com/user-attachments/assets/0eda915e-db6c-4b46-9b5a-d32cc5706743)
This gave an output in `malstrings.txt`:
![image](https://github.com/user-attachments/assets/71ea9f42-a0cc-4439-b297-4e7d0b14219c)
which was:
```


FLARE FLOSS RESULTS (version v3.1.0-0-gdb9af41)

+------------------------+----------------------------------------------------+
| file path              | C:\Tools\Malware\MerryChristmas.exe                |
| identified language    | unknown                                            |
| extracted strings      |                                                    |
|  static strings        | 1715 (18576 characters)                            |
|   language strings     |    0 (    0 characters)                            |
|  stack strings         | 0                                                  |
|  tight strings         | 1                                                  |
|  decoded strings       | 0                                                  |
+------------------------+----------------------------------------------------+


 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 
  FLOSS STATIC STRINGS (1715)  
 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 

+------------------------------------+
| FLOSS STATIC STRINGS: ASCII (1714) |
+------------------------------------+

!This program cannot be run in DOS mode.
.text
`.data
.rdata
@.pdata
@.xdata
@.bss
.idata
.CRT
.tls
.reloc
B/19
B/31
B/45
B/57
B/70
B/81
8MZu
HcP<H
ATUWVSH
 [^_]A\
 [^_]A\
l$0H
T$ I
)t$@
)|$PD
)D$`
D$0I
|$(H
t$ H
(t$@1
(|$PD
(D$`H
D$XH
T$XL
D$`L
L$hH
WVSH
PHc5F
T$ H
P[^_
L$ D
T$8H
UAWAVAUATWVSH
[^_A\A]A^A_]
D$0H
t$ H
L$ H
L$ H
)T$0
CCG 
\twTH
ATUWVSH
 [^_]A\H
WVSH
 [^_
9MZu
HcQ<H
HcA<H
WVSH
:MZuYHcB<H
 [^_
 [^_
:MZu
LcB<I
8MZu
HcP<H
8MZu
IcP<L
@' t\tH
8MZu
HcH<H
:MZu
LcB<I
;MZu
McC<M
QPH=
WVSH
0[^_
L$ H
L$ H
D$PA
T$XH
D$HL
L$0H
D$(L
L$DH
D$8L
D$PD
C$9C(~
u HcS$
AWAVAUATUWVSH
|$01
HcS$
C$t7
C$9C(~
H[^_]A\A]A^A_
WVSH
HcC$
S$t<
S$9S(~
HcC$
S$9S(~
 [^_
@t`L
D$,L
\$,A
D$,L
L$-L
\$,M
UAWAVAUATWVSH
l$ A
l$ H
[^_A\A]A^A_]
HcC$
C$s8
C$9C(~
HcC$
C$9C(~
UAWAVAUATWVSH
l$ 1
l$ t
vAI9
HcC$
C$t;D
C$9C(~
HcC$
S$9S(~
[^_A\A]A^A_]
UATWVSH
t$ H
HcS$
C$t6
C$9C(~
[^_A\]
[^_A\]
UWVSH
=UUUUw
([^_]
([^_]
WVSH
gfffH
gfffH
 [^_
T$0H
|$0L
D$LH
L$HI
L$HI
T$0H
|$0L
D$LH
L$HH
HcC$
S$9S(~
L$HI
WVSH
T$0H
|$0L
|$LH
L$HI
P[^_
P[^_
L$HI
P[^_
AUATUWVSH
|$0H
|$0I
D$0.H
D$1D
l$.H9
gfffE
gfffH
X[^_]A\A]
|$01
AWAVAUATUWVSH
D$0H
|$|L
@u\t9
D$pumHc
|$xH
[^_]A\A]A^A_
D$pH
9|$x
D$pI
L$0H
T$pH
D$xI
D$`I
D$pH
D$xI
|$@H
D$xI
|$@H
D$xI
|$@H
L$x@
D$xI
|$ H
T$(f
D$xD
D$pH
D$xI
T$ H
|$@H
D$xI
T$ H
|$@H
D$xI
T$ H
|$@H
L$`L
T$4L
L$ H
L$^A
\$8~
T$^f
D$pH
D$p1
D$pH
T$pH
D$pD
WVSH
 [^_
AWAVAUATUWVSH
8[^_]A\A]A^A_
t$,A
AWAVAUATUWVSH
D$ H
T$@L
D$8L
L$HH
D$(H
D$8A
t$8H
D$hf
T$pf
\$`D
D$(A
T$0H
[^_]A\A]A^A_
D$(A
T$0H
[^_]A\A]A^A_
D$(A
T$0H
\$@A)
T$pE
D)\$pD
\$xE1
t$XD
\$dH
D$dt$
ID$d
t$XE1
D$T 
\$XE1
\$XH
D$xA9E
HcD$xE
t$`1
D$T 
D$0H
t$T\t0
D$xD
L$XL
t$hE)
t$X)
t$pD
)D$p)
T$|H
\$|I
D$`A)
D$pA
L$pD
L$pH
\$@I
\$XE
D$xI
D$p1
D$xD
T$hD
L$hD
L$hD
\$@H
L$@H
d$ u\t
D$XE
T$@H
L$@L
D$ \t
L$@H
D$T 
\$`L
D$xE1
L$XD
\$@D
L$XH
l$XI
D$T 
9D$@
t$X)
t$xf
t$X9
D$xD
L$XL
D$|)
t$`A
D$T 
D)d$|
T$ H
D$dI
L$@H
\$pE
D$T 
L$hE
D$XE
L$@I
L$@H
D$T 
L$@L
L$ L
T$ H
9t-E
D$d 
D$TA
L$XH
D$xD
D$TA
t$`)
t$pA
D$dM
D$d 
ATUWVSHcY
[^_]A\
[^_]A\
WVSH
 [^_
 [^_H
D$(H
UWVSH
~%Hc
([^_]
D$(H
AWAVAUATUWVSH
(Lci
([^_]A\A]A^A_
UWVSH
([^_]
AVAUATUWVSH
IcD$
 [^_]A\A]A^
AVAUATUWVSH
 HcB
2D90t
 [^_]A\A]A^
WVSHcA
tSD)
WVSH
uIHc
 [^_
T$hD
T$LD
L$(A
T$8L
D$h1
WVSH
D$+H
0[^_
AVAUATUWVSH
0[^_]A\A]A^
L$xL
D$pL
L$xI
|$ A
T$=A
D$<H
|$ A
ATUWVSH
D$>L
D$>H
d$(I
D$ H
@[^_]A\
AVAUATUWVSH
tnE1
d$(I
@[^_]A\A]A^
D$>H
d$(I
l$ H
UWVSH
l$(I
D$ H
L$>I
H[^_]
HKLM\Software\Microsoft\Windows\CurrentVersion
ProgramFilesDir
THM{HiddenClue}
reg query "%s" /v %s
Registry query executed successfully.
Failed to execute registry query.
Argument domain error (DOMAIN)
Argument singularity (SIGN)
Overflow range error (OVERFLOW)
Partial loss of significance (PLOSS)
Total loss of significance (TLOSS)
The result is too small to be represented (UNDERFLOW)
Unknown error
_matherr(): %s in %s(%g, %g)  (retval=%g)
Mingw-w64 runtime failure:
Address %p has no image-section
  VirtualQuery failed for %d bytes at address %p
  VirtualProtect failed with code 0x%x
  Unknown pseudo relocation protocol version %d.
  Unknown pseudo relocation bit size %d.
%d bit pseudo relocation at %p out of range, targeting %p, yielding the value %p.
(null)
Infinity
?aCoc
<2ZGU
vH7B
W4vC
[%Co
O8M2
GCC: (x86_64-posix-seh-rev0, Built by MinGW-Builds project) 14.2.0
DeleteCriticalSection
EnterCriticalSection
GetLastError
InitializeCriticalSection
IsDBCSLeadByteEx
LeaveCriticalSection
MultiByteToWideChar
SetUnhandledExceptionFilter
Sleep
TlsGetValue
VirtualProtect
VirtualQuery
WideCharToMultiByte
__C_specific_handler
___lc_codepage_func
___mb_cur_max_func
__getmainargs
__initenv
__iob_func
__set_app_type
__setusermatherr
_amsg_exit
_cexit
_commode
_errno
_exit
_fmode
_initterm
_lock
_onexit
_unlock
abort
calloc
exit
fprintf
fputc
free
fwrite
localeconv
malloc
memcpy
memset
signal
strerror
strlen
strncmp
system
vfprintf
wcslen
KERNEL32.dll
msvcrt.dll
GNU C17 14.2.0 -mtune=core2 -march=nocona -g -g -g -O2 -O2 -O2 -fno-ident -fbuilding-libgcc -fno-stack-protector
char
long long unsigned int
long long int
short unsigned int
long int
unsigned int
long unsigned int
unsigned char
long double
double
float
short int
ix86_tune_indices
X86_TUNE_SCHEDULE
X86_TUNE_PARTIAL_REG_DEPENDENCY
X86_TUNE_SSE_PARTIAL_REG_DEPENDENCY
X86_TUNE_SSE_PARTIAL_REG_FP_CONVERTS_DEPENDENCY
X86_TUNE_SSE_PARTIAL_REG_CONVERTS_DEPENDENCY
X86_TUNE_DEST_FALSE_DEP_FOR_GLC
X86_TUNE_SSE_SPLIT_REGS
X86_TUNE_PARTIAL_FLAG_REG_STALL
X86_TUNE_MOVX
X86_TUNE_MEMORY_MISMATCH_STALL
X86_TUNE_FUSE_CMP_AND_BRANCH_32
X86_TUNE_FUSE_CMP_AND_BRANCH_64
X86_TUNE_FUSE_CMP_AND_BRANCH_SOFLAGS
X86_TUNE_FUSE_ALU_AND_BRANCH
X86_TUNE_ACCUMULATE_OUTGOING_ARGS
X86_TUNE_PROLOGUE_USING_MOVE
X86_TUNE_EPILOGUE_USING_MOVE
X86_TUNE_USE_LEAVE
X86_TUNE_PUSH_MEMORY
X86_TUNE_SINGLE_PUSH
X86_TUNE_DOUBLE_PUSH
X86_TUNE_SINGLE_POP
X86_TUNE_DOUBLE_POP
X86_TUNE_PAD_SHORT_FUNCTION
X86_TUNE_PAD_RETURNS
X86_TUNE_FOUR_JUMP_LIMIT
X86_TUNE_SOFTWARE_PREFETCHING_BENEFICIAL
X86_TUNE_LCP_STALL
X86_TUNE_READ_MODIFY
X86_TUNE_USE_INCDEC
X86_TUNE_INTEGER_DFMODE_MOVES
X86_TUNE_OPT_AGU
X86_TUNE_AVOID_LEA_FOR_ADDR
X86_TUNE_SLOW_IMUL_IMM32_MEM
X86_TUNE_SLOW_IMUL_IMM8
X86_TUNE_AVOID_MEM_OPND_FOR_CMOVE
X86_TUNE_SINGLE_STRINGOP
X86_TUNE_PREFER_KNOWN_REP_MOVSB_STOSB
X86_TUNE_MISALIGNED_MOVE_STRING_PRO_EPILOGUES
X86_TUNE_USE_SAHF
X86_TUNE_USE_CLTD
X86_TUNE_USE_BT
X86_TUNE_AVOID_FALSE_DEP_FOR_BMI
X86_TUNE_ADJUST_UNROLL
X86_TUNE_ONE_IF_CONV_INSN
X86_TUNE_AVOID_MFENCE
X86_TUNE_EXPAND_ABS
X86_TUNE_USE_HIMODE_FIOP
X86_TUNE_USE_SIMODE_FIOP
X86_TUNE_USE_FFREEP
X86_TUNE_EXT_80387_CONSTANTS
X86_TUNE_GENERAL_REGS_SSE_SPILL
X86_TUNE_SSE_UNALIGNED_LOAD_OPTIMAL
X86_TUNE_SSE_UNALIGNED_STORE_OPTIMAL
X86_TUNE_SSE_PACKED_SINGLE_INSN_OPTIMAL
X86_TUNE_SSE_TYPELESS_STORES
X86_TUNE_SSE_LOAD0_BY_PXOR
X86_TUNE_INTER_UNIT_MOVES_TO_VEC
X86_TUNE_INTER_UNIT_MOVES_FROM_VEC
X86_TUNE_INTER_UNIT_CONVERSIONS
X86_TUNE_SPLIT_MEM_OPND_FOR_FP_CONVERTS
X86_TUNE_USE_VECTOR_FP_CONVERTS
X86_TUNE_USE_VECTOR_CONVERTS
X86_TUNE_SLOW_PSHUFB
X86_TUNE_AVOID_4BYTE_PREFIXES
X86_TUNE_USE_GATHER_2PARTS
X86_TUNE_USE_SCATTER_2PARTS
X86_TUNE_USE_GATHER_4PARTS
X86_TUNE_USE_SCATTER_4PARTS
X86_TUNE_USE_GATHER_8PARTS
X86_TUNE_USE_SCATTER_8PARTS
X86_TUNE_AVOID_128FMA_CHAINS
X86_TUNE_AVOID_256FMA_CHAINS
X86_TUNE_AVOID_512FMA_CHAINS
X86_TUNE_V2DF_REDUCTION_PREFER_HADDPD
X86_TUNE_AVX256_UNALIGNED_LOAD_OPTIMAL
X86_TUNE_AVX256_UNALIGNED_STORE_OPTIMAL
X86_TUNE_AVX256_SPLIT_REGS
X86_TUNE_AVX128_OPTIMAL
X86_TUNE_AVX256_OPTIMAL
X86_TUNE_AVX512_SPLIT_REGS
X86_TUNE_AVX256_MOVE_BY_PIECES
X86_TUNE_AVX256_STORE_BY_PIECES
X86_TUNE_AVX512_MOVE_BY_PIECES
X86_TUNE_AVX512_STORE_BY_PIECES
X86_TUNE_DOUBLE_WITH_ADD
X86_TUNE_ALWAYS_FANCY_MATH_387
X86_TUNE_UNROLL_STRLEN
X86_TUNE_SHIFT1
X86_TUNE_ZERO_EXTEND_WITH_AND
X86_TUNE_PROMOTE_HIMODE_IMUL
X86_TUNE_FAST_PREFIX
X86_TUNE_READ_MODIFY_WRITE
X86_TUNE_MOVE_M1_VIA_OR
X86_TUNE_NOT_UNPAIRABLE
X86_TUNE_PARTIAL_REG_STALL
X86_TUNE_PARTIAL_MEMORY_READ_STALL
X86_TUNE_PROMOTE_QIMODE
X86_TUNE_PROMOTE_HI_REGS
X86_TUNE_HIMODE_MATH
X86_TUNE_SPLIT_LONG_MOVES
X86_TUNE_USE_XCHGB
X86_TUNE_USE_MOV0
X86_TUNE_NOT_VECTORMODE
X86_TUNE_AVOID_VECTOR_DECODE
X86_TUNE_BRANCH_PREDICTION_HINTS_TAKEN
X86_TUNE_BRANCH_PREDICTION_HINTS_NOT_TAKEN
X86_TUNE_QIMODE_MATH
X86_TUNE_PROMOTE_QI_REGS
X86_TUNE_EMIT_VZEROUPPER
X86_TUNE_SLOW_STC
X86_TUNE_USE_RCR
X86_TUNE_LAST
ix86_arch_indices
X86_ARCH_CMOV
X86_ARCH_CMPXCHG
X86_ARCH_CMPXCHG8B
X86_ARCH_XADD
X86_ARCH_BSWAP
X86_ARCH_LAST
signed char
__int128
__int128 unsigned
_Float16
complex _Float16
complex float
complex double
complex long double
_Float128
complex _Float128
func_ptr
__CTOR_LIST__
__DTOR_LIST__
""gY0uKgg0=L""
C:/buildroot/src/gcc-14.2.0/libgcc/config/i386\cygwin.S
C:\buildroot\x86_64-1420-posix-seh-msvcrt-rt_v12-rev0\build\gcc-14.2.0\x86_64-w64-mingw32\libgcc
GNU AS 2.39
C:\buildroot\x86_64-1420-posix-seh-msvcrt-rt_v12-rev0\build\gcc-14.2.0\x86_64-w64-mingw32\libgcc
C:/buildroot/src/gcc-14.2.0/libgcc/config/i386
cygwin.S
C:\buildroot\x86_64-1420-posix-seh-msvcrt-rt_v12-rev0\build\gcc-14.2.0\x86_64-w64-mingw32\libgcc
C:/buildroot/src/gcc-14.2.0/libgcc/libgcc2.c
C:/buildroot/x86_64-1420-posix-seh-msvcrt-rt_v12-rev0/build/gcc-14.2.0/x86_64-w64-mingw32/libgcc
C:/buildroot/src/gcc-14.2.0/libgcc
C:/buildroot/src/gcc-14.2.0/gcc/config/i386
libgcc2.c
i386.h
gbl-ctors.h
libgcc2.c
.file
crtexe.c
envp
argv
argc
mainret
.l_endw
.l_start
.l_end
atexit
.text
.data
.bss
.xdata
.pdata
.file
cygming-crtbeg
.text
.data
.bss
.xdata
.pdata
.file
fprintf
printf
snprintf
main
.text
.data
.bss
.xdata
.pdata
.rdata
.file
gccmain.c
__main
.text
.data
.bss
.xdata
.pdata
.file
natstart.c
.text
.data
.bss
.file
wildcard.c
.text
.data
.bss
.file
dllargv.c
_setargv
.text
.data
.bss
.xdata
.pdata
.file
_newmode.c
.text
.data
.bss
.file
tlssup.c
__xd_a
__xd_z
.text
.data
.bss
.xdata
.pdata
.CRT$XLD@
.CRT$XLC8
.rdata
.CRT$XDZX
.CRT$XDAP
.CRT$XLZH
.CRT$XLA0
.tls$ZZZ
.tls
.file
xncommod.c
.text
.data
.bss
.file
cinitexe.c
.text
.data
.bss
.CRT$XCZ
.CRT$XCA
.CRT$XIZ(
.CRT$XIA
.file
merr.c
_matherr
.text
.data
.bss
.rdata
.xdata
.pdata
.file
CRT_fp10.c
_fpreset
fpreset
.text
.data
.bss
.xdata
.pdata
.file
mingw_helpers.
.text
.data
.bss
.file
pseudo-reloc.c
the_secs
.text
.data
.bss
.rdata
.xdata
.pdata
.file
usermatherr.c
.text
.data
.bss
.xdata
.pdata
.file
xtxtmode.c
.text
.data
.bss
.file
crt_handler.c
.text
.data
.bss
.xdata
.rdata
.pdata
.file
tlsthrd.c
.text
.data
.bss
.xdata
.pdata
.file
tlsmcrt.c
.text
.data
.bss
.file
exit_wrappers.
exit
_exit
.text
.data
.bss
.xdata
.pdata
.file
.text
.data
.bss
.file
pesect.c
.text
.data
.bss
.xdata
.pdata
.file
fake
.text
.data
.bss
.file
libgcc2.c
.text
.data
.bss
.file
mingw_matherr.
.text
.data
.bss
.file
mingw_vfprintf
.text
.data
.bss
.xdata
.pdata
.file
mingw_vsnprint
.text
.data
.bss
.xdata
.pdata
.file
mingw_pformat.
fpi.0
.text
.data
.bss
.xdata
.pdata
.rdata
.file
dmisc.c
.text
.data
.bss
.xdata
.pdata
.file
gdtoa.c
__gdtoa
.text
.data
.bss
.rdata
.xdata
.pdata
.file
gmisc.c
.text
.data
.bss
.xdata
.pdata
.file
misc.c
freelist
p05.0
.text
.data
.bss
.xdata
.pdata
.rdata
.file
strnlen.c
strnlen
.text
.data
.bss
.xdata
.pdata
.file
wcsnlen.c
wcsnlen
.text
.data
.bss
.xdata
.pdata
.file
__p__fmode.c
.text
.data
.bss
.xdata
.pdata
.file
__p__commode.c
.text
.data
.bss
.xdata
.pdata
.file
mingw_lock.c
.text
.data
.bss
.xdata
.pdata
.file
handler
.text
.data
.bss
.xdata
.pdata
.file
acrt_iob_func.
.text
.data
.bss
.xdata
.pdata
.file
wcrtomb.c
wcrtomb
.text
.data
.bss
.xdata
.pdata
.file
mbrtowc.c
mbrtowc
mbrlen
.text
.data
.bss
.xdata
.pdata
.text
.data
.bss
.idata$7p
.idata$5H
.idata$4
.idata$6z
.text
.data
.bss
.idata$7t
.idata$5P
.idata$4
.idata$6
.text
.data
.bss
.idata$7x
.idata$5X
.idata$4
.idata$6
.text
.data
.bss
.idata$7|
.idata$5`
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5h
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5p
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5x
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6&
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$62
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6<
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6D
.text
.data
.bss
.idata$7
.idata$5
.idata$4 
.idata$6N
.text
.data
.bss
.idata$7
.idata$5
.idata$4(
.idata$6Z
.text
.data
.bss
.idata$7
.idata$5
.idata$40
.idata$6b
.text
.data
.bss
.idata$7
.idata$5
.idata$48
.idata$6l
.text
.data
.bss
.idata$7
.idata$5
.idata$4@
.idata$6v
.text
.data
.bss
.idata$7
.idata$5
.idata$4H
.idata$6~
.text
.data
.bss
.idata$7
.idata$5
.idata$4P
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4X
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4`
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4h
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4p
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4x
.idata$6
.text
.data
.bss
.idata$7
.idata$5
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5 
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5(
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$50
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$58
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5@
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5H
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5P
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5X
.idata$4
.idata$6
.text
.data
.bss
.idata$7
.idata$5`
.idata$4
.idata$6 
.file
fake
hname
fthunk
.text
.data
.bss
.idata$2
.idata$4
.idata$5H
.file
fake
.text
.data
.bss
.idata$4
.idata$5h
.idata$7
.text
.data
.bss
.idata$7\
.idata$58
.idata$4
.idata$6d
.text
.data
.bss
.idata$7X
.idata$50
.idata$4
.idata$6T
.text
.data
.bss
.idata$7T
.idata$5(
.idata$4
.idata$6B
.text
.data
.bss
.idata$7P
.idata$5 
.idata$4
.idata$64
.text
.data
.bss
.idata$7L
.idata$5
.idata$4
.idata$6,
.text
.data
.bss
.idata$7H
.idata$5
.idata$4x
.idata$6
.text
.data
.bss
.idata$7D
.idata$5
.idata$4p
.idata$6
.text
.data
.bss
.idata$7@
.idata$5
.idata$4h
.idata$6
.text
.data
.bss
.idata$7<
.idata$5
.idata$4`
.idata$6
.text
.data
.bss
.idata$78
.idata$5
.idata$4X
.idata$6
.text
.data
.bss
.idata$74
.idata$5
.idata$4P
.idata$6
.text
.data
.bss
.idata$70
.idata$5
.idata$4H
.idata$6
.text
.data
.bss
.idata$7,
.idata$5
.idata$4@
.idata$6p
.file
fake
hname
fthunk
.text
.data
.bss
.idata$2
.idata$4@
.idata$5
.file
fake
.text
.data
.bss
.idata$4
.idata$5@
.idata$7`
.file
cygming-crtend
.text
.data
.bss
__xc_z
strerror
_lock
__xl_a
_cexit
wcslen
__xl_d
_tls_end
memcpy
system
malloc
_CRT_MT
abort
__dll__
calloc
fprintf
Sleep
_commodep
__xi_z
signal
strncmp
memset
__xl_z
__end__
__xi_a
__xc_a
_fmode
fputc
__xl_c
_newmodeP
fwrite
_onexit
_errno
strlen
_unlock
vfprintf
free
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_frame
.debug_str
.debug_line_str
__mingw_invalidParameterHandler
pre_c_init
.rdata$.refptr.__mingw_initltsdrot_force
.rdata$.refptr.__mingw_initltsdyn_force
.rdata$.refptr.__mingw_initltssuo_force
.rdata$.refptr.__ImageBase
.rdata$.refptr.__mingw_app_type
managedapp
.rdata$.refptr._fmode
.rdata$.refptr._commode
.rdata$.refptr._MINGW_INSTALL_DEBUG_MATHERR
.rdata$.refptr._matherr
pre_cpp_init
.rdata$.refptr._newmode
startinfo
.rdata$.refptr._dowildcard
__tmainCRTStartup
.rdata$.refptr.__native_startup_lock
.rdata$.refptr.__native_startup_state
has_cctor
.rdata$.refptr.__dyn_tls_init_callback
.rdata$.refptr._gnu_exception_handler
.rdata$.refptr.__mingw_oldexcpt_handler
.rdata$.refptr.__imp___initenv
.rdata$.refptr.__xc_z
.rdata$.refptr.__xc_a
.rdata$.refptr.__xi_z
.rdata$.refptr.__xi_a
WinMainCRTStartup
.l_startw
mainCRTStartup
.CRT$XCAA
.CRT$XIAA
__gcc_register_frame
__gcc_deregister_frame
registryCheck
.rdata$zzz
reg-Read-Value-V1.1.c
__do_global_dtors
__do_global_ctors
.rdata$.refptr.__CTOR_LIST__
initialized
__dyn_tls_dtor
__dyn_tls_init
.rdata$.refptr._CRT_MT
__tlregdtor
__report_error
mark_section_writable
maxSections
_pei386_runtime_relocator
was_init.0
.rdata$.refptr.__RUNTIME_PSEUDO_RELOC_LIST_END__
.rdata$.refptr.__RUNTIME_PSEUDO_RELOC_LIST__
__mingw_raise_matherr
stUserMathErr
__mingw_setusermatherr
_gnu_exception_handler
__mingwthr_run_key_dtors.part.0
__mingwthr_cs
key_dtor_list
___w64_mingwthr_add_key_dtor
__mingwthr_cs_init
___w64_mingwthr_remove_key_dtor
__mingw_TLScallback
.rdata$.refptr.__imp_exit
.rdata$.refptr.__imp__exit
pseudo-reloc-list.c
_ValidateImageBase
_FindPESection
_FindPESectionByName
__mingw_GetSectionForAddress
__mingw_GetSectionCount
_FindPESectionExec
_GetPEImageBase
_IsNonwritableInCurrentImage
__mingw_enum_import_library_names
.debug_info
.debug_abbrev
.debug_line
.debug_line_str
.debug_aranges
.debug_str
.debug_frame
__mingw_vfprintf
__mingw_vsnprintf
__pformat_cvt
__pformat_putc
__pformat_wputchars
__pformat_putchars
__pformat_puts
__pformat_emit_inf_or_nan
__pformat_xint.isra.0
__pformat_int.isra.0
__pformat_emit_radix_point
__pformat_emit_float
__pformat_emit_efloat
__pformat_efloat
__pformat_float
__pformat_gfloat
__pformat_emit_xfloat.isra.0
__mingw_pformat
__rv_alloc_D2A
__nrv_alloc_D2A
__freedtoa
__quorem_D2A
.rdata$.refptr.__tens_D2A
__rshift_D2A
__trailz_D2A
dtoa_lock
dtoa_CS_init
dtoa_CritSec
dtoa_lock_cleanup
__Balloc_D2A
private_mem
pmem_next
__Bfree_D2A
__multadd_D2A
__i2b_D2A
__mult_D2A
__pow5mult_D2A
__lshift_D2A
__cmp_D2A
__diff_D2A
__b2d_D2A
__d2b_D2A
__strcp_D2A
__p__fmode
.rdata$.refptr.__imp__fmode
__p__commode
.rdata$.refptr.__imp__commode
_lock_file
_unlock_file
mingw_get_invalid_parameter_handler
_get_invalid_parameter_handler
mingw_set_invalid_parameter_handler
_set_invalid_parameter_handler
invalid_parameter_handler.c
__acrt_iob_func
__wcrtomb_cp
wcsrtombs
__mbrtowc_cp
internal_mbstate.2
mbsrtowcs
internal_mbstate.1
s_mbstate.0
register_frame_ctor
.text.startup
.xdata.startup
.pdata.startup
.ctors.65535
___RUNTIME_PSEUDO_RELOC_LIST__
__imp_abort
__lib64_libkernel32_a_iname
__data_start__
___DTOR_LIST__
__imp__fmode
__imp____mb_cur_max_func
__imp__lock
IsDBCSLeadByteEx
SetUnhandledExceptionFilter
_head_lib64_libmsvcrt_def_a
.refptr.__mingw_initltsdrot_force
__imp_calloc
__imp___p__fmode
___tls_start__
.refptr.__native_startup_state
GetLastError
__rt_psrelocs_start
__dll_characteristics__
__size_of_stack_commit__
__mingw_module_is_dll
__iob_func
__size_of_stack_reserve__
__major_subsystem_version__
___crt_xl_start__
__imp_DeleteCriticalSection
__imp__set_invalid_parameter_handler
.refptr.__CTOR_LIST__
__imp_fputc
VirtualQuery
___crt_xi_start__
.refptr.__imp__fmode
__imp__amsg_exit
___crt_xi_end__
__imp__errno
.refptr.__imp___initenv
_tls_start
.refptr._matherr
.refptr.__RUNTIME_PSEUDO_RELOC_LIST__
__mingw_oldexcpt_handler
__imp__unlock_file
.refptr.__imp__exit
TlsGetValue
__bss_start__
__imp_MultiByteToWideChar
__imp___C_specific_handler
___RUNTIME_PSEUDO_RELOC_LIST_END__
__size_of_heap_commit__
__imp_GetLastError
.refptr._dowildcard
__mingw_initltsdrot_force
__imp_free
___mb_cur_max_func
.refptr.__mingw_app_type
__mingw_initltssuo_force
__tens_D2A
VirtualProtect
___crt_xp_start__
__imp_LeaveCriticalSection
__C_specific_handler
.refptr.__mingw_oldexcpt_handler
.refptr.__RUNTIME_PSEUDO_RELOC_LIST_END__
___crt_xp_end__
__minor_os_version__
EnterCriticalSection
_MINGW_INSTALL_DEBUG_MATHERR
.refptr.__xi_a
.refptr._CRT_MT
__imp__exit
__section_alignment__
__native_dllmain_reason
_tls_used
__imp_memset
__IAT_end__
__imp__lock_file
__imp_memcpy
__RUNTIME_PSEUDO_RELOC_LIST__
__lib64_libmsvcrt_def_a_iname
__imp_strerror
.refptr._newmode
__data_end__
__imp_fwrite
__CTOR_LIST__
__imp___getmainargs
_head_lib64_libkernel32_a
__bss_end__
__tinytens_D2A
__native_vcclrit_reason
___crt_xc_end__
.refptr.__mingw_initltssuo_force
.refptr.__native_startup_lock
__imp_EnterCriticalSection
_tls_index
__native_startup_state
___crt_xc_start__
__imp_system
___CTOR_LIST__
.refptr.__dyn_tls_init_callback
__imp_signal
.refptr.__mingw_initltsdyn_force
__rt_psrelocs_size
__imp_WideCharToMultiByte
.refptr.__ImageBase
__imp_strlen
__bigtens_D2A
__imp_malloc
.refptr._gnu_exception_handler
__file_alignment__
__imp_InitializeCriticalSection
__getmainargs
InitializeCriticalSection
___lc_codepage_func
__imp_exit
__imp_vfprintf
__major_os_version__
__mingw_pcinit
__imp_IsDBCSLeadByteEx
__imp___initenv
__IAT_start__
__imp__cexit
__imp_SetUnhandledExceptionFilter
__imp__onexit
__DTOR_LIST__
WideCharToMultiByte
__set_app_type
__imp_Sleep
LeaveCriticalSection
__imp___setusermatherr
__size_of_heap_reserve__
___crt_xt_start__
__subsystem__
_amsg_exit
__imp_TlsGetValue
__setusermatherr
.refptr._commode
__imp_fprintf
__mingw_pcppinit
__imp___p__commode
MultiByteToWideChar
__imp_VirtualProtect
___tls_end__
__imp_VirtualQuery
__imp__initterm
__mingw_initltsdyn_force
_dowildcard
__imp___iob_func
__imp_localeconv
localeconv
__dyn_tls_init_callback
_initterm
__imp_strncmp
.refptr._fmode
__imp___acrt_iob_func
__major_image_version__
__loader_flags__
.refptr.__tens_D2A
.refptr.__imp__commode
___chkstk_ms
__native_startup_lock
__imp_wcslen
__imp____lc_codepage_func
__rt_psrelocs_end
__imp__get_invalid_parameter_handler
__minor_subsystem_version__
__minor_image_version__
__imp__unlock
.refptr.__imp_exit
__imp___set_app_type
.refptr.__xc_a
.refptr.__xi_z
.refptr._MINGW_INSTALL_DEBUG_MATHERR
__imp__commode
DeleteCriticalSection
__RUNTIME_PSEUDO_RELOC_LIST_END__
.refptr.__xc_z
___crt_xt_end__
__mingw_app_type


+------------------------------------+
| FLOSS STATIC STRINGS: UTF-16LE (1) |
+------------------------------------+

(null)


 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 
  FLOSS STACK STRINGS (0)  
 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 



 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 
  FLOSS TIGHT STRINGS (1)  
 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 

F0056514


 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 
  FLOSS DECODED STRINGS (0)  
 ΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇΓöÇ 

```
Then I used `Ctrl+F` to find the flag:
![image](https://github.com/user-attachments/assets/35006b77-83de-4718-8a96-2404c9c1d723)
This gave me the flag:`THM{HiddenClue}`                     
Then I used `cat YaraMatches.txt` to get the malware detected:
![image](https://github.com/user-attachments/assets/0edb17c7-df12-4413-8ddd-7ccbb8c0c6d4)

Then used the windows log viewer by navigating to `Sysmon`:                             
There I edited the XML query to:                      
```
<QueryList>
  <Query Id="0" Path="Microsoft-Windows-Sysmon/Operational">
    <Select Path="Microsoft-Windows-Sysmon/Operational">
      *[System[(EventRecordID="INSERT_EVENT_record_ID_HERE")]]
    </Select>
  </Query>
</QueryList>
```
and used the record id of: `128016`                   
![image](https://github.com/user-attachments/assets/c3a9a5ba-52be-43f2-9a03-b8dab7798604)
This helps me get information of processes created.                                  
Then solved given questions using this information gained throught the challenge:
![image](https://github.com/user-attachments/assets/5fbb88aa-50f1-4a74-ac65-3cbc3cde075a)

