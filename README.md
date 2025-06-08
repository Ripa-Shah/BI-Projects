SSAS
I’ve a database called it as ContosoDW. By using this database, I’m loading data into SQL Analysis Service from my SQL Server database. Here I’m using visual studio 2022 version. I’ve clicked on table import wizard. After that, I’ve selected SQL Server.
Load data into Analysis Service using SQL Server
 
 

I’ve selected next. After that, I’ve selected server name and database name.
 

I’ve used service account in impersonation information:
 

I’ve chosen first option: select a list of tables and views to chose data to import
 

I click next. I selected DimProduct, DimProductCategory, DimProductSubcategory and FactSales table.
Measure:
After that, I’ve created a measure using following option.
 
Now I’m creating measure called it as Sum of total cost using above option:
 

Relationships model:
There are two types of model views: (1) Data View (2) Model View
By selecting model view, we can see the diagram, a depiction of the fact and dimension tables. I’m sharing screenshots of the above selected tables.
 
By using this diagram, we can edit the relationship. To edit a relationship, I usually double click on the connectors, and it will open a relationship dialog box.
 

Now switched it back to data view.
Calculated Column:
You can create a calculated column by selecting a new column and add a DAX formula into formula bar i.e. =CALCULATE (sum (Sales[SalesAmount]),Product[color]=’Red’)
=SWITCH(TRUE(), FactSales[UnitPrice]>1000,”A”, FactSales[UnitPrice]>100,”B”,”C”)
I rename column as Price Class.
=CALCULATE(SUM(FactSales[DiscountQuantity]),if(FactSales[DiscountAmount]<>0,TRUE(),FALSE()))
 

 
Hierarchies:

I’ve created hierarchies using diagram view:
 
=RELATED(DimProductSubcategory[ProductSubcategoryName])
=RELATED(DimProductCategory[ProductCategoryName])
Now I’m planning to build the model using build ->build option
 

Once build is successful, I will check the project properties to check whether it has the right server selected or not
 

Once I check the project properties and server name, I will focus on login name and its permissions. For that, I need to go to SSMS and login to database engine. In that, I will create NT Service\MSOLAP$SQL login name if that does not exist.
 

I will give that login name to appropriate permission to access the Analysis Service project since this login name is associated with SQL Server analysis services.


 

After this configuration setting, I will return to visual studio and start to deploy the project.

 

Now deployment is successful:
 
Now we have deployed the model. Thus, we can see this model in SQL Server management studio. Go to SSMS and connect to SQL Server Analysis service instead of database engine. In Database section, you can see name of your Analysis Service project as a database. 
 

Now right click on Tabular Project2 and select process Database. It will pop up following dialogue box
 

Choose process full mode. It will finish according to the process it will take to finish.
 

Now open the PowerBI desktop app and click on get data and select analysis service. Select Tabular project2 and select model. It will pop up fact and dimension tables.
 

I created a PowerBI report using above data for Product information
 
