# Incident-handling-with-Splunk-Action-Objective

![image](https://github.com/user-attachments/assets/3bd2bd7d-e62a-4681-a6da-0b08d09a6d0c)

## Project Report: Investigating the Cyber Attack on Wayne Enterprises

### 1. Overview

Wayne Enterprises recently experienced a cyber attack in which attackers infiltrated their network, compromised their web server, and successfully defaced their website, http://www.imreallynotbatman.com. The defaced site displayed a message, "YOUR SITE HAS BEEN DEFACED," along with the attackers' trademark. I was engaged as a Security Analyst to investigate the incident, identify the root cause, and trace all attacker activities within the network.

### 2. Action on Objectives

Given that the website was defaced, it was crucial to determine what caused the defacement. My first step was to analyze the traffic flow that could lead to identifying the root cause of the defacement.

I began the investigation by examining the Suricata log source, focusing on the IP addresses communicating with the compromised web server (192.168.250.70).

    Search Query: index=botsv1 dest=192.168.250.70 sourcetype=suricata

![image](https://github.com/user-attachments/assets/126069ea-fdcd-49b6-b636-e90b17ac3ea5)

The initial logs did not show any external IPs communicating with the server. Therefore, I adjusted the search to analyze traffic originating from the web server.

    Search Query: index=botsv1 src=192.168.250.70 sourcetype=suricata

![image](https://github.com/user-attachments/assets/4f12cd8f-7386-4f46-aa63-f42e19e7ba7a)

Interestingly, the output revealed that the web server was initiating outbound traffic to three external IPs. Typically, web servers do not originate traffic; this is usually done by the client. The large amount of traffic to these external IPs warranted further investigation.

I then pivoted into the destination IPs one by one to examine the nature of the traffic.

    Search Query: index=botsv1 src=192.168.250.70 sourcetype=suricata dest_ip=23.22.63.114


![image](https://github.com/user-attachments/assets/605660c7-377e-47b0-876a-bb36ea7b2e78)

This search revealed two PHP files and a JPEG file. The JPEG file, in particular, seemed suspicious. I refined the search to trace the origin of this JPEG file.

    Search Query: index=botsv1 url="/poisonivy-is-coming-for-you-batman.jpeg" dest_ip="192.168.250.70" | table _time src dest_ip http.hostname url

![image](https://github.com/user-attachments/assets/6d7d4574-0b6c-4fa2-bf37-21527e5b848e)

The results clearly showed that a suspicious JPEG file, "poisonivy-is-coming-for-you-batman.jpeg," was downloaded from the attacker's host (prankglassinebracket.jumpingcrab.com) and used to deface the site.

![image](https://github.com/user-attachments/assets/71154e03-2e6f-47ba-81ef-533ed0e17ade)


### Answer the questions below:

#### 1. What is the name of the file that defaced the imreallynotbatman.com website?

![Screenshot from 2024-08-24 12-16-46](https://github.com/user-attachments/assets/bbe3f385-94dd-4eb9-a62b-7d43d696eca5)


        Answer: poisonivy-is-coming-for-you-batman.jpeg

#### 2. Fortigate Firewall 'fortigate_utm' detected an SQL attempt from the attacker's IP 40.80.148.42. What is the name of the rule that was triggered during the SQL Injection attempt?


![image](https://github.com/user-attachments/assets/4f13f69c-3804-48b3-8dcb-79c93a660900)

        
        Answer: HTTP.URI.SQL.Injection

3. Conclusion

This investigation provided a comprehensive analysis of the cyber attack on Wayne Enterprises, uncovering the root cause of the website defacement. By carefully analyzing the traffic logs and identifying the malicious JPEG file used in the attack, I was able to trace the attacker's activities back to their source. Additionally, the detection of the SQL injection attempt through Fortigate Firewall logs highlighted the need for robust security measures and continuous monitoring.
4. What I Learned

Through this project, I gained valuable experience in using Splunk for log analysis and threat detection. The process of identifying malicious files, tracing attacker activities, and understanding the flow of network traffic deepened my knowledge of cybersecurity investigation techniques. I also learned the importance of using multiple log sources, such as Suricata and Fortigate, to gain a complete picture of an attack.
5. Benefits of the Project

This project enhanced my skills in incident response, particularly in the areas of log analysis and root cause identification. It reinforced the importance of a proactive approach to cybersecurity, including the need for regular monitoring, strong password policies, and timely patching of vulnerabilities. The experience I gained will be invaluable in future investigations and will contribute to my growth as a cybersecurity professional.
