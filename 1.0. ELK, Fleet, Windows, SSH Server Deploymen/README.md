# Elasticsearch Setup

1. Deploy an Ubuntu server with 80GB of storage and 16GB of RAM
2. Once the server has been deployed, SSH into the machine and update it 

```powershell
ssh root@108.61.119.236
apt update && upgrade -y
```

1. In Google, search **Download Elasticsearch** and select the option
2. Select **deb x86_64** then right click the **Download** button and select, **Copy Link Address**

![Screenshot (776).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/ae4ea87b-0944-4e15-92b1-110ec365df3d/Screenshot_(776).png)

1. Back in the server, input the following command

```powershell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.17.0-amd64.deb
```

![Screenshot (778).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/957a2118-2499-4d93-b35d-5076043f5141/Screenshot_(778).png)

1. Install elasticsearch

```powershell
dpkg -i elasticsearch-8.17.0.amd64.deb
```

![Screenshot (779).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/d6fa9653-20d9-46f7-a97d-6b4b5f67f579/Screenshot_(779).png)

1. Under **Security autoconfiguration information,** a default password will be provided for the superuser. Make sure to save the password

1. Change into the directory where the configuration file for elasticsearch is stored and nano into the **elasticsearch.yml** file

```powershell
cd /etc/elasticsearch
nano elasticsearch.yml
```

1. In order for the SOC Machine to reach the elasticsearch server, the Public IP of the server needs to be input in the .yml file under **network.host**
    - Ensure to uncomment [**network.host](http://network.host)** and **http.port**
    - Once finished, select CTRL + X + Y + ENTER

![Screenshot (782).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/4e45f34d-f080-486f-b9fb-c22c0e81fee6/Screenshot_(782).png)

1. Start up Elasticsearch

```powershell
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
systemctl status elasticsearch.service
```

![Screenshot (783).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/76444cb5-cad9-4eb9-98fe-575c8ecc8d12/Screenshot_(783).png)

## Setting up Kibana

1. Navigate to https://www.elastic.co/downloads/kibana
2. Select the Download type as **Deb 86 x64** and copy the link address to the server instance

```powershell
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.17.0-amd64.deb
```

![Screenshot (784).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/4b94cdcf-bfa0-4cfb-acd8-ab41e45d4eb9/Screenshot_(784).png)

1. Install Kibana

```powershell
dpkg -i kibana-8.17.0-amd64.deb
```

![Screenshot (785).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/66bf6534-2f97-4f3c-bdce-6a668100e3c8/Screenshot_(785).png)

1. Go to the config file of Kibana
    - Uncomment **server.port** and **server.host**
    - For server host, input the **Public IP** of the VM

```powershell
nano /etc/kibana/kibana.yml
```

![Screenshot (786).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/604377de-6619-4d64-9d84-623f053715de/Screenshot_(786).png)

1. Enable and start Kibana

```powershell
systemctl daemon-reload
systemctl enable kibana.service
systemctl start kibana.service
systemctl status kibana.service
```

![Screenshot (787).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/8f6d1c23-bf3a-4bb1-ae58-d4f154604014/Screenshot_(787).png)

1. Generate an elasticsearch enrollment token for Kibana
2. cd to the **/bin** directory of elasticsearch
    - The one we’re interested in is **elasticsearch-create-enrollment-token**

```powershell
cd /usr/share/elasticsearch/bin
./elasticsearch-create-enrollment-token --scope kibana
```

![Screenshot (788).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/2edab103-36c4-47a1-bcb8-53d87b6ffe0c/Screenshot_(788).png)

![Screenshot (789).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/e1779777-0551-4933-8e03-0454252ee7d0/Screenshot_(789).png)

1. Save the enrollment token as it will be used for later use
2. Under firewall rules, allow access from the Public IP and Port. Make sure to do it within the Ubuntu Server as well

```powershell
ufw allow 5601
```

1. Access the Kibana instance by inputting in the following in the URL - **Server_IP:5601** and paste the enrollment token generated from earlier

![Screenshot (791).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/da555110-e8c5-4124-bc5a-06f9fb474dcc/Screenshot_(791).png)

1. Back in the server instance, cd to **/bin** of Kibana. The one of interest is **kibana-verification-code.** A verification code will then be generated for Kibana

```powershell
cd /usr/share/kibana/bin/
./kibana-verfication-code
```

![Screenshot (792).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/19c5d10e-ba0c-48c4-b3ad-ac5a87adc0e3/Screenshot_(792).png)

![Screenshot (794).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/3ec44a5d-6003-42d6-8bd6-fff3640b719c/Screenshot_(794).png)

1. Input the verification code into elasticsearch setup. Input the username and password generated by elasticsearch when it was installed

![Screenshot (795).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/ed05256a-5e32-4f7a-8dc2-649d834c5171/Screenshot_(795).png)

![Screenshot (796).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/420bea6a-7a43-469e-bf11-bc1f618357e4/Screenshot_(796).png)

1. Generate encryption keys for Kibana otherwise rules will not be able to be deleted or modified after restart
2. In the following directory, **/usr/share/kibana/bin,** the two main binaries of importance are:
    - **kibana-encryption-keys**
    - **kibana-keystore**

```powershell
./kibana-encryption-keys generate
```

1. Once the encryption keys have been generated, save to notepad and now add to the keystore

```powershell
./kibana-keystore add
```

![Screenshot (797).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/74c28953-1228-4959-bcbb-322e7ebae83e/Screenshot_(797).png)

1. Restart Kibana and in elasticsearch under the security tab → Alerts, you should no longer receive a warning

```powershell
systemctl restart
```

[]()

# Windows Server Deployment

1. Deploy a Windows Server 2022 instance with at least 2 CPUs, 55 GB of storage, and 2 GB of RAM
2. Check if the Windows Server is able to be connected to via RDP by searching **Remote Desktop** and inputting the Public IP of the Server 

![Screenshot (798).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/aa76de5c-179a-4e25-88d4-006465bc49a9/Screenshot_(798).png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/691348af-e6b0-4d35-ae1b-4a6e3b73872d/image.png)

# Fleet Server

Fleet Server is **a component of the Elastic Stack used to centrally manage Elastic Agents**. It's launched as part of an Elastic Agent on a host intended to act as a server

Agents provide a **unified** way to add monitoring for logs, metrics, and other types of data

There are **two types** of agents

- Standalone
- Fleet managed

## Elastic Agent and Fleet Server Setup

1. Deploy an Ubuntu 22.04 server with at least 1 CPU, 4GB RAM, and 55GB of Storage
2. Make sure the ELK Server is running and in the interface of the hamburger menu select, **Fleet** under **Management** tab

![Screenshot (800).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/340d6cd0-66d6-4435-8147-3f78a8259c41/Screenshot_(800).png)

1. Select **Add Fleet Server** and choose **Quick Start** and input the name/IP of the fleet server created

![Screenshot (801).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/0227b165-29b6-4497-a85c-f1301c7ff722/Screenshot_(801).png)

1. In the firewall for DFIR-SOC, allow all ports to be accessed by the Fleet Server by inputting the custom IP
2. SSH into the ELK Server and allow port 9200 because that is the port for elasticsearch
    - This is because elasticsearch listens on port 9200 and the fleet server needs to be able to access to the ELK Server

```powershell
ufw allow 9200
```

1. SSH into the Fleet Server instance and run the command provided by elasticsearch

```powershell
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.0-linux-x86_64.tar.gz
tar xzvf elastic-agent-8.17.0-linux-x86_64.tar.gz
cd elastic-agent-8.17.0-linux-x86_64
sudo ./elastic-agent install \
  --fleet-server-es=https://104.207.142.125:9200 \
  --fleet-server-service-token=AAEAAWVsYXN0aWMvZmxlZXQtc2VydmVyL3Rva2VuLTE3MzU1MTQ2MjAzMTE6UW1xMnNrN1RTTnVtV01VNHNxUEY0QQ \
  --fleet-server-policy=fleet-server-policy \
  --fleet-server-es-ca-trusted-fingerprint=4351dc83fa6f37520c43687c24a43fef6c4170cdec972fa9f3f2640773d3a8f6 \
  --fleet-server-port=8220
```

1. If done correctly, on the elasticsearch site, it should prompt **Fleet Server Connected**

![Screenshot (802).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/a76f79c3-e21f-42b4-aa3c-8cb967d27fd4/Screenshot_(802).png)

1. Continue the enrollment and specify the hosts that should be monitored. Since we want to monitor the Windows Server, input a policy name and select the agent type that will be installed **Windows**

![Screenshot (803).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/f9fa8d77-ae96-466c-947c-2aa7f6d01a76/Screenshot_(803).png)

1. In the Fleet Server run the following command to allow the firewall to receive access from the port it runs on 8220  and 443

```powershell
ufw allow 8220
ufw allow 443
```

1. In elasticsearch under Manage → Fleet change the port connection from 443 to 8220 since Fleet runs on 8220

![Screenshot (805).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/301c92cb-09bb-44af-8fb1-6a43a3ab01c2/Screenshot_(805).png)

![Screenshot (806).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/1d719e4f-669a-471d-a267-b83778148c3e/Screenshot_(806).png)

1. Connect to the Windows Server created earlier and run PowerShell as Admin. Input the command provided by elasticsearch. Ensure to change the port from 443 to 8220
    - At the end of the command add **—insecure** because a self signed certificate will be presented

```powershell
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.0-windows-x86_64.zip -OutFile elastic-agent-8.17.0-windows-x86_64.zip 
Expand-Archive .\elastic-agent-8.17.0-windows-x86_64.zip -DestinationPath .
cd elastic-agent-8.17.0-windows-x86_64
.\elastic-agent.exe install --url=https://45.76.22.179:**8220** --enrollment-token=WXhySEZKUUJDbDgzWE9GTWVHWW86cy1uRkdRelpSeVNPejljMll1WnZ3UQ== **--insecure**
```

![Screenshot (804).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/c9cc0c19-cf8a-4fe4-9723-9c45801eca86/Screenshot_(804).png)

1. Back in elastic, under **Fleet → Agents,** the Windows Server should be available if the agent was successfully installed

![Screenshot (807).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/44eafe87-3615-4171-a87c-e0b0c141fcc5/Screenshot_(807).png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/b5f87923-cb88-410a-b35e-57deb7dc07ad/image.png)

# Sysmon

System Monitor (Sysmon) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to **monitor and log system activity to the Windows event log**. It provides detailed information about 

- **Process creations**
- **Network connections**
- **Changes to file creation time**

## Important Event IDs

1. Event ID 1 - Process Creation
2. Event ID 3 - Network Connections
3. Event IDs 6,7,8 - Driver/Image Load and Create Remote Thread
4. Event ID 10 - ProcessAccess
5. Event ID 22 - DNS query

# Sysmon Setup

1. RDP Into the Windows Server 
2. Search sysmon in google and select the link under MSFT Learn
3. Select Download Sysmon

![Screenshot (809).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/57ad41cb-0ba7-4dee-9c62-085ef8c83a9e/Screenshot_(809).png)

1. Once downloaded, extract all contents from the sysmon folder
2. Download the config file for sysmon which will be under the name of **sysmonconfig.xml**

![Screenshot (810).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/c53afe83-99e4-41b3-8104-7f8923129cb3/Screenshot_(810).png)

![Screenshot (811).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/cfe8963a-1345-4db3-9a5a-3314e401fdce/Screenshot_(811).png)

1. On the right hand side, select **Raw.** Right click the page and select **Save as.** Save the config file to **sysmon** directory downloaded earlier

![Screenshot (812).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/d70a6c96-b082-45da-84bb-34a6e76f571e/Screenshot_(812).png)

1. Open PowerShell as admin and cd into the the directory of where sysmon is located

```powershell
cd C:\Users\Administrator\Downloads\Sysmon
```

1. Install sysmon and the install can be verified by searching **Services** and checking Sysmon64 is on the machine

```powershell
.\Sysmon64.exe -i sysmonconfig.xml
```

![Screenshot (813).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/bb090dbe-6c4c-4c7c-b6f9-74c22d0579b7/Screenshot_(813).png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/8f557724-0280-41d3-97f7-8e29ebb03dd1/image.png)

# Elasticsearch Data Ingestion

**Objective -** Ingest Sysmon and Windows Defender logs into Elasticsearch

1. Log into Elasticsearch, and on the homepage select **Add Integrations**
2. Search **Custom Windows Event logs** and select **Add Custom Windows Event Logs**
3. RDP into the windows server and open **Event Viewer.** Open the Sysmon event logs by expanding the following: **Application and Services Logs → Microsoft → Windows → Sysmon →** right click **Operational** and select Properties
    - This is done in order to find the **channel name** for sysmon that way Elasticsearch can collect the logs from it
    - **Full name** should be selected for the channel name

![Screenshot (815).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/59b37324-f547-4b7f-a8df-ebe5be601f20/Screenshot_(815).png)

![Screenshot (816).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/d5329d48-c2ad-468d-8d27-570b106d6040/Screenshot_(816).png)

1. Add Full name of Sysmon to **Channel Name** back in Elasticsearch. Leave everything else as default

![Screenshot (817).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/72600a62-d845-48b0-9659-9ddf55d603b4/Screenshot_(817).png)

1. Under **Where to add this integration,** select **add to an existing host.** And under **Agent Policies,** select the policy that has the agent deployed on Windows Server. Once done, select Save and Continue

![Screenshot (819).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/67523824-5d65-4dcc-9f6a-f1e05d131a14/Screenshot_(819).png)

1. Sysmon has now been added to the Windows Policy

![Screenshot (820).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/cb85bf88-2844-4d94-ad14-55f4b0f82dc9/Screenshot_(820).png)

## Windows Defender Log Ingestion

1. Select **Add Custom Windows Event Logs** and name the integration 
2. Back in the Windows Server in Event Viewer, the channel name can be found under **Application and Services Logs → Microsoft → Windows → Windows Defender →** right click **Operational** and select Properties

![Screenshot (821).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/757a5dc0-816a-429a-a1fb-a70d1b510587/Screenshot_(821).png)

1. Informational Logs are not required, but for this project Event IDs **1116**, **1117**, **5001** will only be collected
2. Copy the full name and add it to the Channel Name back in Elasticsearch

![Screenshot (822).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/1506d7c9-2d0a-46a5-b63c-43b0a28479c2/Screenshot_(822).png)

1. Select **Advanced Options.** Under **Event ID,** include **1116, 1117, 5001**
2. Again, under **Where to add this integration?** Select the Policy for the Windows Agent

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/df9455eb-2d79-4d55-a6c0-d0b200a5e0a3/image.png)

1. Both Sysmon and Defender logs should now be pulled into the Elasticsearch instance

![Screenshot (824).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/b4f98054-99f0-41e6-b4bb-39edc0946e3c/Screenshot_(824).png)

**Note -** If CPU and Memory Usage are unavailable in the specific machine being monitored in the **Fleet** tab, there’s a chance that the firewall is blocking traffic, so try to allow traffic to the ELK Server on port 9200

![Screenshot (825).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/30e100db-6c90-46cb-92cb-eb3f36f2efeb/Screenshot_(825).png)

1. In the **Discover** tab verify that logs are coming in from Sysmon by searching **winlog.event_id: 1.** Expand any of the events and select **Page 2.** Check that the **event_provider** is **Microsoft-Windows-System**

![Screenshot (826).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/bee0bb56-3c9d-4c8f-96c5-453e1520d756/Screenshot_(826).png)

# Ubuntu Server 24.02 SSH Server Installation

1. Deploy an Ubuntu 24.04 server with at least 2 GB of RAM, 55 GB of Storage, and 1 CPU

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/f244cfb2-7002-44b2-bb32-442b1a82dd56/image.png)

1. SSH into the server and cd to the directory containing the **auth.log** 
    - This will contain authentication attempts

```powershell
cd /var/log
```

![Screenshot (828).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/ab496c8e-0ca4-4237-b2a2-2376bb6c7a88/Screenshot_(828).png)

1. Failed passwords can be filtered by running the following 
    - **cut** helps select data from a particular column specified by a delimiter
    - The following when checking for root will paste all the IPs of the those that tried to SSH into the SSH Server

```powershell
grep -i failed auth.log
grep -i failed auth.log | grep -i root | cut -d ' ' -f 9
```

## Elastic Agent Installation on SSH Server

1. In Elasticsearch, go to the **Fleet** tab under **Management** 
2. Select **Agent Policy** and create a new policy for the SSH Server

![Screenshot (829).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/d9367d36-574e-49c3-bcbb-90df37a52b7c/Screenshot_(829).png)

1. Go to the **Agents** tab and select, **Add Agent.** Select the policy that was just created and **enroll in Fleet**

![Screenshot (830).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/f3aeb35a-e29f-4505-bb09-e10e88cce8cd/Screenshot_(830).png)

1. For the OS, ensure to select **Linux Tar** since this is a linux-based server the agent will be installed on 

![Screenshot (831).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/62dd5345-81a4-4e45-8af1-0263130ad1d2/Screenshot_(831).png)

1. Copy and Paste the command to the home directory of the SSH Server
    - Be sure to add **- -insecure** to the end of the command because this is a self signed certification

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/fbc64a23-b21f-4309-ab97-1de9bd13f234/image.png)

![Screenshot (833).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/f76b45f7-9c06-4f7a-ae24-a737638865cc/Screenshot_(833).png)

1. If agent was successfully installed, back in the elastic agent enrollment section, it should confirm likewise

![Screenshot (834).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/9a0cd882-fb9c-4627-87fd-a696bee1b11f/Screenshot_(834).png)

1. In the **Discover** tab under **agent_name,** it can be confirmed that the logs and agent are being generated from the **SSH Server** 

![Screenshot (835).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/bde5abf0-4166-44f0-9492-f6c65f0fb656/Screenshot_(835).png)
