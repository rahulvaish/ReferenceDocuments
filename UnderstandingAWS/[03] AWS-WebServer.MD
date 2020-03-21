#### Setting up Webserver on AWS EC2:

##### Lets use the existing [EC2 Instance](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingAWS/%5B01%5D%20AWS-EC2.MD), with a different Security Group.
##### STEP#1: Create a New Security Group:
![image](https://user-images.githubusercontent.com/689226/77194469-0bca3b00-6b06-11ea-94f6-4c6438e552a9.png)
##### Inbound Rules:
![image](https://user-images.githubusercontent.com/689226/77194671-6499d380-6b06-11ea-9634-4cc2dc3b01a9.png)
##### Outbound Rules:
![image](https://user-images.githubusercontent.com/689226/77194804-a0cd3400-6b06-11ea-8f3d-0a4c4b4da012.png)
##### STEP#2: Plug this Security Group with the existing EC2:
![image](https://user-images.githubusercontent.com/689226/77195161-35379680-6b07-11ea-8552-f10f5fb6ef5c.png)

![image](https://user-images.githubusercontent.com/689226/77195220-48e2fd00-6b07-11ea-937b-28524afe9094.png)
##### STEP#3: Install HTTPD on EC2:
```
sudo su -
yum install httpd
chkconfig httpd on
chkconfig httpd --list
```
![image](https://user-images.githubusercontent.com/689226/77195782-2f8e8080-6b08-11ea-96fd-27102eb01945.png)
##### STEP#4: Prepare a HTML page to host on HTTPD | Start the Service:
```
service httpd start
```
![image](https://user-images.githubusercontent.com/689226/77197035-754c4880-6b0a-11ea-8510-2b14debf0e13.png)
##### STEP#6: Test:
![image](https://user-images.githubusercontent.com/689226/77197187-c1978880-6b0a-11ea-8306-f403d0d2b75a.png)

![image](https://user-images.githubusercontent.com/689226/77197306-f99ecb80-6b0a-11ea-9ed8-532887cfa1e9.png)
