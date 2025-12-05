##### Assignment-5

##### 

##### Functions :





1\. create a function to find the greatest of three numbers



query :

create function fn\_greatestnum(@num1 int , @num2 int,@num3 int)

returns int

as

begin

declare @res int

if @num1 >=@num2 and @num1>=@num3

   set @res =@num1

 

else if @num2>=@num1  and @num2>=@num3

    set @res =@num2

 

 else

    set @res =@num3

    return @res

end



select dbo.fn\_greatestnum(30,40,50)







 

2\. create a function to calculate to discount of 10% on price on all

the products

--1-

select \* from products

create function discount(@dis\_count int)

returns table

as

return

(select pid,pname,price,price-((price\*@dis\_count)/100) as

Discountprice from products)



select \* from discount(10)



--2--

create function discount\_2(@dis\_count int)

returns @res table

(

&nbsp;   pid int,

&nbsp;   pname varchar(30),

&nbsp;   price int,

&nbsp;   discount\_price int

)

as

begin

&nbsp;   insert into @res

&nbsp;   select

&nbsp;       pid,

&nbsp;       pname,

&nbsp;       price,

&nbsp;       price - ((price \* @dis\_count) / 100)

&nbsp;   from products

&nbsp;   return

end





select \* from discount\_2(10)




 

3\. create a function to calculate the discount on price as following

if productname = 'books' then 10%

if productname = toys then 15%

else

no discount

--1--

create function dbo.discount\_3(@p\_name varchar(20),@price int)

returns int

as

begin

declare @res int

if @p\_name='books'

&nbsp;set @res=@price -(@price \* 10/100)

else if @p\_name='pen'

set @res=@price -(@price \* 15/100)

else

set @res=@price

return @res

end

drop function dbo.discount\_3



select  dbo.discount\_3('books',120) as Discount\_price



--2--

select 

&nbsp;   pid,

&nbsp;   pname,

&nbsp;   price,

&nbsp;   dbo.discount\_3(pname, price) AS discount\_price

from products





 

4\. create inline function which accepts number and prints last n

years of orders made from  now.

(pass n as a parameter)

---------------------------task4------------------------

select \* from orders

create view v10 as select \* from orders



create function last\_year(@n int)

returns table

as

return

(select \* from v10 where orderdate >= dateadd(year,-(@n),getdate()))



drop function last\_year

select \* from last\_year(3)





