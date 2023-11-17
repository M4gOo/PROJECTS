


# Design a SQL Server migration strategy - Compare on-premises Azure costs

The Total Cost of Ownership (TCO) is one [tool](https://azure.microsoft.com/pt-br/pricing/tco/calculator/) that you can use during a data platform modernization project to assess the cost difference the migration can make.

Open the tool in the webrowser, Under Define your workloads section.

Leave Servers empty

under Databases fill out the specification required

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/7d49fedd-cf11-4ffa-8ea7-3a985d5553db)

under Storage fill out the specification required

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/cffccae2-40b5-497f-82f6-c8b2e8a30c38)

under Networking fill out the specification required, then click Next

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/69c181d7-4b9d-4cbb-a837-96a84768393e)

At the Adjust Assumptions section, adjust the preferred Currency 

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1054ad4b-834f-4085-80e8-ddc98197773a)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9701e8ce-83b9-4c5b-adc0-c62ac76f1525)

Under Software Assurance coverage (provides Azure Hybrid Benefit) enbale both options (Windows Server Software Assurance and SQL Server Software Assurance coverages)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/31458756-c8b9-412e-aebf-4eaf988902e5)

Enable the Geo-redundant storage (GRS)

Under Virtual Machine costs enable "Enable this for the Calculator to not recommend Bs-series virtual machines".
The B-series virtual machines don’t have the memory-to-vCore ratio of 8 that is recommended for SQL Server workloads.

Under electricity costs use this [webpage](https://www.statista.com/statistics/263492/electricity-prices-in-selected-countries/) to help to fill out with a realistic value

Under Storage, IT labor costs and Other assumption fill out with your project budget information. Then, Next.

At View Report Section check all the cost breakdown

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/6a8e0dd1-35ff-41ef-b754-1487d9d5c5c2)

It is possible to change the timeframe, Regions and so on

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/53dc5b04-7965-48a3-9ad2-f07db723a5a2)

In this report it is important to know:
- What is the most significant cost component of on-premises?
- What are the largest costs saving if you decide to migrate to Azure?


# ??????????? precisa dwe tempo para montar o lab ---- Assess SQL Server databases for migration to Azure SQL - Identify compatibility issues 

It is important to perform an assessment of the legacy database and identify any potential compatibility issues or changes that need to be made before migration. Also, review the schema of the database and identify any features or configurations that aren’t supported in Azure SQL Database.

https://learn.microsoft.com/en-us/training/modules/assess-sql-server-databases-for-migration-to-azure-sql/7-exercise-identify-compatibility-issues

https://github.com/MicrosoftLearning/dp-300-database-administrator/blob/master/Instructions/Labs/00-setup-environment.md



# Deploy an Azure SQL Database


Students will configure basic resources needed to deploy an Azure SQL Database with a Virtual Network Endpoint. Connectivity to the SQL Database will be validated using Azure Data Studio from the lab VM.

As a database administrator for AdventureWorks, you will set up a new SQL Database, including a Virtual Network Endpoint to increase and simplify the security of the deployment. Azure Data Studio will be used to evaluate the use of a SQL Notebook for data querying and results retention.



![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b5992933-be8b-4a3a-af62-8a0e43e3fb7b)


south central - virtual network, create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1fbf410a-440a-43eb-8b3a-42731796f7e5)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f601f786-8898-4ecb-a516-db4e61a9438c)


### Provision an Azure SQL Database

sql database service
![image](https://github.com/M4gOo/PROJECTS/assets/57456345/ba3df97a-966e-4426-91b9-b41043e91283)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3f6edfa5-1fb4-4ea1-ae66-a774ce4dd852)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/74359ef4-498c-416c-bd7a-84109b9f214f)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/f8dc42d7-381d-467c-bb20-ab64e618ffec)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/8146389f-b7f4-4c4b-8cba-77a8eac09967)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/390d6c60-bb9c-412a-9704-28ca86bd16bd)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/92dc4736-0e90-4428-acc0-3e1f4fae7d55)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4f274de7-7f6e-4008-9faf-e89afb33d541)

Then create

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/89d43cfe-cc73-44fb-a3e7-9419e89e35c3)


### Enable access to an Azure SQL Database

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c20f8856-8dd2-42dd-bea9-86d94ffdbbb8)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/3769dfda-6f8a-4eed-83e0-f284c12a135f)

scroll down and check the box Allow Azure services and resources to access this server property. Click Save.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/c5c95320-3382-4a5a-adb5-b842624b0827)


### Connect to an Azure SQL Database in Azure Data Studio

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/58eabdf4-b7ed-48af-949d-814f41a09389)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/fe5c8f3a-19a2-4243-824a-f91fe75ef206)

The server name is already know, then connect

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/b900ee99-d858-4939-b09f-2c4a15bffcc2)


You may be asked to add a firewall rule that allows your client IP access to this server. If you are asked to add a firewall rule, click on Add account and login to your Azure account. On Create new firewall rule screen, click OK.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4b10b9dc-7836-4de5-b2fc-a8a59d88213c)

Alternatively, you can manually create a firewall rule for your SQL server on Azure portal by navigating to your SQL server, selecting Networking, and then selecting + Add your client IPv4 address (your IP address)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/4dcfeccd-bca6-4528-a58b-62cb8cf1f2a5)

Back on the Connection sidebar, continue filling out the connection details:

Type the name of the Database, then connect, nao consegui conectar nessa pleura!

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/5271ccf0-af6c-424e-a640-eb370fd4d75a)

Azure Data Studio will connect to the database, and show some basic information about the database, plus a partial list of objects.

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1b44997c-f8cc-48dc-b4a0-8d325185c566)

### Query an Azure SQL Database with a SQL Notebook

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/e8a3ba00-ef27-43d9-8e62-3cb2c98e258f)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/67d593b4-aec1-476a-a22c-0523130dc8a6)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/14bb606d-a97f-4438-bb46-ef280db2c9ff)

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/9bebcc9b-dff8-477a-b5f7-1c52c3ae32b3)

SELECT TOP 10 cust.[CustomerID], 
    cust.[CompanyName], 
    SUM(sohead.[SubTotal]) as OverallOrderSubTotal
FROM [SalesLT].[Customer] cust
    INNER JOIN [SalesLT].[SalesOrderHeader] sohead
         ON sohead.[CustomerID] = cust.[CustomerID]
GROUP BY cust.[CustomerID], cust.[CompanyName]
ORDER BY [OverallOrderSubTotal] DESC

SELECT TOP 10 cat.[Name] AS ProductCategory, 
    SUM(detail.[OrderQty]) AS OrderedQuantity
FROM salesLT.[ProductCategory] cat
   INNER JOIN [SalesLT].[Product] prod
      ON prod.[ProductCategoryID] = cat.[ProductCategoryID]
   INNER JOIN [SalesLT].[SalesOrderDetail] detail
      ON detail.[ProductID] = prod.[ProductID]
GROUP BY cat.[name]
ORDER BY [OrderedQuantity] DESC

![image](https://github.com/M4gOo/PROJECTS/assets/57456345/1a76498f-2b86-4a6c-af36-aa27d8be3b39)
























