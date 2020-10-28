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
### UC9 Add payment information columns to the table
---
alter table employee_payroll rename column salary to basic_pay;
alter table employee_payroll add deductions double after basic_pay;
alter table employee_payroll add taxable_pay double after deductions;
alter table employee_payroll add tax double after taxable_pay;
alter table employee_payroll add net_pay double after tax;
---
### UC10.1 Ability to make employee as part of two departments
---
update employee_payroll set department = 'Sales' where name = 'Terissa';
insert into employee_payroll (NAME, DEPARTMENT, GENDER, basic_pay, deductions, taxable_pay, tax, net_pay, START) values
    -> ('Terissa','Marketing','F','300000','100000','200000','50000','150000','2018-01-05');
---
### UC11 Implementing ER Diagram to Employee Payroll
##### Create company table
---
create table Company(
    -> Company_Id int not null,
    -> Company_name varchar(300) not null,
    -> primary key(Company_Id))engine=InnoDB;
---
##### Create employee table
---
create table Employee(
    -> ID int unsigned not null auto_increment primary key,
    -> Company_Id int not null,
    -> Name varchar(150) not null,
    -> Address varchar(300),
    -> Gender varchar(1),
    -> Phone varchar(250),
    -> foreign key(Company_Id) references Company(Company_Id))engine=InnoDB;
---
##### Create payroll table
---
create table Payroll(
    -> ID int unsigned not null auto_increment primary key,
    -> foreign key(ID) references Employee(ID),
    -> basic_pay double not null,
    -> deductions double not null,
    -> taxable_pay double not null,
    -> tax double not null,
    -> net_pay double not null)engine=InnoDB;
---
##### Create department table
---
create table Department(
    -> Department_Id int not null primary key,
    -> Department_name varchar(250) not null)engine=InnoDB;
---
##### Create employee-department table
---
create table employee_department(
    -> ID int unsigned not null auto_increment,
    -> foreign key(ID) references Employee(ID),
    -> Department_Id int not null,
    -> foreign key(Department_Id) references Department(Department_Id))engine=InnoDB;
---
### UC12 Checking if queries work after implementing ER Diagram
##### insert values to tables
---
insert into Company values
    -> (1,'Company1'),
    -> (2,'Company2'),
    -> (3,'Company3');
---
---
insert into Employee values
    -> (101,1,'Ram','Kunjvihar','M','956789'),
    -> (102,1,'Shyam','Mirzapur','M','4456789'),
    -> (103,2,'Munna','Jaunpur','M','9586789'),
    -> (104,2,'Golu','Delhi','F','4786789'),
    -> (105,3,'Sharad','Mumbai','M','3986789'),
    -> (106,3,'Lovely','Kashi','F','2586789');
---
---
insert into Payroll values
    -> (101,30000,1000,29000,1000,28000),
    -> (102,32000,2000,30000,2000,28000),
    -> (103,35000,3000,33000,3000,30000),
    -> (104,38000,4000,34000,4000,30000),
    -> (105,40000,5000,35000,4000,31000),
    -> (106,45000,5000,40000,5000,35000);
---
---
insert into Department values
    -> (1001,'Sales'),
    -> (1002,'Marketing'),
    -> (1003,'Purchase'),
    -> (1004,'Research');
---
---
INSERT INTO employee_department VALUES
    -> (101,1001),
    -> (101,1002),
    -> (103,1004),
    -> (104,1004),
    -> (105,1003),
    -> (102,1003),
    -> (106,1001);
---
##### Total salary by gender
---
select sum(payroll.net_pay),employee.gender from employee
    -> left join payroll
    -> on payroll.ID=employee.ID group by employee.gender;
---
##### Average salary by gender
---
select avg(payroll.net_pay),employee.gender from employee
    -> left join payroll
    -> on payroll.ID=employee.ID group by employee.gender;
---
##### Minimum and maximum salary by gender
---
select min(payroll.net_pay),max(payroll.net_pay),employee.gender from employee
    -> left join payroll
    -> on payroll.ID=employee.ID group by employee.gender;
---
##### Count of employees by gender
---
select count(payroll.net_pay),employee.gender from employee
    -> left join payroll
    -> on payroll.ID = employee.Id group by employee.gender;
---