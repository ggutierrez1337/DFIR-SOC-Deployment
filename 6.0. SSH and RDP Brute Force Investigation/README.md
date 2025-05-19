# 6.0. SSH and RDP Brute Force Investigation

## SSH Brute Force Investigation

6.0. In the Elastic menu under **Security,** select **Alerts**. It can be seen that today alone, there has been 108 alerts generated due to SSH brute force activity

![Screenshot_(1140)](https://github.com/user-attachments/assets/f0e735c0-ffe2-4505-9922-5016ce878f22)

![Screenshot_(1141)](https://github.com/user-attachments/assets/0150acae-488b-420e-b3b6-520983719781)

**Note -** When investigating an SSH Bruteforce, there are some indicators or sources to look for

- **IP** - Is this IP known to perform brute force activities?
- Were there any other users affected by this IP?
- Were any of the attempts successful?
    - If so, what activities occurred after the successful login?

6.1. Expand one of the alerts. Information such as IP and username used to try and access the SSH server can be seen. Check to see if this IP is known to perform such activities or is a known malicious IP using AbuseIPDB 
    - It can be seen that this IP is in fact malicious and has been reported **57,767** and confidence abuse of **100%** times and mainly involved in **SSH Brute force activity**

![Screenshot_(1142)](https://github.com/user-attachments/assets/fa88ec8b-e133-4ab0-a829-0d91f64094ff)

![Screenshot_(1143)](https://github.com/user-attachments/assets/832518ea-bf10-4d6e-adbf-d689ce3239ba)

![image](https://github.com/user-attachments/assets/de9922c3-3510-4b16-9af9-312973d545ea)

6.2. Greynoise can also be used to investigate and verify potentially malicious IPs as well, which in this case has confirmed this IP is malicious as well

![Screenshot_(1145)](https://github.com/user-attachments/assets/2893cc92-9076-4bf8-b137-bd7a1c9e537a)

6.3. Next check if any other users are affected by this IP. In Kibana, search the IP. In the last 30 days, there has been **33,167 events** pertaining to this specific IP.  In this case, the only target for this attack was **root.** No other users such as oracle, guest, test, etc

![Screenshot_(1146)](https://github.com/user-attachments/assets/40c8ad8e-da45-4d6a-a3cd-fee4fe34c893)

![Screenshot_(1147)](https://github.com/user-attachments/assets/f059a94f-dbb4-4afc-86e7-b088865026b1)

6.4. Expanding on the IP address in the search, check to see if any of these brute force attempts were **accepted**. Of all the attempts made on the SSH Server, none were accepted by this IP

![Screenshot_(1148)](https://github.com/user-attachments/assets/f8bd42a0-d802-4cb4-abec-0ca6c999d949)

6.5. Since no attempts were successful and the attacker could not gain access to the machine to further any attacks, this alert can be closed and the IP can be blocked from trying to continue its attack

**Note -** In the real world this alert could also be found in ticketing system where the ticket could be closed and notes input about this alert and findings

6.6. The next step will be to block the IP from the network and reporting this malicious IP. After doing so, respond to the claimed ticket in osTicket with actions taken and close out the ticket.

![image 1](https://github.com/user-attachments/assets/5460fcd3-8071-4dbf-bb29-64eb57ebc7dc)

![image 2](https://github.com/user-attachments/assets/ba05cc31-76d9-47b8-a1c1-43b05fc5f233)

![image 3](https://github.com/user-attachments/assets/7d97c170-5f83-483d-8ba6-47e44d03d84e)

## RDP Brute Force Investigation

6.7. In Elastic, go to **Security → Alerts**. In the past 30 days, it can been that there has been **578 alerts** with **44** of those being RDP. 

![image 4](https://github.com/user-attachments/assets/6d9bf4c8-27dd-4c9f-815e-bd8daf7f2381)

![image 5](https://github.com/user-attachments/assets/d1a52109-6b95-4e1b-893a-ad5b95782020)

![image 6](https://github.com/user-attachments/assets/672746ce-b3ca-4a24-8b33-c0921e27daab)

6.8. Select the RDP Brute Force Alert and filter it that way only those alerts are shown. Expand one of the alerts, and it can be seen that **SRC IP 217.160.125.6** tried to Brute Force the Windows Server using username **Administrator** a total of **5 times**

![image 7](https://github.com/user-attachments/assets/2fb7f0dc-e986-4ca9-a660-507cc22801e9)

6.9. Go to SSH Rules settings and copy the **Body** section of the **Action** since tickets should be generated for RDP Brute Force alerts. Repeat the process used for creating an action to generate tickets in osTicket

![image 8](https://github.com/user-attachments/assets/72a2a970-66af-4024-b5b3-39d22367a34a)

6.1.0. Check if the IP is known for this type activity using AbuseIPDB. It can be seen that this IP has been reported **17 times** regarding SSH and RDP Brute Force Activity 

![image.png](attachment:a428ff97-ca2e-4c76-8ed9-c6ad189fab90:image.png)

6.1.1. Using Elasticsearch, lookup the IP address to check if other usernames/user accounts might have been used as well. In this case, only **Administrator** was used and none of the attempts were successful

![image.png](attachment:cc63eb7f-9aca-4de0-bdcb-c40967ca654b:image.png)

**Note -** If the authentication had been successful, taking down the **time and date** of logon helps 

6.1.2. In OsTIcket, respond to the alert and ticket created according to your triage and actions that should be taken if necessary

![Screenshot (1169).png](attachment:a0371672-da47-4f7c-8f20-60d9d052ba01:Screenshot_(1169).png)

![Screenshot (1171).png](attachment:829db155-051f-4989-8ae2-75f1db19d50d:Screenshot_(1171).png)
