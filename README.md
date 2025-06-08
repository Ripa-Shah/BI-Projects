SSAS
I’ve a database called it as ContosoDW. By using this database, I’m loading data into SQL Analysis Service from my SQL Server database. Here I’m using visual studio 2022 version. I’ve clicked on table import wizard. After that, I’ve selected SQL Server.
Load data into Analysis Service using SQL Server
 ![image](https://github.com/user-attachments/assets/5eedd0ee-d827-4778-ab5b-a7787650f5a1)
 ![image](https://github.com/user-attachments/assets/2b85f790-0079-432b-81bf-2f475427097e)


 

I’ve selected next. After that, I’ve selected server name and database name.
 
![image](https://github.com/user-attachments/assets/3a0358cc-12b8-44ec-895b-c856d08a7352)

I’ve used service account in impersonation information:
 ![image](https://github.com/user-attachments/assets/9285afe5-0297-4432-9830-c6bbbeec045b)


I’ve chosen first option: select a list of tables and views to chose data to import
 
![image](https://github.com/user-attachments/assets/7273c9d4-1091-47a4-abc2-caf29fb0b2e5)

I click next. I selected DimProduct, DimProductCategory, DimProductSubcategory and FactSales table.
Measure:


After that, I’ve created a measure using following option.
 ![image](https://github.com/user-attachments/assets/d141bffd-412f-4d89-adce-ad72cd4a5bcb)
Now I’m creating measure called it as Sum of total cost using above option:
![image](https://github.com/user-attachments/assets/2e46a575-1754-42ed-a921-3a5a94b1eb6e)
 

Relationships model:
There are two types of model views: (1) Data View (2) Model View
By selecting model view, we can see the diagram, a depiction of the fact and dimension tables. I’m sharing screenshots of the above selected tables.
  
![image](https://github.com/user-attachments/assets/cd9c2cb6-d66a-4c70-9569-6a10ed27ca4f)
By using this diagram, we can edit the relationship. To edit a relationship, I usually double click on the connectors, and it will open a relationship dialog box.

![image](https://github.com/user-attachments/assets/e7bfbad6-906c-44ea-8fa5-5ad2149ca105)


Now switched it back to data view.
Calculated Column:
You can create a calculated column by selecting a new column and add a DAX formula into formula bar i.e. =CALCULATE (sum (Sales[SalesAmount]),Product[color]=’Red’)
=SWITCH(TRUE(), FactSales[UnitPrice]>1000,”A”, FactSales[UnitPrice]>100,”B”,”C”)
I rename column as Price Class.
=CALCULATE(SUM(FactSales[DiscountQuantity]),if(FactSales[DiscountAmount]<>0,TRUE(),FALSE()))
 
![image](https://github.com/user-attachments/assets/7fe1d243-fdff-4c21-8b24-f843ab257ebc)

![image](https://github.com/user-attachments/assets/7cc30b2a-988c-46b4-af67-3f49a62f2928)
 
Hierarchies:

I’ve created hierarchies using diagram view:
![image](https://github.com/user-attachments/assets/3614fd83-cd56-45a1-aea4-7023ac25d05b)
 
=RELATED(DimProductSubcategory[ProductSubcategoryName])
=RELATED(DimProductCategory[ProductCategoryName])
Now I’m planning to build the model using build ->build option
![image](https://github.com/user-attachments/assets/fe1046b2-34ca-464e-9e36-3c0565c7ac0d)
 

Once build is successful, I will check the project properties to check whether it has the right server selected or not
 ![image](https://github.com/user-attachments/assets/02f5856f-4c3d-48fc-bc6a-efc8c5964d2b)


Once I check the project properties and server name, I will focus on login name and its permissions. For that, I need to go to SSMS and login to database engine. In that, I will create NT Service\MSOLAP$SQL login name if that does not exist.
 ![image](https://github.com/user-attachments/assets/b9456842-e4f4-423a-a7dc-74fc3708ed69)


I will give that login name to appropriate permission to access the Analysis Service project since this login name is associated with SQL Server analysis services.
![image](https://github.com/user-attachments/assets/781758bb-7fbe-4407-8154-2b57786ecd2c)


 

After this configuration setting, I will return to visual studio and start to deploy the project.
![image](https://github.com/user-attachments/assets/285515f3-9b72-40a1-9374-e1148c495d9b)

 

Now deployment is successful:
![image](https://github.com/user-attachments/assets/63ee4134-97e0-4b46-9a36-5762e8a5a44e)
 
Now we have deployed the model. Thus, we can see this model in SQL Server management studio. Go to SSMS and connect to SQL Server Analysis service instead of database engine. In Database section, you can see name of your Analysis Service project as a database. 
![image](https://github.com/user-attachments/assets/4e86d60d-093f-4ee5-b8d2-f95f3c91c69a)
 

Now right click on Tabular Project2 and select process Database. It will pop up following dialogue box
![image](https://github.com/user-attachments/assets/1c2ddc77-a0a7-4dea-b93b-a5b729db598e)
 

Choose process full mode. It will finish according to the process it will take to finish.
![image](https://github.com/user-attachments/assets/230feba8-79c7-4ae8-9b2c-d52c2e0a8806)
 

Now open the PowerBI desktop app and click on get data and select analysis service. Select Tabular project2 and select model. It will pop up fact and dimension tables.
 
![image](https://github.com/user-attachments/assets/64023827-be3d-4753-891e-2779a996dd1b)

I created a PowerBI report using above data for Product information
![image](https://github.com/user-attachments/assets/fbb741fa-f37d-438f-ace0-2e54336e70c7)

 
