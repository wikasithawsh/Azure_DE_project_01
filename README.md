### Azure_DE_project_01
--------------------------------------------------------------------------------------------------------------------

![image](https://github.com/user-attachments/assets/522d33f7-9144-448c-abdf-a7e9f669ec5b)

--------------------------------------------------------------------------------------------------------------------
## Step 01: Setting up the Data Source
## Data Source is On-prem SQL database
1.1:SQL Server Instance 
1.2: SQL Server Management Studio (SSMS)
## used data set = //github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
1.3: Restore Database Using SQL Server Management Studio (SSMS)
![image](https://github.com/user-attachments/assets/3f3d93c5-08ad-4461-9d77-c3acee516d3c)

## Step 02: Creating Resources in Azure Cloud 
##2.1: Create Azure Resource Group 
![image](https://github.com/user-attachments/assets/94aa3607-af82-4e4a-a0a9-a36c78df6ba3)
Note: This is the resource group I will use to build this complete end-to-end project. Inside this resource group, I will be creating all the tools required for this project.

## 2.2: Create Azure Synapse Analytics and Storage Account (DatalakeGen2)
![image](https://github.com/user-attachments/assets/af528e72-885d-4f17-96cf-11e1382c00a6)

![image](https://github.com/user-attachments/assets/fc9de621-8581-40dc-b141-7ed393a43d18)

![image](https://github.com/user-attachments/assets/716dde2d-f667-477d-a07e-c286d5eb3750)  



## Error link service test connection is failed > 

reason is below > 
![image](https://github.com/user-attachments/assets/d494cd55-f982-4a10-b34d-28bb9bfef9d0)

Solution > 
![image](https://github.com/user-attachments/assets/c97201d8-adee-40c5-a970-41e2cd8e41ff)




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

## SQL server, user login creation & grant permission 
# SQL server > file >open>file > createloging.sql
"CREATE LOGIN wiks WITH PASSWORD = 'WIKSdataengproject';
create user wiks for login wiks " 

![image](https://github.com/user-attachments/assets/ba730459-94ab-42a3-88a9-57f24fe71037)

![image](https://github.com/user-attachments/assets/5f296ac7-8e02-46b1-a7e9-1743fc596805)

## Grant permission to SQL server-created user 
Hint: right click on user > select  user properties 
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

Then we need to download the Expresssetup application into our local machine where the SQL on-prem database is.

The below image shows > that the express setip application is installed now.
![image](https://github.com/user-attachments/assets/01a64c45-161c-4bf1-a064-08dea1b177b5)

## Note:  
1:  The benefit of using Express Setup is > It tries to download the file and do the key-based authentication automatically 
    for us.
2:  When manual setup is good > If we are going to install the self-hosted integration runtime in a different machine. Then we 
    should probably go with the manual setup where we will be going to manually register the self-hosted integration runtime 
    with the key copied from the issue data factory.
    -------------------------------------------------------------------------------------------------------------
3: Main usage of Integrated Runtime = We should have two things to perform any operation 
   1: Compute power \
   2: Infrastructure to execute the operation \
   So here, this integration runtime is giving us both in terms of the Azure Data factory to do any sort of operations like 
   data integration or data movement or any kind of thing.\
   ## So in simple terms, we can say the integration runtime is also called a computing infrastructure in ADF
   
![image](https://github.com/user-attachments/assets/15a9ea73-2ddb-4e31-84fb-9d0dd762ec96)

Once express set up is done we can see below in ADF.
![image](https://github.com/user-attachments/assets/88985c54-aee0-4884-a244-6d12fab97465)

## Express setup post install verification 
from local laptop we can check it via MS integration configuration manager 
![image](https://github.com/user-attachments/assets/37ee19e4-6ad3-4df2-ba98-fa52120ec4bd)

## 3.4 : Data Ingestion 
in ADF : Go Author > Pipielines > 


![image](https://github.com/user-attachments/assets/11045908-1643-4d27-901a-c1efcae2cb24)

Test Connection > 
![image](https://github.com/user-attachments/assets/48a6cdbc-a3ea-4612-9750-b65ca316a2f4)

Now we can see secret name failing, gives error, this is because we have config both username  passwords in key vault
![image](https://github.com/user-attachments/assets/b4644389-0a83-4603-af80-6ab14a4355ed)

## We need to grant permission to read secret object | need to create new access policy > 
![image](https://github.com/user-attachments/assets/b72aca44-2db8-4987-b87d-be74ec448a6b)

## Created Access Policy like below 
![image](https://github.com/user-attachments/assets/a93efff7-7f6e-47b9-b575-1382b63cc119)

in access policy > need to give ADF name under princilple 
![image](https://github.com/user-attachments/assets/330f7b8d-0609-4245-8739-c9f3023e9f3f)

Now Access policy been created 
![image](https://github.com/user-attachments/assets/fc5c1664-1717-41e2-ae10-dbcb332e717f) 

So now ADF can read secrets >>>>
Now that previous error is resolved > 
![image](https://github.com/user-attachments/assets/1733f252-0ee8-4be6-9f51-21972edcd063)

![image](https://github.com/user-attachments/assets/decab294-bc48-44ae-816a-424dc9a40a04)

![image](https://github.com/user-attachments/assets/bd4541d2-f7a6-43e0-8467-28497c868bb7)

## Error > 
![image](https://github.com/user-attachments/assets/54666689-0faf-4374-8057-721feb365405)

Solution tried| worked solution , restrted SQL server] > 
1. Check SQL Server Authentication Settings
Ensure that SQL Server is configured to allow SQL Server Authentication, especially if you're using a SQL login instead of Windows Authentication.
Open SQL Server Management Studio (SSMS) and log in to your instance.
Right-click on the server name (SQLEXPRESS) in Object Explorer > Properties.
Go to Security and ensure that SQL Server and Windows Authentication mode is selected.
![image](https://github.com/user-attachments/assets/5d658873-163f-443d-aa02-84e597967622)

Error /issue is resolved >>>>
![image](https://github.com/user-attachments/assets/4fb6afaa-1ec1-4306-aff9-25936912123d)

Now link connection is successfully connected (on-prem sql server to ADF)

---------------------------------------------------------------------------------------------
![image](https://github.com/user-attachments/assets/3dee0d87-22bb-4d8f-841a-dd4e9b81a8d6)

## Here first I copy address table data fro SQL server to ADF
![image](https://github.com/user-attachments/assets/e83a3955-b343-4296-a550-5e385a3efd34)

>![image](https://github.com/user-attachments/assets/0395e0c2-916a-4d73-9715-30b3e186e889)
## Source is configured now , we can preview how source data is arranged like below
## Note : for Source we selected sql server
![image](https://github.com/user-attachments/assets/2a003970-1c88-4c2c-8bff-23cdcf966860)

## For sink > We can select Azure datalake GEN 2 > parquet file format 
![image](https://github.com/user-attachments/assets/7e8d1cda-7d86-4146-852e-0356d964654f) 

## Hint: 
So here now we try to use/connect Azure Datalake Gen2 , so we have to create a link for it , this time ADF to Azure DL connection , so we can use default AuroinResolvetregationRuntime here.( to connect between azure cloud components)

Check test connection for sink 
![image](https://github.com/user-attachments/assets/e8fefb6b-e675-4b1c-9983-c9d40f41a913)

Test connection is ok > Then create link for sink 
![image](https://github.com/user-attachments/assets/26f6d0fb-38ec-4f92-bc2b-0e1f8450fe6b)

## Now we need to define in which location that we should save files in DL 
we can save data in DL in bronze container 

what is bronze container > 
![image](https://github.com/user-attachments/assets/a4c6cad2-3417-4d38-97da-93b4480c53d2) 

![image](https://github.com/user-attachments/assets/76edc66d-0267-4708-8b7e-13330a0ec812)

![image](https://github.com/user-attachments/assets/a517d20a-3435-4bb9-9254-f728a7795939)

-------------------------------------------------------------------------------------------
## ADF pipeline testing
## Error occured | pipeline is failed 
![image](https://github.com/user-attachments/assets/b1ec9a9f-2306-41a6-ad31-e967ed06bd61)

## Root cause :  (JreNotFound) indicates that the Java Runtime Environment (JRE) is not installed on your Self-hosted Integration Runtime (SHR) machine. The Self-hosted Integration Runtime is required for data transfer activities like copying data, and Java is needed for parsing and writing to certain file formats, such as Parquet or ORC

## solution >>> 
Add java path correctly in local pc 
restart self-hosted runtime 
![image](https://github.com/user-attachments/assets/6c72710f-6126-4e87-bc12-32b1be37acc5)

![image](https://github.com/user-attachments/assets/cea08541-d22a-4c83-be09-ecc9d2955fe6)

![image](https://github.com/user-attachments/assets/c4193689-9b76-42c4-b2ee-b0be94a7d0a8)

From Azure Doc > 
![image](https://github.com/user-attachments/assets/e68794c5-4d91-4d29-b704-567f11cbeb72)

### Special Note: To copy data from on-prem sql db to ADF, we must have oracle jre-8u431-windows-x64.
It was not worked JDK 23 , eben in Azure doc mentioned it's worked but here not worked.
https://www.java.com/en/download/manual.jsp

## Issue is resolved  | worked last solution was install oracal JDK & restarted self-hosted runtime 
![image](https://github.com/user-attachments/assets/9a89368e-7f06-462c-a7fb-ff8a61a670d1)


## Verification : we can see data has been copied into Datalake > bronze container 
![image](https://github.com/user-attachments/assets/d03ec710-2579-4869-aa51-af0acf4f8bec)

## Publish all the chnages (Deploying all the chnages into ADF)
![image](https://github.com/user-attachments/assets/e24fff27-767f-4425-aa2f-cf126643dafb)

Below is the output we can see in data lake > one table is data is ingested 
![image](https://github.com/user-attachments/assets/1af73707-dab2-4dfd-a98c-32d0f4df25ed)

-------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------
## Copy all the on-prem SQL Datbles at once into ADF 

Created sql script as below to get schema > getschema.sql 

SELECT
s.name AS SchemaName,
t.name AS TableName
FROM
sys.tables T 
INNER JOIN sys.schemas s 
ON t.schema_id = s.schema_id
WHERE s.name = 'SalesLT'
![image](https://github.com/user-attachments/assets/e06e4402-b2eb-4d9d-ad26-94b8284db3a2)

Next we can execute that script from SQL management studio like below 
![image](https://github.com/user-attachments/assets/3bda14fe-5c31-4dd2-8194-2cf2ab144458)

As per above image , these arethe tables now we are going to ingest into ADF.
## first we add lookup activity in ADF
here we don't give one table name ike previously , but we can use previously created Self-Hosted Runtime and link as well 
![image](https://github.com/user-attachments/assets/394157c1-892e-483e-b368-b822206aa604)

## Here in lookup activity , we can add getschema.sql query in ADF here also as below 
![image](https://github.com/user-attachments/assets/b2f1261b-0532-4c06-ad49-7b6eff45c202)

## Next task is how we can use lookup activity to load all SQl data into ADF , so we first debug lookup activity 
![image](https://github.com/user-attachments/assets/6fa90380-2c0f-4930-a8d1-08a8f7e85cb9)

Added config in lookup activity in ADF 
![image](https://github.com/user-attachments/assets/d6f56a83-c0a8-4d87-a906-c50330502e05)




## preview data in lookup activity 
![image](https://github.com/user-attachments/assets/c784a284-fbf8-4f31-abba-ca4946aae57e)


## in lookup table activity , inside we can add below configs 
![image](https://github.com/user-attachments/assets/f97a7b20-c778-43ee-b4b8-ee8c0bee8b80)

![image](https://github.com/user-attachments/assets/1dc7a888-ec3c-4b27-92f6-805d7a146380)

## Now inside lookup activity , we can config sink,since now source alreday is configured as above image.

## Note > folder structure in ADF herei followed is like this = bronze/SalesLT/Address/Address.parquet
so we can define folder structure as we wish like below 
![image](https://github.com/user-attachments/assets/2499c2ef-b181-4172-a009-c68fce1039da)

![image](https://github.com/user-attachments/assets/aba97334-0c77-4fe8-b4fd-5c3948bf2dec).

![image](https://github.com/user-attachments/assets/f793a8b2-c867-435f-b0c8-83a3647fe9ed)

![image](https://github.com/user-attachments/assets/d125b586-3600-4a87-8727-5f912878d017)

## once these chnages are done we cna publish piple line and then we can see whether all data are copied or not 
![image](https://github.com/user-attachments/assets/6059e2c9-746b-4e04-9d8a-5c9a915d13cc)

## All sql data tables ingestion pipeline is successfully completed 

![image](https://github.com/user-attachments/assets/f8d092f5-9f9d-4a09-92bf-8aca3e39425d)

Data has been ingested in corect folder structure in Azure Data Lake
![image](https://github.com/user-attachments/assets/7998b1ec-ca1f-461e-9642-b2bf81eb1e21)

---------------------------------------------------------------------------------------------------------
## 5 : Data Transformation | Datbricks
5.1 : Created compute cluster 
![image](https://github.com/user-attachments/assets/43f2ea1a-61ef-41fb-aeeb-13f567d2ae25)

5.2 : create Databricks notebook for data lake stoarge mount 

5.3: used code from azure docs > https://learn.microsoft.com/en-us/azure/databricks/archive/credential-passthrough/adls-passthrough (Azure Data Lake Storage Gen2)

/////////////////////////////////////////////////////////////////////////////////////////////////////////////
## Error >> error occurs because the Databricks cluster is unable to retrieve the ADLS Gen2 token required to authenticate and mount the Azure Data Lake Storage.
![image](https://github.com/user-attachments/assets/89f05973-63fb-46be-ad3a-0fd62b0d19cc)

## Solution >
![image](https://github.com/user-attachments/assets/4010d1fa-d181-4ebf-a362-1daa7d485856)

Issue is resolved by following above compute creation change in databrcks 
![image](https://github.com/user-attachments/assets/389eaf91-c32b-4543-bfbf-9af9f986d3a9)

Another error occured when creating gold container > manually created gold container in storage acc then databricks worked 
//////////////////////////////////////////////////////////////////////////////////////////////////////////////

5.4: Data transformations from bronze to silver container

T1 : data column modification (date time into date )
Before Transform :
![image](https://github.com/user-attachments/assets/72d51f59-2589-429e-a84a-b19839b6b7d0)

After transformation :
![image](https://github.com/user-attachments/assets/953c5693-7ee1-40bf-8b80-e77bb1d8a271)

Trnsformed data is saved in silver container 
![image](https://github.com/user-attachments/assets/d366ab40-fb8c-478a-94c2-65d3082329f7)

---------------------------------------------------------------------------------------------------------
## 5.5 Data Transforation from Silver to Gold container | Gold container data is used for BI layer for reporting 
Next Transformation > 
two words in columnameas one word > we can split into two words in column name 
![image](https://github.com/user-attachments/assets/ff2090e7-5615-4823-a401-0012ca48a463)

After Transformation column name is used _ to split two names now as below 
![image](https://github.com/user-attachments/assets/2a193c99-bcb4-44ff-8c0c-c8baef289194)

Transformed data is saved as delta format in gold container from silver container 
![image](https://github.com/user-attachments/assets/58efea8a-ce4c-4d63-8ddd-26d92edcc902)

----------------------------------------------------------------------------------------------
## 6: Azure Data Factory to run Data Bricks notebooks in pipelines
 # We need to build connection from AF to Azure Databricks now..

 Right now our ADF pipeline is like below 
 ![image](https://github.com/user-attachments/assets/8b14ee7c-fe10-49e9-9fd6-39ea23c7a5c0)

 So we need to create link service from ADF to Databricks: 
 ![image](https://github.com/user-attachments/assets/9060d43f-8692-4f0a-9d86-c775ec3613e3)

 We need to create a access token to Databricks 
 ![image](https://github.com/user-attachments/assets/d00906b3-0bbc-4196-91e3-172deaac7a39)

 How to create access token for ADF from Databricks is below :
 ![image](https://github.com/user-attachments/assets/f2bb0642-722a-4801-a01b-cda621584dbc)

 Access token is created 
 ![image](https://github.com/user-attachments/assets/45a3f1f6-a809-4daa-b19c-4900a7820def)

 Then we can add that created token as a seceret in azure key vault 
 ![image](https://github.com/user-attachments/assets/9f4062c3-a573-4624-ad2a-9ccc93f204f7)

 ![image](https://github.com/user-attachments/assets/3e3dc5d1-03d1-4f85-90d1-600386e2a95b)

 Then need to test connection  once it is ok we can craete link service fro ADF to Databricks

 Publish changes into ADF 

 ![image](https://github.com/user-attachments/assets/77128c18-3e46-4365-b1c6-22d6b0f2cd2d)

 ## Now below one is the full pipeline
 ![image](https://github.com/user-attachments/assets/f6effd7b-b2c2-4230-88e6-5fa16b8caf0a)

 ![image](https://github.com/user-attachments/assets/2afb6cd9-5d43-4cc1-9ef8-236aac494f2a)

 ## Now we cna publish chnages into ADF and test pipeline by running trigger 
 



 ![image](https://github.com/user-attachments/assets/b0353dff-5555-40b8-a708-1b450501342b)

 we need to use exsisitng using databrocks compute cluster here > 
 ![image](https://github.com/user-attachments/assets/d1efda66-9551-4408-9cb2-063a62b4e6b2)

### When databricks cluster is inactive we need to create new cluster and need to give new cluster name here to ADF
![image](https://github.com/user-attachments/assets/98f779e0-ee63-41fc-b109-29cff33b5912)
## ---------------------------------------------------------------------------------------------------
******************************************************************************************************
### On-Prem MySQL > Azure Data Factory > Azure Data LAke Gen2 [ Bronze Layer > Silver Layer > Gold Layer ] > Azure DataBricks  Pipeline is successfully Implemented as below image. 

![image](https://github.com/user-attachments/assets/b97c3e71-c5d3-48a7-9d15-8d6701e04121)

******* ---------------------------------------------------------------------------------******
------------------------------------------------------------------------------------------------
## Step 7 : Azure Synapse Analytics 
## Data loading with Azure Synapse analytics 
in created Resource Group > 
![image](https://github.com/user-attachments/assets/e6b286c0-2757-434d-8098-7a6b92a230cb)

Azure Synapse Analytics is build top of ADF 

![image](https://github.com/user-attachments/assets/bbf17ad8-5b6c-431c-8b12-78e9b888fe06)

First we create databse in  Synapse Analytics (SA) via Data tab like below>
## Note: TWo type data bses are there 
1: Serverles = good for small workload , not expensive
2: dedicated = god for large workload , but this is expensive one 

![image](https://github.com/user-attachments/assets/97aee9f7-9a5b-4ba0-834e-2736125a6e21)
Furthermore, we can see SA sql tools , there is default serverless built in sql tool is there.
so we use use it if we use serverless database.
if we use serverless data base , then it can connect data in Gold container in DL and do query , so we need to give compute power for that to database.data is not loaded into  SA data base fro Azure Gen2 DL.So data is at Dl, but SA db can connect it with and do needful quering etc.

But if we use deicated db in SA, then it takes both compute & storage since then Data should load feom gold container to SA Sql dedicated db. 

Here I used serverless SQL db for SA.
![image](https://github.com/user-attachments/assets/918cd992-511b-4358-9fdc-4faef67acee5)

## Created SA serverless sql db naes as = gold_db
![image](https://github.com/user-attachments/assets/55ca00b4-74e2-4690-8bb4-a02351fc8a6e)









