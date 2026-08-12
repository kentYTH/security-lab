# Insider Investigation — CyberDefenders

**Date:** August 2026  
**Analyst:** Kent Yam  
**Platform:** CyberDefenders  
**Challenge:** Insider  
**Tools Used:** FTK Imager  

---

## Case Summary

A Linux machine belonging to a user named Karen is suspected of 
malicious activity including data exfiltration and launching attacks 
against other systems. A forensic disk image was provided for investigation.

---

## Investigation Findings

### Q1 — Which Linux distribution is being used on this machine?
**Answer:** Kali

**Finding:** The Linux distribution that is being used on the machines is Kali Linux.

**Evidence location:** /boot/

**Method:** Navigated to /boot in FTK Imager and inspected the list of files there which showed files such as "config-4.13.0-kali1-amd64" 

### Q2 — What is the MD5 hash of the Apache access.log file?
**Answer:** d41d8cd98f00b204e9800998ecf8427e
**Finding:** The MD5 hash of the file is d41d8cd98f00b204e9800998ecf8427e.
**Evidence location:** /var/log/apache2/access.log
**Method:** Exported file hash of access.log using FTK Imager and inspected extracted file for hash values.

### Q3 — It is suspected that a credential dumping tool was downloaded. What is the name of the downloaded file?
**Answer:** mimikatz_trunk.zip 
**Finding:**  mimikatz_trunk.zip was found in the Downloads file directory which iss a widely used post-exploitation tool designed to extract plaintext passwords, hashes, PINs, and Kerberos tickets from system memory. It can also perform pass-the-hash attacks, manipulate authentication tokens, and export certificates, making it a powerful tool for credential theft.
**Evidence location:** /root/Downloads/mimikatz_trunk.zip
**Method:** Navigated to /root/Downloads/ directory, which commonly stores files downloaded by the user.

### Q4 — A super-secret file was created. What is the absolute path to this file?
**Answer:** /root/Desktop/SuperSecretFile.txt
**Finding:** By examining the .bash_history file, we can Identify the absolute path for the SuperSecretFile.txt along with other commands.
**Evidence location:** /root/.bash_history
**Method:**  By analyzing the .bash_history file in the /root/ directory, we uncover several commands executed by the user. Among them, we find commands that specifically create and write to a file referred to as a "super-secret" file.

### Q5 — Program That Used didyouthinkwedmakeiteasy.jpg
**Answer:** binwalk
**Finding:** binwalk was found running didyouthinkwedmakeiteasy.jpg. Binwalk is a fast command-line tool used to search binary images for embedded files, file systems, and executable code. It is primarily designed for reverse engineering and extracting the contents of firmware images from IoT and smart devices. 
**Evidence location:** /root/.bash_histoy
**Method:** Navigated the /root/.bash_history and analysed the commands that was executed and found binwalk didyouthinkwedmakeiteasy.jpg was executed.

### Q6 — What is the third goal from the checklist Karen created?
**Answer:** profit
**Finding:** A file called checklist was found, the list contains "Gain Bob's Trust", "Learn how to hack", "Profit".
**Evidence location:** /root/Desktop/Checklist
**Method:** Navigated to Desktop, where 2 files were found. Containing 2 files including a file called "Checklist".

### Q7 — How many times was apache run?
**Answer:** 0
**Finding:** The logs in /var/log/apache2/ all have a size of 0 bytes, indicating that Apache was never run on this system.
**Evidence location:** /var/log/apache2/
**Method:** Navigated and analysed files in /var/log/apache2/

### Q8 — It is believed this machine was used to attack another. What file proves this?
**Answer:** irZLAohL.jpeg
**Finding:** Flightsim was found running in a terminal window with escalated privileges. Flightsim is an application which generates malicious network traffic for security. 
**Evidence location:** /root/irZLAohL.jpeg
**Method:** On the root directory, irZLAohL.jpeg was found where an attack was running in the command line with Administrator priveleges.

### Q9 — Within the Documents file path, it is believed that Karen was taunting a fellow computer expert through a bash script. Who was Karen taunting?
**Answer:** Young
**Finding:** [name] Young's name was found in one of the script files."Heck yeah! I can write bash too Young" was found in firstscript_fixed file.
**Evidence location:** /root/Documents/myfirsthack/firstscript_fixed
**Method:** By analysing the .bash_history  

### Q10 — A user su'd to root at 11:26 multiple times. Who was it?
**Answer:** postgres 
**Finding:** In this investigation, the auth.log file reveals multiple entries indicating that a user escalated privileges using the su command at 11:26. Specifically, the logs show the following entry. Mar 20 11:26:22 KarenHacker su[4060]: Successful su for postgres by root
**Evidence location:** /var/log/auth.log
**Method:** Navigated to /var/log/auth.log and analysed timestamp 11:26.

### Q11 — Current Working Directory From Bash History
**Answer:** /root/Documents/myfirsthack/
**Evidence location:** /root/.bash_history
**Method:** Analysed chronology of commands executed in .bash_history.

---

## Conclusion

Forensic analysis of the disk image reveals a clear pattern of malicious and suspicious activity attributed to the user Karen.

Karen downloaded Mimikatz — a well-known credential dumping tool used to extract authentication credentials from system memory — indicating deliberate intent to harvest credentials for unauthorized access. Evidence also shows Karen used FlightSim to simulate and launch attacks against external machines, suggesting this system was used as an attack platform against other targets.

Additionally Karen left a taunting message directed at a fellow computer expert within a bash script in the Documents directory, indicating awareness of being investigated or monitored. Most critically Karen successfully executed the su command to escalate privileges to the postgres database account, which raises serious concerns around unauthorized data manipulation, information extraction, and configuration of unauthorized database access.

Taken together the evidence paints a picture of a user who was actively engaged in credential theft, external attack activity, and unauthorized privilege escalation — all hallmarks of a malicious insider threat.

Key evidence includes:
* Mimikatz credential dumping tool found in the downloads directory, indicating deliberate intent to harvest system credentials
* FlightSim tool execution logs confirming this machine was used to launch attacks against external targets
* Bash script in the Documents directory containing a taunting message directed at a named individual, suggesting awareness of investigation
* Auth log entries showing repeated su command execution to the postgres account at 11:26, indicating unauthorized privilege escalation attempts
---

## Tools Used

- FTK Imager — disk image analysis and file system navigation
- CyberDefenders platform — challenge environment

---

## Key Learnings

- **Authentication log analysis (auth.log):** The auth.log file records 
all authentication events on a Linux system including successful and 
failed login attempts, su command executions, and privilege escalation 
activity. During this investigation auth.log revealed repeated su 
command executions to the postgres account at a specific timestamp — 
demonstrating how authentication logs can precisely identify when and 
how many times a user attempted to gain elevated privileges, making it 
a critical artifact in any insider threat investigation.

- **Bash history analysis:** The .bash_history file in a user's home 
directory preserves a sequential record of commands executed in the 
terminal. This is one of the most valuable artifacts in Linux forensics 
as it can reveal attacker tools downloaded, commands run, and the 
current working directory at the time of investigation — even when the 
user attempts to cover their tracks.

- **Log file investigation:** Apache access logs and authentication logs 
like auth.log record detailed activity including how many times a 
service was run and which users executed privilege escalation commands 
like su. Cross-referencing multiple log sources helps build a more 
complete picture of user activity and timeline.

- **MD5 hashing for evidence integrity:** Verifying the MD5 hash of 
evidence files confirms they have not been tampered with since 
collection. This is a fundamental step in maintaining forensic integrity 
and chain of custody — any change to the file produces a completely 
different hash.
