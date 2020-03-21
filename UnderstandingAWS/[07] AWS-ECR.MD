#### Elastic Container Regitry

![image](https://user-images.githubusercontent.com/689226/77225678-37493600-6b97-11ea-97e6-89e3ff1f97b8.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77228186-a761b700-6bab-11ea-9c86-15259efd0cc7.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77225692-6fe90f80-6b97-11ea-8d8b-ec6e6adb451f.png)
<br>
![image](https://user-images.githubusercontent.com/689226/77225698-79727780-6b97-11ea-9cdc-93c8fe4f6a09.png)
<br>
#### Follow te below steps:
##### STEP#1: SetUp AWSCLI on root:
```
rahul@rahul:~$ sudo su -
root@rahul:~# pip3 install awscli
root@rahul:~# aws --version
```
```
root@rahul:~# aws configure
AWS Access Key ID [******GJK4]: AKIAVFM5VXN6S3USJTHE            
AWS Secret Access Key [******QL8A]: 91O******wsZVbF*********O0B3JvVH*****lU
Default region name [us-east-1]: us-east-1
Default output format [None]:
```
##### STEP#2: Follow the commands from "View Push Commands":
```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 355202349949.dkr.ecr.us-east-1.amazonaws.com/rahulvaishawsrepo
```
![image](https://user-images.githubusercontent.com/689226/77229196-6d47e380-6bb2-11ea-8e27-9111257b524d.png)

```
root@rahul:~# git clone https://github.com/rahulvaish/Docker-Java/
root@rahul:~/Docker-Java# git checkout SpringBootDocker
root@rahul:~/Docker-Java/SpringBootDocker# apt install maven
root@rahul:~/Docker-Java/SpringBootDocker# mvn clean install
```

```
docker build -t rahulvaishawsrepo .
```
```
docker tag rahulvaishawsrepo:latest 355202349949.dkr.ecr.us-east-1.amazonaws.com/rahulvaishawsrepo:latest
```
```
docker push 355202349949.dkr.ecr.us-east-1.amazonaws.com/rahulvaishawsrepo:latest
```
##### STEP#3: Check AWS ECR:

![image](https://user-images.githubusercontent.com/689226/77230077-c403ec00-6bb7-11ea-9d2e-6128a13573e1.png)



