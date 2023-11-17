


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












