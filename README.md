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