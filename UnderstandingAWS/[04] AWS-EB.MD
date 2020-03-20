#### Begining with Elastic Beanstalk:

#### STEP#1: Launch EC2:
![image](https://user-images.githubusercontent.com/689226/77197524-57cbae80-6b0b-11ea-92d1-ea695372246b.png)

#### STEP#2: Prepare deployment.json
```
vi awsspringbootdocker.json
```
```
{
 "AWSEBDockerrunVersion": "1",
 "Image": {
  "Name": "rahulvaish/springbootdocker"
 },
 "Ports": [{
  "ContainerPort": "8080"
 }]
}
```
#### STEP#3: Use awsspringbootdocker.json to Create Application:
![image](https://user-images.githubusercontent.com/689226/77201040-cad82380-6b11-11ea-9286-7d84a056bfe7.png)
<br>
<br>
![image](https://user-images.githubusercontent.com/689226/77201141-fce98580-6b11-11ea-9d4d-43db1fb8788a.png)
<br>
<br>
![image](https://user-images.githubusercontent.com/689226/77201174-0e329200-6b12-11ea-8b0e-3ee56c10ac06.png)
<br>
<br>
![image](https://user-images.githubusercontent.com/689226/77201279-391ce600-6b12-11ea-8ff5-810c3628096d.png)
<br>
<br>
![image](https://user-images.githubusercontent.com/689226/77201584-bcd6d280-6b12-11ea-8f1b-f4924fcddc82.png)
<br>
<br>
#### STEP#4: Test:
![image](https://user-images.githubusercontent.com/689226/77201608-c6603a80-6b12-11ea-9de7-0c37824f8571.png)
