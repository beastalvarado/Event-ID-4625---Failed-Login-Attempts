# EventID-4625 Failed Login Attempts
---
## Objective
This project demonstrates how to detect Windows Failed Login Attempts (Event ID 4625) using Splunk. I simulated failed logins, analyzed the logs with SPL, and created a real-time alert to detect repeated login failures. This shows how to apply threat detection in a SIEM environment.
---
### Tools Used
-Windows Server / Windows Workstation
Log source for simulating failed login attempts (Event ID 4625).

-Splunk Enterprise / Splunk Free
Security Information and Event Management (SIEM) platform for log analysis and detection.

-NxLog
Log forwarding agent to send Windows Event Logs to Splunk.

-VirtualBox / VMware Workstation
Virtualization platform to run lab environments.

---
### Network Diagram 

![ChatGPT Image May 9, 2025, 05_53_28 PM](https://github.com/user-attachments/assets/398648e4-af32-4dfd-a7b4-78ee83c6e632)
---
### 🧪 Project Walkthrough
1. Simulating the Activity
To generate real log data, I simulated multiple failed login attempts by using Kali Linux to remotely attempt logins to my Windows Server using incorrect credentials. This activity generates Event ID 4625 in the Windows Security Logs.
2.Collecting and Analyzing the Logs
After simulating the activity, I switched to my Ubuntu Server where I have Splunk running to collect and analyze the Windows Event Logs. Using Splunk, I began reviewing the data to answer key security investigation questions.
---

🧠 What Happened?
I used the stats count command to quickly check how many failed login attempts were captured.
  
🚨This confirmed that 10 failed login events (Event ID 4625) were generated.

 ![Screenshot 2025-05-10 075649](https://github.com/user-attachments/assets/da4acfcf-6543-4882-9de4-9483290604c6)
---
🧠 Who Did It and When Did It Happen?
I created a table to display the timestamp, source IP address, and source port for each failed login event.
All attempts originated from the source IP 192.168.1.13, which belongs to my Kali Linux machine. 

🚨The attempts occurred within seconds of each other, indicating a potential brute-force attempt simulation.

![Screenshot 2025-05-10 080622](https://github.com/user-attachments/assets/c2d96ee9-5bbc-4c9b-91f0-9c05f9152c45)

🧠 Who Was Targeted?
I expanded the table to include the Account Domain and Account Name fields. This helped me confirm that the targeted account was "Administrator" on the "WINDOWS-DC0" domain.

🚨This is concerning because the Administrator account is a high-privilege target that attackers often go after to gain full control of a system.
![Screenshot 2025-05-10 083828](https://github.com/user-attachments/assets/9c7ded0d-3dd6-465a-881a-1e87c20d6e2d)


🧠 How Many Times Did It Happen?
After identifying the source IP, domain, and targeted account, I refined my SPL to focus specifically on those details and then counted the number of matching events to confirm the activity. The query confirmed that there were 10 failed login attempts targeting the Administrator account from the Kali Linux machine.

🚨Since these attempts happened in rapid succession, this could indicate brute-force behavior rather than a one-time user error.
![Screenshot 2025-05-10 084528](https://github.com/user-attachments/assets/4df94dc9-0613-426c-ad95-1b395bd37221) 

🧩 Additional Findings 

Logon Type: I noticed that the failed logins were using Logon Type 3 (Network Logon), which indicates attempts to log in over the network rather than locally. This aligns with the fact that the attempts came from another machine (Kali Linux).
![Screenshot 2025-05-10 090017](https://github.com/user-attachments/assets/f7739add-f8eb-4ffe-9f4c-fc9d6e6e8693)

Failure Reason: The logs provided a failure reason code, indicating that the logins failed due to unknown username or bad password. This further confirms that the attempts were unauthorized.

![Screenshot 2025-05-10 090244](https://github.com/user-attachments/assets/a94f9175-f45a-4ec1-9ef3-20a8a6aee2a8)
---

### 🚨 Alert Creation
After configuring the "Failed Logon Alert - 4625", I successfully tested and triggered the alert based on the simulated failed login attempts.

✅ Alert Details:
Alert Name: Failed Logon Alert - 4625

Description: Detects 5 or more failed login attempts from the same Source IP and Account Name within the scheduled search window.

Alert Type: Real-Time

Trigger Condition: Per-Result (≥ 5 failed attempts)

Actions:

Added to Triggered Alerts

Email Notification to SOC Team

Status: Enabled
![Screenshot 2025-05-10 091908](https://github.com/user-attachments/assets/17445dcc-a344-48ad-a9c5-b37d81b564ed)

## 📝 Summary & Key Takeaways

This project demonstrated my ability to perform a complete detection lifecycle by:

- ✅ Simulating failed login attempts (Event ID 4625) using a Kali Linux attacker machine.
- ✅ Investigating the incident in Splunk using relevant fields such as Account Name, Source IP, and Logon Type.
- ✅ Confirming that the behavior represented a brute-force attack targeting the Administrator account.
- ✅ Creating and testing a real-time Splunk alert to detect future occurrences.

### 🎓 Key Takeaways
- Gained hands-on experience in Windows log analysis and SIEM investigation.
- Developed SPL skills to search, filter, and correlate events.
- Practiced SOC-level analytical thinking through question-based investigation.
- Built an actionable detection rule aligned with real-world brute-force behavior.


---
### Skills Learned 
- SIEM Use Case Development.
- Windows Event Log Analysis.
- Log Collection & Forwarding.
- Security Monitoring & Threat Detection.
- Splunk Search Processing Language (SPL).
---
