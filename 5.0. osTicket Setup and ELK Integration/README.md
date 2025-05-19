# 5.0. osTicket Setup and ELK Integration

## XAMPP Installation and Setup

5.0. Deploy a Windows Server 2022 with at least 55 GB of SSD, 2 GB of RAM, and 1 CPU
5.1. RDP into the osTicket Server. In the browser search **XAMPP** and download the Windows version. XAMPP will be automatically installed in the **C:\xampp** folder
5.3. Once finished installing, the first thing that should be done is editing the **properties** config file for XAMPP by right clicking the file and selecting **Edit** - **This PC → System Drive (C:) → xampp → properties**

![Screenshot_(985)](https://github.com/user-attachments/assets/323f7ea9-90cc-49f5-8e64-e6d486113269)

5.4. Change the **apache_domainname** to the **Public IP of the Windows Server,** then select **File** and **Save** the changes

![Screenshot_(986)](https://github.com/user-attachments/assets/b307f8e0-b9d8-4af6-bb35-a4ac2b2f6697)

5.5. The next change that will be made is to the **config.inc.php** config file which is located in the **phpMYAdmin** folder

![Screenshot_(987)](https://github.com/user-attachments/assets/c7ba58fb-b057-4203-8bb0-e51a5efde33d)

![Screenshot_(988)](https://github.com/user-attachments/assets/d26e6538-846e-4e01-9795-9ad50fef7dd7)

5.6. Copy/Paste the config file and name it whatever seems fit as this will be a backup file just in case something goes wrong
5.7 Right click **config.inc.php,** select **Open With,** and choose **Notepad**
5.8. Change the **$cfg['Servers'][$i]['host']** to the **Public IP** of the Windows Server. Once finished **save** the file and exit
5.9. Create an endpoint inbound firewall rule by selecting New Rule in **Windows Defender Firewall with Advanced Settings.** Choose ports **80,443** and **allow** all TCP connections to the ports 

![Screenshot_(989)](https://github.com/user-attachments/assets/7e1947ca-082a-463f-8ddd-bade706522dc)

![image](https://github.com/user-attachments/assets/441a47b7-f310-4337-996c-33c8ad38db8e)

![image 1](https://github.com/user-attachments/assets/0bde3ac0-16ef-4da7-9bc2-9d3324df09cd)

5.1.0. In the XAMPP control panel, **start** the **Apache** and **MySQL** services

![Screenshot_(993)](https://github.com/user-attachments/assets/c0ff7d7e-8e53-4d4e-9eb1-8ed604f3ccb5)

5.1.1. In the XAMPP control panel, select **Admin** in the **Apache** Section. Then select the **phpMyAdmin** tab, but there will be an error which is to be expected

![image 2](https://github.com/user-attachments/assets/29628431-77cd-45bc-90ad-fb1e3bca8bf9)

5.1.2. A correction to an error needs to be made, so back in the **config.inc.php** file, change the **$cfg['Servers'][$i]['host']** back to **127.0.0.1** for now then select **Admin** for **Apache** in the XAMPP GUI

[]()

5.1.3 Select **phpMYAdmin.** Go to the **User Accounts** tab and choose the **root** account with the **Host Name** of **localhost**

![Screenshot_(997)](https://github.com/user-attachments/assets/8ae21049-7a9f-4c49-a48d-37e32e7cc55e)

5.1.4. Select **login information.** Under **Host name,** change the drop down to **Use text field** and input the **Public IP** of the Windows Server. Also, change the password to **Use text field** and any desired password. Once completed, select **Go** and changes will be saved

![Screenshot_(999)](https://github.com/user-attachments/assets/fdc95527-a4b8-4171-a422-5ca6a4bf20e9)

5.1.5. Going back to the **config.inc.php** the **$cfg['Servers'][$i]['host']** can now be changed to the **Public IP** of the Windows Server and change **$cfg['Servers'][$i]['password']** to the password created in the control panel

![Screenshot_(1089)](https://github.com/user-attachments/assets/2847d72c-d61a-4c97-a56f-8ea9057bb25b)

5.1.6. Once logged in as admin, there will be an error message specifying, **Access denied for user ‘pma’.** This can be fixed by going to **User accounts** and selecting the **pma** account. Change the **Host name** to **Use text field** and the **IP** of the **Server,** and input any desired **password** for the account, then select **Go**

![Screenshot_(1090)](https://github.com/user-attachments/assets/82bc1198-f3e4-435d-9147-47061c4c62a0)

![Screenshot_(1091)](https://github.com/user-attachments/assets/4e3efb69-a99a-4e81-a254-28d836fd5410)

![Screenshot_(1092)](https://github.com/user-attachments/assets/6c804e89-2adc-4cc7-a08a-f939d8546fe8)

5.1.7. Back in the config file, modify the password for the user **pma** which can be found under the **User for advanced features** section then save the file

![image 3](https://github.com/user-attachments/assets/4f4b6ec2-5300-488d-8ae8-283567f6460d)

5.1.8. Close out the phpmyadmin page and open it back up from the XAMPP console Apache → Admin. Now when logging in, there should be no errors 

![Screenshot_(1094)](https://github.com/user-attachments/assets/69624405-44c7-4abe-8784-0b19573883e5)

## osTicket Installation and Configuration

5.1.9. Go to [**osticket.com](http://osticket.com)** and select **Self-Hosted,** then select **Download** for the **Open Source** option and download the latest version of osTicket

![Screenshot_(1095)](https://github.com/user-attachments/assets/25b3ac0f-525c-4f1c-ad4a-ff761778bcf3)

![Screenshot_(1096)](https://github.com/user-attachments/assets/1dd693f9-9a31-49b0-a338-012e6a58491f)

![Screenshot_(1097)](https://github.com/user-attachments/assets/984cc319-b00b-4527-acf5-58dd03a82baf)

5.2.0. Once the download is completed, extract all contents from the folder. Once extracted, there will be **2 Folders, scripts** and **upload.**

![Screenshot_(1098)](https://github.com/user-attachments/assets/cae3f7e5-091c-43a9-8ee6-f4b1b5a949f5)

[]()

5.2.1. Go the **XAMPP** folder and where it says **htdocs,** this is where default web contents are stored for the apache server. In this folder create a new folder and name it **osticket**

![Screenshot_(1099)](https://github.com/user-attachments/assets/60d65723-eb17-497d-907b-9ad65bd25b4c)

![Screenshot_(1100)](https://github.com/user-attachments/assets/65bfe685-8d0d-4e29-a016-a60ffd3aaef1)

5.2.2. Go back to the osTicket directory and copy both **scripts** and **upload** folders, then paste them in the new osticket directory in XAMPP (xampp → htdocs → osticket)

![Screenshot_(1101)](https://github.com/user-attachments/assets/6a47be2c-f38c-4d32-92ca-440d9e2ebd76)

![Screenshot_(1102)](https://github.com/user-attachments/assets/32d5c387-4308-437f-b46b-7928f904beb0)

5.2.3. Go to the browser and in the URL, input the IP of the machine followed by **/osticket/upload.** If everything is correct, the setup page for osTicket will be brought up, then select continue
    - e.g. 45.76.18.130/osticket/upload

![Screenshot_(1103)](https://github.com/user-attachments/assets/f7062478-6c9c-4b3f-89b4-408b1556fee3)

5.2.4. Next it will say the **Configuration file is missing** and we are given options on how to resolve this. Go to the path, xampp → htdocs → osticket → upload → include

![Screenshot_(1104)](https://github.com/user-attachments/assets/2c2b9c98-0cdb-43bd-a9ab-19b46dd50836)

![Screenshot_(1108)](https://github.com/user-attachments/assets/5af83115-dda7-486f-8403-56602b9dd712)

5.2.5. Scroll down until you see the **ost-sampleconfig.php.** Rename it to **ost-config.php** as recommended in the setup page

![Screenshot_(1109)](https://github.com/user-attachments/assets/e40d9c57-8bcc-481a-87b7-a1f4705ad8c6)

![Screenshot_(1110)](https://github.com/user-attachments/assets/d18677b1-ac28-4e7e-a299-02d927318757)

5.2.6. In the XAMPP control panel, select Apache → Admin. On the left hand side where DB information is, select **New.** Name the DB DFIR-DB, then select **Create.** It can then be seen that the **DFIR-DB** has been created

![Screenshot_(1112)](https://github.com/user-attachments/assets/79e99b67-18e5-420d-9881-be618f5ff661)

![Screenshot_(1113)](https://github.com/user-attachments/assets/8e18b44f-9275-4b41-bbf4-1ead8c86ddb2)

![Screenshot_(1114)](https://github.com/user-attachments/assets/b428b90a-cabe-4ed8-a1a0-56a6e9aa91ea)

5.2.7. Go back to the home page and select **User Accounts,** select the **root** account with the server IP. Go to the **Database** tab and select the DFIR-DB. For **Database specific privileges,** check all

![Screenshot_(1115)](https://github.com/user-attachments/assets/f3cc0a13-e4a6-4e2f-aea2-915b6d47b43b)

![Screenshot_(1116)](https://github.com/user-attachments/assets/d97e133d-b4cc-43e3-be4e-aaf78345c67c)

![image 4](https://github.com/user-attachments/assets/6910404e-1c40-40a2-824f-f69696290466)

5.2.8. Back in the setup page, select **Continue**. Input desired username, names, and passwords. Under the **Database Settings.** For the **MYSQL Hostname,** input the IP of the server hosting osTicket. Name the DB whatever was named in phpMyAdmin (DFIR-DB) along with username/password. Once finished click **Install Now**

![Screenshot_(1111)](https://github.com/user-attachments/assets/11b8c871-ad20-4b4e-a126-7c53fc37d9d5)

[]()

5.2.9. If osTicket is setup correctly, the following prompted will be shown. Next it is informing to remove write commands from the **os-config.php** file, which can be done with powershell

![Screenshot_(1118)](https://github.com/user-attachments/assets/7bd87330-1f52-4c25-9828-27b64af63332)

5.3.0. Open PowerShell as Administrator. Back in File Explorer copy the path where the **os-config.php** file is stored (xampp → htdocs → upload → include)

![Screenshot_(1119)](https://github.com/user-attachments/assets/c8b6b3e9-7b8e-4072-b28e-e9611db2efd3)

5.3.1. Back in PowerShell, run the following command

```powershell
cd C:\xampp\htdocs\osticket\upload\include
```

5.3.2. Back in the osTicket installer, copy the command provided for PowerShell and run the command in PowerShell where the **os-config.php** file is located. If the command does not work, run the following

```powershell
icals .\ost-config.php \reset
```

![Screenshot_(1121)](https://github.com/user-attachments/assets/023db675-0b84-479f-90dc-0195e79a23f5)

5.3.3. If working, the file will be **Successfully Processed**

![Screenshot_(1122)](https://github.com/user-attachments/assets/8575a0d5-7fed-49d0-9936-a0bc36d87797)

5.3.4 Copy and Paste both the **Your osTicket URL** and the **Your Staff Control Panel** URLs to notepad, or somewhere they can be found for later use

![Screenshot_(1123)](https://github.com/user-attachments/assets/9ae74fe9-0e0e-404f-a62a-ad09de671648)

5.3.5 When visiting both URLs, they should be accessible and the server is now running osTicket correctly

![Screenshot_(1125)](https://github.com/user-attachments/assets/63d241d9-5867-4927-98fb-a2ac1eafa85d)

![Screenshot_(1126)](https://github.com/user-attachments/assets/0a9626c1-361b-4d39-a2a6-b9f1a67612ce)

## Integrating the ELK Stack with osTicket

### Sending POST Requests From Elasticsearch to osTicket API

5.3.6. Go to the **Staff Control Panel URL** and sign in using the credentials to setup osTicket. Go to the **Admin Panel,** then select **Manage** and **API**

![Screenshot_(1128)](https://github.com/user-attachments/assets/84ad0490-55fd-4cfb-ad41-1552d451044e)

5.3.7 Select **Add new API Key**. Because the osTicket and the ELK server are in the same private cloud, the **Private IP** of the ELK server can be input. Public IP can be used as well. Input the **IP** of the ELK server and check the **Can Create Tickets** box, then click **Add Key** and an API key will be generated
    - The API key will be used on Elastic to begin integrating osTicket

![Screenshot_(1129)](https://github.com/user-attachments/assets/b77a6876-40f7-4fb5-a14f-04e54f0c20c0)

![Screenshot_(1130)](https://github.com/user-attachments/assets/8bbe61c9-ff41-40ab-bd67-4cbe6110c526)

5.3.8. Copy the API Key and paste it somewhere it can be accessed later. Login to your Elastic server and in the tab menu under **Management,** select **Stack Management**. Under **Alerts and Insights,** click **Connectors** and **Create Connector**
    - With these connectors, they can be used for actions such as sending emails based on alerts, creating a Jira Ticket, sending a message in teams, triggering a response with CrowdStrike security solutions

![Screenshot_(1131)](https://github.com/user-attachments/assets/bfa2c28b-a85a-47f3-ba22-3fcc06bd8f7e)

![Screenshot_(1133)](https://github.com/user-attachments/assets/542a7231-b514-4405-9852-9d47c59f2c66)

**Note** - By default APIs cannot be used with the free subscription, so select **Manage License** and **Start a 30-day trial.** Elastic Defend is an EDR that is only available with the licenses, and cannot isolate hosts with the free subscription. Using a tool like Lima Charlie EDR can be a workaround

![Screenshot_(1134)](https://github.com/user-attachments/assets/15454132-61da-40f0-9057-a72a6bdd60e9)

5.3.9. In the connectors, select **Webhook**. Name the connector **osTicket** and the **Method** will be a **POST** request since an alert is being sent and ticket generated. Input the URL of osTicket 
    - e.g. http://45.76.18.130/osticket/upload/api/tickets.xml

![Screenshot_(1135)](https://github.com/user-attachments/assets/58f88f9b-40a9-4389-813d-83ecc72615fa)

5.4.0.. For **Authentication** select **None** and select **Add HTTP header**. For the **Key** input **X-API-Key,** and under **Value,** input the API Key of osTicket. Once done, click **Save & Test**

![Screenshot_(1136)](https://github.com/user-attachments/assets/b5dfd93c-6764-4618-807c-83d5673e8dbe)

5.4.1. Copy and Paste the following XML format to test and see if osTickets API is working with Elastic. Select **Run** once the code has been input. If run with no issues, it will say **Test was successful**
    - This code can be found in the osTicket github under the folder **api → tickets.md**

```jsx
<?xml version="1.0" encoding="UTF-8"?>
<ticket alert="true" autorespond="true" source="API">
    <name>Angry User</name>
    <email>api@osticket.com</email>
    <subject>Testing API</subject>
    <phone>318-555-8634X123</phone>
    <message type="text/plain"><![CDATA[Message content here]]></message>
    <attachments>
        <file name="file.txt" type="text/plain"><![CDATA[
            File content is here and is automatically trimmed
        ]]></file>
        <file name="image.gif" type="image/gif" encoding="base64">
            R0lGODdhMAAwAPAAAAAAAP///ywAAAAAMAAwAAAC8IyPqcvt3wCcDkiLc7C0qwy
            GHhSWpjQu5yqmCYsapyuvUUlvONmOZtfzgFzByTB10QgxOR0TqBQejhRNzOfkVJ
            +5YiUqrXF5Y5lKh/DeuNcP5yLWGsEbtLiOSpa/TPg7JpJHxyendzWTBfX0cxOnK
            PjgBzi4diinWGdkF8kjdfnycQZXZeYGejmJlZeGl9i2icVqaNVailT6F5iJ90m6
            mvuTS4OK05M0vDk0Q4XUtwvKOzrcd3iq9uisF81M1OIcR7lEewwcLp7tuNNkM3u
            Nna3F2JQFo97Vriy/Xl4/f1cf5VWzXyym7PHhhx4dbgYKAAA7
        </file>
    </attachments>
    <ip>123.211.233.122</ip>
</ticket>
```

![Screenshot_(1138)](https://github.com/user-attachments/assets/9e917a0e-e0af-4621-ae83-eb852cf53104)

5.4.2. If osTicket’s API is successful and Elastic’s connector is working as intended. Go to **Staff Control Panel URL** for osTicket**,** login, and under **Tickets → Subject,** there should be a line stating **Testing API;** meaning Elastic and osTicket have been successfully integrated

![Screenshot_(1139)](https://github.com/user-attachments/assets/097693d9-35c9-42e2-b5a5-e6b2b6c3b4da)

## Alert Forwarding to osTicket

5.4.3. Modify the rules in Elastic to push alerts into osTicket. In Elastic under the **Security** section in Menu, select **Rules,** then choose **Detection rules (SIEM)**. 

![Screenshot_(1150)](https://github.com/user-attachments/assets/33c115af-9e82-419b-b2b4-c746a69af6e7)

![Screenshot_(1151)](https://github.com/user-attachments/assets/edac1995-507a-46e2-a7ab-441ab6b7f5fc)

5.4.4. Select the **SSH Brute Force Attempt** rule, choose **Edit rule settings**, and under **Actions** tab select the **Webhook**

![Screenshot_(1152)](https://github.com/user-attachments/assets/0bb20018-9f68-482a-ad67-92ab01f4d4b1)

![Screenshot_(1153)](https://github.com/user-attachments/assets/2bdef124-bfb9-41a2-921b-d0bc77a4a664)

![Screenshot_(1154)](https://github.com/user-attachments/assets/5bd8f32f-2112-4faa-9d3d-aa08c2c0f8b4)

5.4.5. osTicket will automatically be selected, and under **Action Frequency**, select **For each alert**. For the body, provide the same XML message used when testing the API
    - For the **Subject,** instead of **Testing API,** select **add variable** in the right hand corner of the **Body** section. Once finished, select **Save Changes**
    

```jsx
<?xml version="1.0" encoding="UTF-8"?>
<ticket alert="true" autorespond="true" source="API">
<name>Angry User</name>
<email>[api@osticket.com](mailto:api@osticket.com)</email>
<subject>{{[rule.name](http://rule.name/)}}</subject>
<phone>318-555-8634X123</phone>
<message type="text/plain"><![CDATA[Message content here]]></message>
</ticket>
```

![Screenshot_(1155)](https://github.com/user-attachments/assets/b03572c6-a8d7-4378-981a-cce113bc38bf)

![Screenshot_(1157)](https://github.com/user-attachments/assets/396356e2-4c57-4b00-9419-49fb2b9f7c3b)

**Note -** Response actions can be taking when an alert is triggered and can be useful when mitigating threats

5.4.6. If everything is working as intended, whenever a new alert is generated, a ticket will now automatically be sent to osTicket as well now

![Screenshot_(1158)](https://github.com/user-attachments/assets/62184f81-3c35-4cdc-92a0-2919b51f0d86)

![Screenshot_(1159)](https://github.com/user-attachments/assets/d1b55aec-612c-450c-9ed1-1be14a92a194)
