# SUB QUERY :



Assignment -2





1\. display all the records from customers who made a purchase of books



--1--



select \* from customers where custid in (select custid from orders where products='Books')



 

2\. display all the records from customer who made a purchase of books , toys, cd



--2--

select \* from customers where custid in (select custid from orders where products in ('Books','Pen','Bag'))



3\. display the list of customers who never made any purchase



--3--

select \* from customers where custid in (select custid from orders where products is null)



4\. display the second highest age from customers (do not use top keyword)

--4--



select max(age) as secondHighestage from customers where age <(select max(age) from customers)



5\. display the list from orders  where customers stays in bangalore



--5--



select \* from orders where custid in(select custid from customers where caddress='Banglore')





6\. display list of customer who made lowest purchase (in terms of quantity)



--6--



select \* from customers where custid in (select custid from orders where qty in (select min(qty) from orders))





7\. display the list of customer who's age is greater then  ajay's age ( but we dont know ajay's age)



--7--

select \* from customers where custid in (select custid from customers where age  >(select age from customers where custname='Ananya'))



8\. update customer table where custid =100's age  =     custid=200's age



--8--



update customers set age =(select age from customers where custid=102) where custid =100



9\. Display those details who made orders in December of any year



--9--

 select \* from customers where custid in(select  custid  from orders where month(orderdate)=12)





10\. Show all  orders made before first half of the month (before the 16th of the month who does not reside in bangalore).





11\. Display list of customers  from delhi and Bangalore who made purchase of less than 3 product



--11--

select custid, custname, caddress from customers where caddress in ('Delhi', 'Bangalore') and custid in (select custid from orders where qty < 3)



12\. Display list of orders where price is greater than average price



--12--

select custid, custname, caddress from customers where custid in (select custid from orders where price > (select avg(price) from orders))



13\. update orders table increasing  price by 10%  for customers residing in Bangalore and who have purchased books.



--13--

update orders set price = price \* 1.10 where products = 'Books'and custid in (select custid from customers where caddress = 'Bangalore')



14.Display orderdetails in following format



OrderID-Date     Total(price\*qty)

100-1/1/2000         500





select concat(orderid, '-', orderdate) as OrderID\_Date,(price \* qty) as Total from orders

