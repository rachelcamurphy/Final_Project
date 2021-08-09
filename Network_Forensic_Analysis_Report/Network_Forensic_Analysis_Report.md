# Network Forensic Analysis Report

## Time Thieves 
You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users' custom site? `FRANK-N-TED.COM.v.t0r0o` `Frank-nTed-DC-.frank-n-ted.com0`
2. What is the IP address of the Domain Controller (DC) of the AD network? `DC=frank-n-ted` `10.6.12.12`
3. What is the name of the malware downloaded to the 10.6.12.203 machine? `june11.dll` and `Trojan.Yakes`
   - Once you have found the file, export it to your Kali machine's desktop.
4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/). 
5. What kind of malware is this classified as? Trojan
![](https://github.com/rachelcamurphy/Final_Project/blob/main/Network_Forensic_Analysis_Report/Images/virus_total_screenshot_day2_wireshark.PNG)
---

## Vulnerable Windows Machine

1. Find the following information about the infected Windows machine:
    - Host name: `CNameString: ROTTERDAM-PC$`
    - IP address: Source: `172.16.4.4` Destination:`172.16.4.205`
    - MAC address: `LenovoEM_b0:63:a4 (00:59:07:b0:63:a4)`
    
2. What is the username of the Windows user whose computer is infected? `username: matthijs.devries`
3. What are the IP addresses used in the actual infection traffic? `172.16.4.205, 185.243.115.84, 166.62.11.64`
4. As a bonus, retrieve the desktop background of the Windows host. 
![](https://github.com/rachelcamurphy/Final_Project/blob/main/Network_Forensic_Analysis_Report/Images/Desktop_Background_Windows_User.png)

---

## Illegal Downloads

1. Find the following information about the machine with IP address `10.0.0.201`:
    - MAC address:`00:16:17:18:66:c8`
    - Windows username: `elmer.blanco`
    - OS version: `Windows NT 10.0; Win64; x64`

2. Which torrent file did the user download? `Betty Boop - Rythm on the Reservation`
    ![](https://github.com/rachelcamurphy/Final_Project/blob/main/Network_Forensic_Analysis_Report/Images/bettyboop_wireshark.PNG)
