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