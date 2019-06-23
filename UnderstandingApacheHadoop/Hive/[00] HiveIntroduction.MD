### Experimenting Hive Basic Operations on Ubuntu-18
<hr>

#### List Databases: </br>
```
show databases;
```

#### Create Database: </br>
```
create database test;
```
#### Switch Database: </br>
```
use test;
```
#### Create Table: </br>
```
create table Employee (Id int, Name string, Age int) row format delimited  fields terminated by ',';
```
#### To Describe Hive Table: </br>
```
describe employee;
describe extended employee;
```
_____________________________________________________________________________________________________________________
#### Insert Data into Table [WAY#1]- Insert Query Technique: </br>
```
insert into Employee values(1,'Rahul',30);
```
_____________________________________________________________________________________________________________________
#### Insert Data into Table [WAY#2]- Load Data from Local File: </br>
 ```
 load data local inpath '/home/rahul/MyFolder/employee1.csv' into table test.employee; 
 ```
_____________________________________________________________________________________________________________________
#### Insert Data into Table [WAY#3]- Load Data from HDFS File: </br>
###### [1] Prepare employee2.csv </br>
###### [2] Move employee2.csv in HDFS: 
 ```
 hdfs dfs -put /home/rahul/MyFolder/employee2.csv /hdfsfolder 
 ```
###### [3] Execute:
 ```
 load data inpath '/hdfsfolder/employee2.csv' into table test.employee; 
 ```
###### NOTE: On checking hdfs dfs -ls /hdfsfolder we now don't find employee2.csv there anymore.
_____________________________________________________________________________________________________________________

#### Insert Data into Table [WAY#4]- External Table Approach: </br>
###### [1] Prepare monuments.csv </br>
###### [2] Move the file in HDFS: 
``` 
hdfs dfs -mkdir /hdfsfolder/externalhive </br>
hdfs dfs -put /home/rahul/MyFolder/monuments.txt /hdfsfolder/externalhive 
```
###### [3] Execute: 
 ```
create external table Monuments(id int, Name string, Place string) row format delimited  fields terminated by ','  location '/hdfsfolder/externalhive/'; 
```
_____________________________________________________________________________________________________________________
#### To locate Hive Files:
```
hdfs dfs -ls /user/hive/warehouse 
hdfs dfs -ls /tmp/hive/ 
```
