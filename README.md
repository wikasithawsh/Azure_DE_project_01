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

