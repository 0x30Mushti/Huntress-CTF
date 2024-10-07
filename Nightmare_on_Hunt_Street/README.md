# Nightmare on Hunt Street

## Description 
```
DeeDee hears the screams,
In the logs, a chilling trace—
Freddy's waiting near.
```

# EVTX File Analysis Guide

## Downloading EVTX Files

Start by downloading the EVTX files. To manage multiple EVTX files efficiently, I utilize two key tools:

**EVTX to CSV Converter**: You can find it on [GitHub](https://github.com/EricZimmerman/evtx). 

To convert the EVTX files to CSV format, use the following command:
```
EvtxECmd.exe -d "path/to/folder/with/evtx/files" --csvf "path/to/save/output/file/filename.csv"
```
(Here, -d specifies the folder containing the EVTX files, if you are converting single file you can use -f ).

## Analyzing Events
Once you have a CSV file with all events, I recommend using Timeline Explorer. It handles CSV files quickly and efficiently.

If you’re new to Event IDs, you can refer to this helpful resource: [Ultimate Windows Security Encyclopedia.](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/)

## Questions and Analysis
# Question 1: What is the IP address of the host that the attacker used?

I start by looking for successful logins since a threat actor attempting Remote Code Execution (RCE) will have their IP address logged if they managed to log in.

Filter: Map Description = Successful logon (Event ID 4624 can also be used).
Result: The remote host IP is REDACTED

# Question 2: How many times was the compromised account brute-forced?
Next, I check for failed login attempts.

Filter: Map Description = Failed logon (Event ID 4625 can also be used).
Result: The number of failed logins is REDACTED.

# Question 3: What is the name of the offensive security tool that was used to gain initial access? 
This question is a bit tricky and may require some Googling.

I consider that the account was accessed over the Network, indicated by Logon Type = 3.

We also see evidence of RCE, along with logs that have been deleted using the command: wevtutil.exe cl Security.

The wevtutil.exe command can be executed by two applications: cmd and PowerShell. I tested rEDACTED to access them remotely, and that turned out to be correct.

Answer: REDACTED

# Question 4: How many unique enumeration commands were run with net.exe? 
Under Executable Info, you can find command line details.

Locate net.exe.

Collect all instances of net.exe and look for unique COMMANDS. Too se which command net.exe use you can typ "net help" in cmd, correlate those with the commands found in the cmdline. 

Answer: REDACTED

# Question 5: What password was successfully given to the user created?
The answer to this question is found in the same location as the question before (net).

Answer: REDACTED










