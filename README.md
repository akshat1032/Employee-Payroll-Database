## Employee-Payroll-Database 

### UC1 Ability to create a payroll service database
---
create database payroll_service;
select database();
use payroll_service;
---

### UC2 Create a employee payroll table in the payroll service database to manage employee payrolls
---
create table employee_payroll(
	ID int unsigned not null auto_increment,
    	NAME varchar(150) not null,
    	SALARY double not null,
    	START date not null,
    	primary key(ID)
     	);
---
### UC3 Insert employee information into table
---
insert into employee_payroll (NAME, SALARY, START) values 
	('Bill', 100000.0, '2018-01-03'),
	('Mark', 200000.0, '2019-11-13'),
	('Charlie', 100000.0, '2020-05-12');
---
### UC4 Retreive employee information from table
---
select * from employee_payroll;
---
### UC5 Retrieve salary information for employee based on name and date range
---
select salary from employee_payroll where name = 'Bill';
select salary from employee_payroll where
	start between cast('2018-01-01' as date) and date(now());
---
### UC6 Add new column to table and update values in the table
---
alter table employee_payroll
	add GENDER char(1)
	after NAME;
update employee_payroll set GENDER = 'M'
	where NAME = 'Bill' or NAME = 'Mark';
update employee_payroll set GENDER = 'F'
	where NAME = 'Charlie';
update employee_payroll set SALARY = 150000
where NAME = 'Charlie';
---
### UC7 Perform addition average find minimum and maximum operation and counter operation
---
select sum(salary) from employee_payroll
	where GENDER = 'M' group by GENDER;
select sum(salary) from employee_payroll
	where GENDER = 'F' group by GENDER;
select avg(salary) from employee_payroll
	where GENDER = 'M' group by GENDER;
select avg(salary) from employee_payroll
	where GENDER = 'F' group by GENDER;
select max(salary) from employee_payroll
	where GENDER = 'M' group by GENDER;
select max(salary) from employee_payroll
	where GENDER = 'F' group by GENDER;
select min(salary) from employee_payroll
	where GENDER = 'M' group by GENDER;
select min(salary) from employee_payroll
	where GENDER = 'F' group by GENDER;
select count(name),gender from employee_payroll
	where GENDER = 'M' group by GENDER;
select count(name),gender from employee_payroll
	where GENDER = 'F' group by GENDER;
---
### UC8 Add new columns address, phone number and department to table
##### Altering the table
---
alter table employee_payroll add PHONE varchar(250) after name;
alter table employee_payroll add DEPARTMENT varchar(250) not null after address;
---
##### setting default value for address field
---
alter table employee_payroll alter address set default 'TBD';
---
##### setting address with default value for all entries
---
update employee_payroll set address = default where name = 'Bill' or name = 'Mark' or name = 'Charlie';
---