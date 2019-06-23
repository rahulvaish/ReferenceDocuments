### Experimenting HDFS Basic Operations on Ubuntu-18
<hr>

##### List HDFS Folders: </br>
```
hdfs dfs -ls /
```

##### Create one HDFS Folder: </br>
```
 hdfs dfs -mkdir /hdfsfolder
```

##### Put a file in HDFS Folder: </br>
```
 hdfs dfs -put /home/rahul/MyFolder/NormalFile.txt /hdfsfolder
```

##### List the files in HDFS Folder: </br>
```
 hdfs dfs -ls /hdfsfolder
```
##### View the file in HDFS Folder: </br>
```
 hdfs dfs -cat /hdfsfolder/NormalFile.txt
```

##### Remove a file from HDFS Folder: </br>
```
 hdfs dfs -rm  /hdfsfolder/NormalFile.txt
```

##### Remove all files from HDFS Folder: </br>
```
 hdfs dfs -rm -r  /hdfsfolder/*
```

##### Remove HDFS Folder: </br>
```
 hdfs dfs -rmdir /hdfsfolder
```
<hr>

##### We can also navigate out HDFS and its contents here: </br>
![image](https://user-images.githubusercontent.com/45539698/59725059-e1331500-9249-11e9-8175-277c6bf898f0.png)

<hr>
