#### AWS Auutoscalar

##### Create a Security Group
![image](https://user-images.githubusercontent.com/689226/77343383-9a81c680-6d57-11ea-9da8-e68d70bf312e.png)

<hr>

##### Plug into a EC2 instance:
![image](https://user-images.githubusercontent.com/689226/77343587-eb91ba80-6d57-11ea-9b45-7d1a53d7ae24.png)
```
THE EC2 INSTANCE BOOTSTRAP CODE:
```
```
#!/bin/bash
yum install -y httpd
chkconfig httpd on
echo "<html><body>Hello World</body></html>" > /var/www/html/index.html
service httpd start 
```
<hr>

##### Create an Image:
![image](https://user-images.githubusercontent.com/689226/77348418-70340700-6d5f-11ea-9c35-e5ae95a30eed.png)
![image](https://user-images.githubusercontent.com/689226/77343804-4a573400-6d58-11ea-9061-baf156d35ac8.png)
![image](https://user-images.githubusercontent.com/689226/77348351-4e3a8480-6d5f-11ea-8ad1-67186b04595b.png)

<hr>

##### Create a Load Balancer:
![image](https://user-images.githubusercontent.com/689226/77349577-395ef080-6d61-11ea-8ec5-e5640956f066.png)
![image](https://user-images.githubusercontent.com/689226/77350166-1aad2980-6d62-11ea-9001-777818370f2e.png)
![image](https://user-images.githubusercontent.com/689226/77350221-2d276300-6d62-11ea-8ab7-3b5f0af7316a.png)
![image](https://user-images.githubusercontent.com/689226/77350292-46301400-6d62-11ea-9ef5-8a79c168d103.png)
![image](https://user-images.githubusercontent.com/689226/77350663-dd956700-6d62-11ea-8583-9f8f427f81b9.png)
![image](https://user-images.githubusercontent.com/689226/77350892-2baa6a80-6d63-11ea-8241-a6d8ceb5a716.png)
![image](https://user-images.githubusercontent.com/689226/77351133-5b597280-6d63-11ea-82a9-a96b2a3812cc.png)
![image](https://user-images.githubusercontent.com/689226/77351184-6f04d900-6d63-11ea-8521-4bcb34219ca8.png)
###### NOTE: Load Balancer is created but so far there is no Image (with application hosted) attached to it!
<hr>

##### Create a AutoScaling Group:
![image](https://user-images.githubusercontent.com/689226/77351442-cacf6200-6d63-11ea-8e1e-1823d00ac8be.png)
![image](https://user-images.githubusercontent.com/689226/77352486-467dde80-6d65-11ea-9335-45da88311cae.png)

##### [1] Create Launch Configuration:
![image](https://user-images.githubusercontent.com/689226/77352621-79c06d80-6d65-11ea-825d-9d3ef2bd7daf.png)
![image](https://user-images.githubusercontent.com/689226/77352655-8644c600-6d65-11ea-90eb-daf04193fe43.png)
![image](https://user-images.githubusercontent.com/689226/77352917-023f0e00-6d66-11ea-99c8-a4b9affee37d.png)
![image](https://user-images.githubusercontent.com/689226/77352962-1be05580-6d66-11ea-912b-41381194579d.png)
![image](https://user-images.githubusercontent.com/689226/77353250-a628b980-6d66-11ea-9338-0ba0aea5112b.png)
###### NOTE:  AutoScaling Group shall reeceive traffic from Load Balancer

##### [2] Create Auto Scaling Configuration:
![image](https://user-images.githubusercontent.com/689226/77377146-4814ca00-6d98-11ea-8114-6e3382d6c042.png)
![image](https://user-images.githubusercontent.com/689226/77377079-21569380-6d98-11ea-84da-c7f5809716c1.png)
![image](https://user-images.githubusercontent.com/689226/77471807-eb1d2080-6e38-11ea-98bf-9d393ce49344.png)

![image](https://user-images.githubusercontent.com/689226/77377359-d9843c00-6d98-11ea-96ae-81d17a84c18b.png)
![image](https://user-images.githubusercontent.com/689226/77377396-033d6300-6d99-11ea-87b5-689a4df01947.png)
![image](https://user-images.githubusercontent.com/689226/77377471-2d8f2080-6d99-11ea-810b-bae3186d0560.png)
![image](https://user-images.githubusercontent.com/689226/77377456-249e4f00-6d99-11ea-9bd8-1298588156e1.png)
![image](https://user-images.githubusercontent.com/689226/77377569-7b0b8d80-6d99-11ea-8e81-08b47e357cdb.png)
![image](https://user-images.githubusercontent.com/689226/77377686-ddfd2480-6d99-11ea-9649-1457af224d6b.png)



