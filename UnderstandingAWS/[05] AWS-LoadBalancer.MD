#### Setting up Elastic Load Balancer for AWS EC2:

##### Lets use the existing [EC2 Instance](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingAWS/%5B01%5D%20AWS-EC2.MD), with a different Security Group.
##### STEP#1: Create a New Security Group:
![image](https://user-images.githubusercontent.com/689226/77218691-07c50a00-6b54-11ea-8fc8-d67b78ac13ce.png)
<br>
<br>
##### Outbound Rules:
![image](https://user-images.githubusercontent.com/689226/77218701-27f4c900-6b54-11ea-961f-278cd90a0f14.png)
<br>
<br>
##### STEP#2: Plug this Security Group with the existing EC2:
![image](https://user-images.githubusercontent.com/689226/77218736-84f07f00-6b54-11ea-8818-632adb293f8a.png)
<br>
<br>
##### STEP#3: Create an Elastic Load Balancer:
![image](https://user-images.githubusercontent.com/689226/77218782-06481180-6b55-11ea-880c-07d207dbf54d.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218790-195ae180-6b55-11ea-902b-acaf57be703b.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218847-d4837a80-6b55-11ea-80eb-4efd7e998f7d.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218853-df3e0f80-6b55-11ea-955d-6cb04b987aaa.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218859-ebc26800-6b55-11ea-998b-39a91390701f.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218873-07c60980-6b56-11ea-85ce-9c7e4eeed96e.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218891-44920080-6b56-11ea-8b28-d8eb25e3702f.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218915-920e6d80-6b56-11ea-85f1-5e8d819066ef.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218926-abafb500-6b56-11ea-9d13-69b3a102052c.png)
<br>
<br>
##### STEP#4: Test and Launch:
![image](https://user-images.githubusercontent.com/689226/77218985-4e683380-6b57-11ea-9b92-00d30a744cd2.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77218998-62ac3080-6b57-11ea-9550-27e7bc439d10.png)


