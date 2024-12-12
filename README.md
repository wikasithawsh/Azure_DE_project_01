### Azure_DE_project_01

## Step 01: Setting up the Data Source
1.1:SQL Server Instance 
1.2: SQL Server Management Studio (SSMS)
## used data set = //github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
1.3: Restore Database Using SQL Server Management Studio (SSMS)
![image](https://github.com/user-attachments/assets/3f3d93c5-08ad-4461-9d77-c3acee516d3c)

## Step 02: Creating Resources in Azure cloud 
##2.1: Create Azure Resource Group 
![image](https://github.com/user-attachments/assets/94aa3607-af82-4e4a-a0a9-a36c78df6ba3)
Note: This is the resource group I will use to build this complete end-to-end project. Inside this resource group, I will be creating all the tools required for this project.

## 2.2: Create Azure Synapse Analytics and Storage Account (DatalakeGen2)
![image](https://github.com/user-attachments/assets/af528e72-885d-4f17-96cf-11e1382c00a6)

![image](https://github.com/user-attachments/assets/fc9de621-8581-40dc-b141-7ed393a43d18)

![image](https://github.com/user-attachments/assets/716dde2d-f667-477d-a07e-c286d5eb3750)

![image](https://github.com/user-attachments/assets/4afb0699-e527-4d84-b661-538e0a849f23)

## 2.3: Create Azure Data Factory 
![image](https://github.com/user-attachments/assets/fe05f6ba-24d5-47f9-b0fc-b0a577f40e77)

![image](https://github.com/user-attachments/assets/6581bda8-3316-4e49-8575-75a9615a9055)

## 2.4:Create Azure Databricks 
![image](https://github.com/user-attachments/assets/98936044-d4a6-4aaa-988b-406ccce22f24)

## 2.5: Create Azure Key Vault

![image](https://github.com/user-attachments/assets/fa7ae15d-b755-46b7-b436-4e421ab780ed)

![image](https://github.com/user-attachments/assets/0f8ff0b8-9c05-4790-9609-18e99cbd572d)
-----------------------------------------------------------------------------------------
## Let's see all the resources I created now 
![image](https://github.com/user-attachments/assets/084b321f-d490-44de-b0fc-60b4ae714e89)

## SQL server , user login creation & grant permission 
# SQL server > file >open>file > createloging.sql
"CREATE LOGIN wiks WITH PASSWORD = 'WIKSdataengproject';
create user wiks for login wiks " 

![image](https://github.com/user-attachments/assets/ba730459-94ab-42a3-88a9-57f24fe71037)

![image](https://github.com/user-attachments/assets/5f296ac7-8e02-46b1-a7e9-1743fc596805)

## grant permission to SQL server created user 
Hint: right clik on user > select  user properties 
![image](https://github.com/user-attachments/assets/6a096e69-404f-4783-8fb9-444efbee1dea)

## key vault > secret objects 

![image](https://github.com/user-attachments/assets/4c16ba41-bd24-469c-aa39-e914048ef21b)

## We need to create two secret values 
1 . username, hint > use details from craetelogin.sql file 
2.  password


![image](https://github.com/user-attachments/assets/525f855e-8e69-4acb-b290-fc063d04f24d)


![image](https://github.com/user-attachments/assets/9ad65aee-79e5-46b1-ab04-83a12e8d4750)

## Note: Using key vault, we can connect ADF, and synapse A with on-prem SQL db without typing username & password

--------------------------------------------------------------------------------------------
## Now I have created all the necessary Azure resources, created user logins for on-prem SQL db, created key vault authentications 
--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
## Step 3 Data ingestion: Azure Data Factory 
3.1: Build /establish a connection between the on-prem SQL database to Azure DF 

we can use integration runtime [self-hosted integration runtime]
![image](https://github.com/user-attachments/assets/ad552a77-4cf1-4096-8066-a516c427f39b)

Note: Here we can see AutoResolveIntegrationRuntime is default created in Azure. It's used to connect ADf with other cloud components like Azure Data Lake
![image](https://github.com/user-attachments/assets/256e48d9-94d4-412d-8714-db8acd7fb7f3)

## 3.2 Creating self-hosted runtime in ADF 
![image](https://github.com/user-attachments/assets/1166424a-58d9-41b8-b051-836d1d1d0419)

## Now we can see two options & we have to choose Self-hosted option as below 
![image](https://github.com/user-attachments/assets/c6ca7e68-aadf-40d3-8b21-a1f4a3188f08)

## Need to provide a name for SHR (self-hosted runtime)
![image](https://github.com/user-attachments/assets/633a6938-382a-4229-8a1c-164e529b248a)


## Now we can see there are two methods that we can use to set up self-hosted runtime
3.2.1: Express set up = We can just click on this link to install the self-hosted integration runtime without actually specifying the key.

3.2.2: Manual set up  = In the manual setup, we can click on that link to download an application. When we install the application we have to enter one of these  keys (we can see in below image those keys)

Here I tried option 1 ( Express setup)
![image](https://github.com/user-attachments/assets/5a7ae05a-8124-4872-9fc5-3f2af27bbeca)

Then we need to download the Expresssetup application into our local machine where SQL on-prem data base is.
