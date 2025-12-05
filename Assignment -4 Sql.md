##### Assignment -4 

##### 

##### Sql Server : 





Task -1



Write a query using CASE to categorize salary levels on Employees table: 

• <20000 → Low 

• 20000–50000 → Medium 

• 50000 → High 



query : 



select EmpId,EmpName,Salary, 

case

&nbsp;when Salary <20000 then 'Low'

&nbsp;when Salary between 20000 and 50000 then 'Medium'

&nbsp;else 'High'

&nbsp;end as SalaryCategory from Employees



Task -2 



Declare a variable @Age. 

Write logic using IF / ELSE: 

• If Age < 18 → print “Minor” 

• Else If Age between 18–60 → “Adult” 

• Else → “Senior” 



query :



declare @Age int

set @Age=78

if @Age <18 

print 'Minor'

else if @Age between 18 and 60

print 'Adult'

else

print 'Senior'



Task -3





Create an encrypted and schemabound view that: 

• Joins Employees, Departments, and Salaries tables 

• Returns only employees who joined in the last 3 years 

• Includes computed column: AnnualSalary = Salary \* 12 

• Prevents updates to base tables that break schema binding 

Tasks 

1\. Create the view with WITH SCHEMABINDING, ENCRYPTION. 

2\. Try altering an underlying table column → observe the error. 





query :





create view dbo.v2

with encryption, schemabinding  as 

select e.EmpId,e.EmpName,e.JoinDate,  e.Salary, AnnualSalary = e.Salary \* 12,d.DeptId,d.DeptName

&nbsp; from dbo.Employees e inner join dbo.Departments d on e.DeptId=d.DeptId 

&nbsp; 

&nbsp; 

&nbsp;  select \* from dbo.v2 where JoinDate >=dateadd(year,-3,getdate())







Task-4



&nbsp;Complex Multi-Table View 

Create a view that: 

• Joins Employees + Sales 

• Shows total sales per employee 

• Shows rank based on total sales across company 



query :



create view v3 as

select e.EmpId,e.EmpName,

sum(s.SaleAmount) as TotalSales,rank()over(order by sum(SaleAmount) desc) as rank\_1 from 

Employees e inner join Sales s on  e.EmpId = s.EmpId group by e.EmpId,e.EmpName







Task-5— Simulate Error Capture 

Write a block that: 

• Attempts dividing by zero 

• Catches the error 

• Prints error details 



query :





begin try

declare @a int

set @a =10

print @a /0

end try

begin catch

print 'you are trying to divide 0 .. pls check again'

print error\_message()

print error\_line() 

print error\_number() 

print error\_severity();

print error\_procedure()

end catch





Task-6— Nested TRY…CATCH With Custom Error 

Validate salary: 

• If salary < 1000, throw custom error using THROW. 

• Declare variable  to simulate salary 



query :



begin try

declare @Salary int;

set @Salary=100;

if @Salary<1000

&nbsp;throw 500001,'Salary must be at least 1000.', 1;

&nbsp;end try

&nbsp;begin catch

&nbsp;print error\_message();

&nbsp;end catch;







Task-7— Rank Employees by Region Sales 

Task 

• Compare Rank / Dense\_Rank / Row\_Number 

• Identify top 2 per region 





query:



with salesrank as (

&nbsp;   select EmpId, Region, SaleAmount,

&nbsp;          rank() over (partition by region order by  SaleAmount desc) as ranksale\_1,

&nbsp;          dense\_rank() over (partition by region order by  SaleAmount desc) as ranksale\_2,

&nbsp;          row\_number() over (partition by region order by  SaleAmount desc) as ranksale\_3

&nbsp;   from sales

)

select \*from salesrank where ranksale\_1 <= 2









Task-8 -Using Sales table: 

• First CTE: Filter only last 1 year sales 

• Second CTE: Compute total sales per region 

• Third CTE: Rank regions based on total sales 

• Output top 3 regions 

&nbsp;

Task-8 Find Employees With Duplicate SalesAmount in Any Department



query : 



with last\_year\_sales as (

&nbsp;

&nbsp;   select \*

&nbsp;   from Sales

&nbsp;   where SaleDate >= dateadd(year, -1, getdate())

),

region\_totals as (

&nbsp;  

&nbsp;   select Region, sum(SaleAmount) as total\_sales

&nbsp;   from last\_year\_sales

&nbsp;   group by Region

),

ranked\_regions as (

&nbsp;   

&nbsp;   select Region, total\_sales,

&nbsp;          rank() over (order by total\_sales desc) as sales\_rank

&nbsp;   from region\_totals

)



select \*

from ranked\_regions

where sales\_rank <= 3;





Task – 9 

Perform Pagination and list all details from employees who’s page between 6 and 10 





query :



with emp\_cte as (

&nbsp;   select \*, row\_number() over (order by empid) as rn

&nbsp;   from employees

)

select \*

from emp\_cte

where rn between ((6 - 1) \* 10 + 1) and (10 \* 10);



