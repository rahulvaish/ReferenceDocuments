### Experimenting Hive Partitioning on Ubuntu-18
<hr>

## STATIC PARTITIONING: </br>


#### List Databases: </br>
```
show databases;
```
#### Create Database: </br>
```
create database hivepartitiondb;
```
#### Switch Database: </br>
```
use hivepartitiondb;
```
#### Create Partitioned Table: </br>
```
  create table Student (Id int, Name string, Age int) partitioned by (City String, State string) row format delimited  fields terminated by ',';
```

#### Insert Data into Table- Load Data from Local Files: </br>
###### [1] Create student1.csv with only Id,Name,Age as columns.
###### [2] Create student2.csv with only Id,Name,Age as columns.
![image](https://user-images.githubusercontent.com/45539698/59730287-7260b680-925f-11e9-8c80-09118f5a3de4.png)
###### [3] Execute:
```
load data local inpath '/home/rahul/MyFolder/student1.csv' into table hivepartitiondb.student partition(city='Lucknow',state='UP');
```
###### [4] View the Table:
![image](https://user-images.githubusercontent.com/45539698/59730222-2ada2a80-925f-11e9-9377-5a368f95da4f.png)
###### [5] Execute:
```
load data local inpath '/home/rahul/MyFolder/student2.csv' into table hivepartitiondb.student partition(city='Bhopal',state='MP');
```
###### [6] View the Table:
![image](https://user-images.githubusercontent.com/45539698/59730652-ce780a80-9260-11e9-871a-8c4cf2fcd038.png)
###### [7] To Understand, navigate:
![image](https://user-images.githubusercontent.com/45539698/59731033-4561d300-9262-11e9-96a0-c516ce30893b.png)
<br>

## DYNAMIC PARTITIONING: </br>

#### Configure Dynamic Partitioning: </br>
```
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;
```
#### Create Partitioned Table: </br>
```
create table Student1 (Id int, Name string, Age int) partitioned by (City String, State string) row format delimited  fields terminated by ',';
```
#### Test Dynamics: </br>
```
insert into student1 partition(city,state) select id,name,age,city,state from student; ['student' Table was created earlier]
```
###### View the Table:
![image](https://user-images.githubusercontent.com/45539698/59733261-ebb1d680-926a-11e9-863d-0a45cbfce431.png)
###### To Understand, navigate:
![image](https://user-images.githubusercontent.com/45539698/59733683-5dd6eb00-926c-11e9-89a3-1a5486b570c6.png)



