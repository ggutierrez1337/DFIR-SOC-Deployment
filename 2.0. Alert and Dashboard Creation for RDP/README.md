# 2.0. Alert and Dashboard Creation for RDP/SSH Activity

## SSH Brute Force Alert and Dashboard Creation

**Note** - When checking for brute force attacks, it is good to know the **failed attempts, user, source IP**

2.0 Under **agent_name** filter only for the SSH server since that is what the alert/dashboard will be based off of

2.1. Under **data_stream.dataset,** filter for **system.auth** to check for information pertaining to authentication

![Screenshot_(836)](https://github.com/user-attachments/assets/8f6f7cf3-2a3d-4e56-a963-e0b69c298656)

2.2. It can be seen under **log.file.path** that the logs are coming from the **/var/log/auth.log** path

![Screenshot_(837)](https://github.com/user-attachments/assets/d823a81f-3bf7-41d5-98ea-83e3834e67fe)

2.3. At the top remove the system.auth data set and continue searching

2.4. Select the **system.auth.ssh.event** and add it to the table. At first there are only dashes being displayed in the data section. Input the following in the search in order to see data pertaining to authentication: **system.auth.ssh.event:***

![Screenshot_(839)](https://github.com/user-attachments/assets/c65bb3ee-0d54-48db-aea7-0b58ae9a0e6c)

![Screenshot_(840)](https://github.com/user-attachments/assets/c23439a5-4b25-4671-9766-45e8fa14a882)

![image](https://github.com/user-attachments/assets/e7f1732e-4c01-4cfd-a872-2de5b6f8c456)

2.5. Add [**user.name](http://user.name)** as a column as well since it will display the usernames that were attempted for authentication

![Screenshot_(842)](https://github.com/user-attachments/assets/6790e8ac-3f50-4213-a0ed-405a76a7bba6)

![Screenshot_(843)](https://github.com/user-attachments/assets/36c390b2-b2c4-4e45-bf4d-e91412290c09)

2.6. Add the **source.ip** and to check where the authentication attempts are coming from

![Screenshot_(844)](https://github.com/user-attachments/assets/7577c70b-079d-49db-86b3-1570b836859d)

![Screenshot_(845)](https://github.com/user-attachments/assets/2a5dd586-c887-4a78-b643-38973c35b4e6)

2.7. To track the geolocation of the login attempts add in the **source.geo.country_name**

![Screenshot_(846)](https://github.com/user-attachments/assets/1234c04a-f4bd-4652-bb53-827f2ab0af65)

![Screenshot_(847)](https://github.com/user-attachments/assets/8bb83f60-f66e-45e1-adb3-a614192fd9d5)

2.8. Hovering over **Failed** field, select the **( + )** as this will filter for only **failed login attempts**

![Screenshot_(848)](https://github.com/user-attachments/assets/a36e80c3-d9d0-4969-a1f6-6e0bc917d56d)

2.9. It can now be seen that there are **14,851 Failed login attempts** to this SSH Server

![Screenshot_(849)](https://github.com/user-attachments/assets/4b2254f7-74db-4e00-bbee-ceec629eb028)

2.1.0. Save the filtered settings by selecting **Save** 

## Alert Creation

2.1.1. To create an alert, select the **Alert** tab in the top right corner and select **create search threshold rule**

2.1.2. The query is already input based on the saved filtered settings and can always be changed. By testing the query it can be seen that within the last **5 minutes,** there has been 18 login attempts to the SSH Server

![Screenshot_(854)](https://github.com/user-attachments/assets/a2338040-252c-4f0f-a662-fb6519874f7a)

2.1.3. Save the rule

## Dashboard and Map Creation

2.1.4. Under the **Analytics** tab select **Maps** and in order to view the failed login attempts from a geographical perspective, rebuild the search query

![Screenshot_(857)](https://github.com/user-attachments/assets/6b82f840-adcb-4bf9-a885-9db0ef5691d8)

2.1.5. Copy and Paste the search into the search bar of **Maps** and select **Add Layer.** There are various mappings to choose from, but in this case, select the **Choropleth** layer

![Screenshot_(858)](https://github.com/user-attachments/assets/05ab670d-11c9-4287-86d6-9eec4fc6577f)

2.1.6. Under **Boundaries Source,** select **Administrative boundaries from the Elastic Maps Service** and for **EMS Boundaries,** choose **World Countries** since the countries of login attempts are trying to be viewed 

![Screenshot_(859)](https://github.com/user-attachments/assets/f36342e9-c43b-49fe-b0ec-59ab414ae196)

2.1.7. Under **Data View,** make sure the data view is the same as the one in the **search query**

![Screenshot_(862)](https://github.com/user-attachments/assets/3126936e-133e-4e4e-aa0b-fe707a09e1d8)

![Screenshot_(863)](https://github.com/user-attachments/assets/383c4dd9-89ba-43b7-8dc9-9fc3d3be24ef)

2.1.8. For the **Join Field,** select **source.geo.country_iso_code** to highlight the countries then select **Add and continue**

![Screenshot_(861)](https://github.com/user-attachments/assets/355fbc73-51ae-48b9-bccd-71f1bf8c8fd7)

2.1.9. Once settings are to the liking, select **Save** 

![Screenshot_(864)](https://github.com/user-attachments/assets/4e72ddbd-5b95-4b44-a054-255dae5b7e11)

2.2.0. Save the Dashboard 

![Screenshot_(865)](https://github.com/user-attachments/assets/77a02823-062d-47bd-b4c3-f75ae84c1c62)

![image 1](https://github.com/user-attachments/assets/e292c787-6f2e-45e2-bf5e-544c8cd8519e)

## RDP Brute Force Alert and Rule Creation

2.2.1. In Elasticsearch under the Discover tab, filter the events only for the **Windows Server**

![Screenshot_(868)](https://github.com/user-attachments/assets/5fe315f1-d0f8-4602-bdf8-b52fe6064804)

![Screenshot_(869)](https://github.com/user-attachments/assets/46e1dc09-e426-46c8-8ff2-594258ef575f)

2.2.1. Since brute force is the main thing being sought after, checking for event IDs pertaining to authentication is a good start

- Event **4625** logs **failed authentication** attempts

2.2.2. Search **event.code:4625** in order to check for failed authentication to the Windows Server. It can be that there are 16,865 events under event code 4625

![Screenshot_(870)](https://github.com/user-attachments/assets/2748a31e-d12f-462c-9371-9163f0e7fc1f)

2.2.3. Add the **source.ip** to the table 

![Screenshot_(872)](https://github.com/user-attachments/assets/82609679-96a6-4e06-995c-3133dfc18767)

2.3.4. Add the usernames being used for authentication to the table

![Screenshot_(873)](https://github.com/user-attachments/assets/bd42293f-ebe3-4bb9-8e77-a110d30cfb93)

2.3.5. Once the query has been created, save the search as **RDP Failed Activity**

### Alert Creation

2.3.6. Create an alert and add the alert name. Leave the rest as default as it is based off of the query where it can be seen that there has been **29 events** in the last 5 minutes

![Screenshot_(874)](https://github.com/user-attachments/assets/ec775d6c-2e97-4ef4-8cd1-f897a059ace0)

2.3.7. Save the alert

### Rule Creation

Detection rules **define conditional logic that is applied to all ingested logs and cloud configurations**. 

This can help be creating a rule that can provide additional information such as the user affected and other info that might have been missed by the alert, since it is limited

2.3.8. Under the **Security → Rules** tab, select **Detection rules (SIEM).** Choose **Create new rule**
 

![Screenshot_(875)](https://github.com/user-attachments/assets/d7bc4253-1531-4ba4-9ceb-a6b6383f1dce)

![Screenshot_(876)](https://github.com/user-attachments/assets/be0016f6-ce86-48cb-a3c4-44972ec6bb63)

2.3.9. Select **Threshold,** and leave **Index Patterns** as default

![Screenshot_(877)](https://github.com/user-attachments/assets/27e9d500-e9d2-48a4-bcfb-0bfd165ef33c)

2.4.0. For **Custom query,** input the query made earlier when creating the SSH Bruteforce Dashboard

```powershell
system.auth.ssh.event: * and agent.name: DFIR-SSH-SERVER and system.auth.ssh.event: Failed and user.name:root
```

2.4.1. Group by the **source.ip** and [**user.name](http://user.name).** Be sure to add in **required fields** and input source.ip and user.name to those fields as well, then select **Continue**

![Screenshot_(878)](https://github.com/user-attachments/assets/05a1f494-93cf-4258-802b-8efd4fa8feaa)

2.4.2. Name the rule. Expand **Advanced Settings** and under **custom highlighted fields** select, **source.ip**

![Screenshot_(879)](https://github.com/user-attachments/assets/785f466b-5eb0-4751-98a4-40c793b53613)

2.4.3.. Keep the rule schedule times as default and click **Continue**

![image 2](https://github.com/user-attachments/assets/b7d94f07-8577-4a73-b06f-0ec671c4b665)

2.4.4. Under **Rule Actions,** do not select any connector type, then select **Create & Enable Rule**

2.4.5. Follow the same steps when creating a rule to detect RDP Bruteforce attempts, and now there should be two rules created 

![Screenshot_(881)](https://github.com/user-attachments/assets/0fdb08d3-6dae-46f6-a080-c43dd3d7c6b2)

## Dashboard for RDP Activity

2.4.6. In the **Discover** tab select the folder in the top right corner and choose the **RDP Failed Activity** query and change the time frame to **7 days** to ensure there is data

![Screenshot_(882)](https://github.com/user-attachments/assets/e972f62c-d5a3-47fe-b58f-27be73ffab6b)

![Screenshot_(883)](https://github.com/user-attachments/assets/40d68919-16ca-42e7-93a9-fc7b012ea5ee)

Take note of the query and be sure the query contains the name of the agent as well in order to create a dashboard from the queried information

```powershell
event.code:4625 and agent.name: DFIR-WINSERV
```

2.4.7. Go to **Maps,** and paste the query into the search as well as changing the time to the last **7 days.** Then select **Add Layer** and choose **Choropleth**

![Screenshot_(885)](https://github.com/user-attachments/assets/19427dba-e9ba-4f57-b3f9-c456fc2e4ba1)

![Screenshot_(886)](https://github.com/user-attachments/assets/06bac9b8-9536-4641-94e2-0e4bffa294da)

2.4.8. Under **Boundaries Source,** select **Administrative boundaries from the Elastic Maps Service** and for **EMS Boundaries,** choose **World Countries** since the countries of login attempts are trying to be viewed 

![Screenshot_(859)](https://github.com/user-attachments/assets/0ba67245-952f-4844-984b-1e155159e1e0)

2.4.9. Under **Data View,** make sure the data view is the same as the one in the **search query**

![Screenshot_(862)](https://github.com/user-attachments/assets/085af8eb-155e-40a6-bb5b-0d0a6cb9fc71)

![Screenshot_(863)](https://github.com/user-attachments/assets/ac34669d-a137-4d67-8a39-f6c5529fa042)

2.5.0. For the **Join Field,** select **source.geo.country_iso_code** to highlight the countries then select **Add and continue**

![Screenshot_(861)](https://github.com/user-attachments/assets/caabd6dc-914f-4fe4-86c8-1539bc92b5a9)

2.6.0. The geolocation of all sources that have failed to RDP into the Windows Server can now be seen on the map

![Screenshot_(887)](https://github.com/user-attachments/assets/87fe672b-1cb9-4dba-aa99-e92b5213b1a2)

![image 3](https://github.com/user-attachments/assets/420d4936-d00d-48dd-9a9c-d3d9849c5eb0)

2.6.1. Save the the map and be sure to select the **Existing Dashboard** so the map will show up under that dashboard

![Screenshot_(889)](https://github.com/user-attachments/assets/16af28e4-7a1b-4f33-a772-764f8f05a13d)

### Query Creation focusing on Logon Types 10 and 7

2.6.2. Go back to the RDP Failed Authentication query and check for the field **winlog.event_data_LogonType.** Add the field to the search and to check those specific logon types

- This will show successful remote connections made to the Windows Server

```powershell
event.code:4624 and (winlog.event_data.LogonType: 10 or winlog.event_data.LogonType:7)
```

![Screenshot_(891)](https://github.com/user-attachments/assets/a9ac4d0e-6c24-4489-8e6f-bd8641c9da74)

[]()

2.6.3. Save the query as **RDP Successful Activity** and then copy the query 

2.6.4. Back in **Dashboards,** duplicate the **RDP Failed Authentication Map.** Change the name to **RDP Successful Authentications,** edit the query and paste the new query to the map, and it will automatically update

![Screenshot_(892)](https://github.com/user-attachments/assets/c1a6da42-b5f1-45a0-bb1d-12cedfe1e220)

### List the Username and Count along with SRC IP, Table, and Map

2.6.5. Back in the **RDP Failed Activity** query, add the countries for the Sources trying to RDP into the Windows Server 

![Screenshot_(893)](https://github.com/user-attachments/assets/0c40caaf-df6f-4f93-bd2e-f9d9ad6afe84)

![Screenshot_(894)](https://github.com/user-attachments/assets/e925e672-dfd1-4f13-83c4-d1fe01bae387)

2.6.6. Back in **Dashboard,** select **Create Visualization**. Add the SSH Failed authorization attempt query in the search 

```powershell
system.auth.ssh.event: * and agent.name: DFIR-SSH-SERVER and system.auth.ssh.event: Failed
```

![Screenshot_(895)](https://github.com/user-attachments/assets/ab0803a7-cf75-4319-87fd-7c1f146cfc0b)

2.6.7. Drag and drop the following field names into the table: **source.ip, user.name,** and **source.geo.country.name**. Change the chart type from a **Bar** chart to a **Table**

![Screenshot_(896)](https://github.com/user-attachments/assets/734ceac5-7ce6-483d-b6c6-26f5acee2bd6)

2.6.8. Configure the rows by selecting them. Select **Top 3 values of [user.name](http://user.name).** In the settings, change the **Number of values** from 3 to **10.** In Advanced, deselect **Group remaining values as Other.**

![Screenshot_(897)](https://github.com/user-attachments/assets/46f7b7ea-8b03-4353-8b56-dc0da7ea99aa)

2.6.9. Repeat the same steps for **source.ip** and then sort the **Count of records** by **descending**

![Screenshot_(899)](https://github.com/user-attachments/assets/c3ad8ae5-8bd3-4805-8bff-d7a2f0a425d5)

2.7.0. Once done save the table as **SSH Failed Activity**

![Screenshot_(901)](https://github.com/user-attachments/assets/1dce9f4c-7f7c-48e0-84df-13d9e96a7b92)

2.7.1. Duplicate the table in order to show **SSH Successful Activity.** Select the title and change where it says **Failed** to **Accepted,** then select **Save and Return**

![Screenshot_(902)](https://github.com/user-attachments/assets/492f2e4f-b24c-4e19-bac9-3388aa15adfe)

2.7.1. Repeat the process this time for **RDP.** Duplicate the Failed Authentication table for SSH and rename it **RDP Failed Authentication,** and changed the query to the following

```powershell
event.code:4625 and agent.name: DFIR-WINSERV
```

2.7.2. Duplicate the **RDP Failed Authentication** table and change the name to **RDP Successful Authentication** and change the query to the following:

```powershell
event.code:4624 and (winlog.event_data.LogonType: 10 or winlog.event_data.LogonType:7)
```

![image 4](https://github.com/user-attachments/assets/34662ed6-69f1-4272-85ec-1fdb86ad1d9c)

2.7.3. The maps and tables have now been created for both RDP as well as SSH to monitor for failed and successful logins

![Screenshot_(902)](https://github.com/user-attachments/assets/d0ccc892-a466-46ed-82a0-ade47e4230cd)

![image 4](https://github.com/user-attachments/assets/746723e6-69b7-4130-a8d4-d716a76c9947)
