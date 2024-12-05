# <ins>Day 2: One man's false positive is another man's potpourri.</ins>

## <ins>Challenge Objective:</ins>
> Investigate alerts triggered on **December 1, 2024**, between **0900 and 0930** using the Elastic SIEM tool.         
> Determine whether they are **True Positives (TP)** or **False Positives (FP)**.   


## <ins>Walkthrough:</ins>   

### <ins>Accessing the Elastic SIEM:</ins>
**Login Details:**
   - URL: `https://LAB_WEB_URL.p.thmlabs.com`
   - Username: `elastic`(given)
   - Password: `elastic`(given)
![image](https://github.com/user-attachments/assets/c48da53f-c0b8-486a-864d-a73ebd67c2da)     
### <ins> Setting a proper timeframe:</ins>
After login I went over to the **Discover** tab to see events and set the time range to **Dec 1, 2024, 0900 - 0930**(using absolute).

![image](https://github.com/user-attachments/assets/270fdca2-8a87-49fc-8daf-e80394c007a4)  

### <ins>Adding Fields:</ins>
To better analyze the logs:
- I added the following fields as columns:
  - `host.hostname`: Host where the command was run.
  - `user.name`: User executing the command.
  - `event.category`: Category of the event.
  - `process.command_line`: Details of the PowerShell commands executed.
  - `event.outcome`: Status of the command execution.

   
![image](https://github.com/user-attachments/assets/ec1cabdd-23b3-4aac-883d-c25fa730c557)   

### <ins>Investigating the Source:</ins>
1. Added the `source.ip` field.
2. Filtered for **authentication events**:
   - Found an unusual tries in failed login attempts from **IP 10.0.11.11**.
   - Then Found a successful login followed by the execution of encoded PowerShell command.

Then I expanded timeframe and found the **IP: `10.0.255.1`**


### <ins>Decoding the PowerShell Command:</ins>
Used **CyberChef** to decode the command.   
![image](https://github.com/user-attachments/assets/c54c24aa-f49b-4e43-ae14-fedf52da3725)   


### <ins>Questions:</ins>
Using all of this Information I answered these questions:   
![image](https://github.com/user-attachments/assets/6dfa9585-cbda-4b8a-bbcb-fb475ed63cef)




