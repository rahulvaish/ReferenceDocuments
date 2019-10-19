### Docker + MySQL

#### To run MySQL image as container on Docker, follow the below steps:

![dockermysql](https://user-images.githubusercontent.com/689226/62350151-76028100-b51f-11e9-83ba-16c81dae6964.jpeg)

<br>

#### To execute the same, with *Data Persistence*:

docker run --name mysql -d -p 3306:3306 -e MYSQL_DATABASE=dev -e MYSQL_ROOT_PASSWORD=root -v /Users/rvaish/mysqlbackup:/var/lib/mysql/  mysql
###### NOTE: Now if the Container is killed / Image is removed; on re-instantiation (on same volume) the data is not lost!
